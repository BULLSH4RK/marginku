# DATABASE.md — Skema Lokal MarginKu

Engine: SQLite via drift. Nama file: `marginku.sqlite`. `PRAGMA foreign_keys = ON` wajib di `beforeOpen`. Versi skema awal: `1` (`schemaVersion = 1`).

## ERD
```
ingredients 1 ──< recipe_items >── 1 recipes
```

## DDL Referensi
```sql
CREATE TABLE ingredients (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  name        TEXT    NOT NULL COLLATE NOCASE UNIQUE,
  base_unit   TEXT    NOT NULL CHECK (base_unit IN ('gram','ml','pcs')),
  pack_qty    REAL    NOT NULL CHECK (pack_qty > 0),
  pack_price  REAL    NOT NULL CHECK (pack_price >= 0),
  updated_at  INTEGER NOT NULL            -- epoch milliseconds
);

CREATE TABLE recipes (
  id             INTEGER PRIMARY KEY AUTOINCREMENT,
  name           TEXT    NOT NULL,
  yield_qty      REAL    NOT NULL DEFAULT 1 CHECK (yield_qty > 0),
  extra_cost     REAL    NOT NULL DEFAULT 0 CHECK (extra_cost >= 0),
  markup_pct     REAL    NOT NULL DEFAULT 0 CHECK (markup_pct >= 0),
  rounding_step  INTEGER NOT NULL DEFAULT 500 CHECK (rounding_step >= 0),
  selling_price  REAL,                    -- nullable
  updated_at     INTEGER NOT NULL
);

CREATE TABLE recipe_items (
  id             INTEGER PRIMARY KEY AUTOINCREMENT,
  recipe_id      INTEGER NOT NULL REFERENCES recipes(id)     ON DELETE CASCADE,
  ingredient_id  INTEGER NOT NULL REFERENCES ingredients(id) ON DELETE RESTRICT,
  qty_used       REAL    NOT NULL CHECK (qty_used > 0),
  UNIQUE (recipe_id, ingredient_id)
);

CREATE INDEX idx_recipe_items_recipe     ON recipe_items(recipe_id);
CREATE INDEX idx_recipe_items_ingredient ON recipe_items(ingredient_id);
```

## Keterangan Kolom
| Tabel.kolom | Catatan |
|---|---|
| `ingredients.base_unit` | Enum string: `gram` \| `ml` \| `pcs`; di Dart: `enum BaseUnit` + TypeConverter |
| `ingredients.pack_qty` | Selalu dalam satuan dasar (hasil konversi kg/liter sudah diterapkan) |
| `ingredients.updated_at` | Diset ulang setiap edit (epoch ms, `DateTime.now().millisecondsSinceEpoch`) |
| `recipes.rounding_step` | `0` = tanpa pembulatan |
| `recipes.selling_price` | Snapshot harga jual tersimpan; `NULL` bila belum ditentukan |
| `recipe_items.qty_used` | Dalam satuan dasar bahan |

## Aturan Integritas
- Hapus resep → `recipe_items` ikut terhapus (CASCADE).
- Hapus bahan yang dipakai → ditolak DB (RESTRICT). Repository mengecek lebih dulu dan mengembalikan daftar resep pemakai untuk pesan UI.
- Simpan resep dengan item = transaksi tunggal (hapus item lama, insert item baru, update resep).
- Ubah harga bahan hanya `UPDATE ingredients`; tidak ada tabel turunan yang perlu disentuh.

## Query Kunci
HPP semua resep (satu query):
```sql
SELECT r.id, r.name, r.yield_qty, r.selling_price,
       (COALESCE(SUM(ri.qty_used * i.pack_price / i.pack_qty), 0) + r.extra_cost) / r.yield_qty AS hpp,
       COUNT(ri.id) AS item_count
FROM recipes r
LEFT JOIN recipe_items ri ON ri.recipe_id = r.id
LEFT JOIN ingredients  i  ON i.id = ri.ingredient_id
GROUP BY r.id
ORDER BY r.name COLLATE NOCASE;
```
Resep pemakai suatu bahan:
```sql
SELECT r.id, r.name FROM recipes r
JOIN recipe_items ri ON ri.recipe_id = r.id
WHERE ri.ingredient_id = ?;
```
> Hasil query HPP harus sama dengan `core/calc.dart`. Test membandingkan keduanya.

## Migrasi
- `schemaVersion` dimulai 1. Setiap perubahan: tambah langkah di `MigrationStrategy.onUpgrade`, uji upgrade dari versi sebelumnya, perbarui `BACKUP.md` (`schema_version`).
