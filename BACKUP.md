# BACKUP.md — Format Backup & Restore MarginKu

## 1. JSON (utama, restore penuh)
Nama file: `marginku-backup-YYYYMMDD-HHmm.json`

```json
{
  "app": "MarginKu",
  "schema_version": 1,
  "exported_at": 1764000000000,
  "ingredients": [
    { "id": 1, "name": "Tepung Terigu", "base_unit": "gram", "pack_qty": 1000, "pack_price": 12000, "updated_at": 1764000000000 }
  ],
  "recipes": [
    { "id": 1, "name": "Brownies Kukus", "yield_qty": 10, "extra_cost": 2000, "markup_pct": 30, "rounding_step": 500, "selling_price": 1500, "updated_at": 1764000000000 }
  ],
  "recipe_items": [
    { "id": 1, "recipe_id": 1, "ingredient_id": 1, "qty_used": 500 }
  ]
}
```
- Semua `id` dipertahankan saat restore agar relasi utuh.
- `selling_price` boleh `null`.

### Validasi saat Restore (urut)
1. File dapat dibaca & berupa JSON valid.
2. `app == "MarginKu"`.
3. `schema_version` ≤ versi aplikasi (jika lebih baru → tolak: "Backup dibuat dengan versi aplikasi lebih baru").
4. Semua array ada; tipe & aturan kolom sama dengan `DATABASE.md` (CHECK).
5. Integritas: setiap `recipe_items.recipe_id` dan `ingredient_id` ada di tabel induk; tidak ada duplikat `(recipe_id, ingredient_id)`; nama bahan unik.
6. Tampilkan ringkasan (N bahan, M resep) → konfirmasi pengguna.

### Eksekusi Restore
Dalam **satu transaksi**: hapus `recipe_items` → `recipes` → `ingredients`, lalu insert `ingredients` → `recipes` → `recipe_items`. Reset sequence `sqlite_sequence` ke max id. Gagal di tahap mana pun → rollback, data lama utuh.

Migrasi: backup `schema_version` lebih lama dari aplikasi dimigrasi di memori sebelum insert.

## 2. CSV (export untuk spreadsheet, import P2)
Tiga file, UTF-8 dengan BOM, pemisah `,`, header baris pertama, desimal `.`:

`marginku-ingredients.csv`
```
id,name,base_unit,pack_qty,pack_price,updated_at
```
`marginku-recipes.csv`
```
id,name,yield_qty,extra_cost,markup_pct,rounding_step,selling_price,updated_at
```
`marginku-recipe_items.csv`
```
id,recipe_id,ingredient_id,qty_used
```
- Sel yang mengandung koma/kutip/baris baru di-escape sesuai RFC 4180.
- `selling_price` kosong = null.
- Import CSV (jika dikerjakan): pilih ketiga file sekaligus, validasi sama dengan JSON, mode "ganti semua data".

## 3. Alur Pengguna
- **Backup**: Menu → Backup & Restore → "Simpan / Bagikan Backup" → file dibuat di cache lalu dibuka lewat share sheet (`share_plus`) / dialog simpan.
- **Restore**: Menu → Backup & Restore → "Restore" → `file_picker` (`.json`/`.csv`) → validasi → ringkasan → konfirmasi → sukses/gagal.

## 4. Implementasi
- `BackupRepo.exportJson() -> String`, `BackupRepo.importJson(String)` (melempar `BackupException(pesan)` bila validasi gagal).
- `BackupRepo.exportCsv() -> Map<String, String>`.
- Penulisan file di `core`/`features/backup/backup_service.dart`; repository tetap bebas dari `dart:io`.
- Test: round-trip export → import ke DB kosong menghasilkan data identik (lihat `TESTING.md`).
