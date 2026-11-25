# AGENTS.md — Panduan untuk AI Agent (MarginKu)

Baca file ini **pertama**, lalu: `PRD.md` → `SPEC.md` → `DATABASE.md` → `BRAND.md` → `DESIGN.md` → `DESIGN_TOKENS.md` → `COMPONENTS.md` → `design-system/marginku/MASTER.md` (+ `pages/*.md`) → `BACKUP.md` → `TESTING.md` → `TASKS.md` → `WEB_GUIDANCE.md` (hanya bila menyentuh HTML/CSS/JS). Kerjakan tugas sesuai urutan di `TASKS.md`.

## Gambaran Proyek
MarginKu: aplikasi Flutter offline untuk hitung HPP & harga jual UMKM. 3 tab: Bahan, Resep, Harga Jual. Tanpa login, DB lokal SQLite, backup/restore JSON/CSV.

## Skills yang Wajib Dipakai
| Skill | Kapan | Cara |
|---|---|---|
| `ui-ux-pro-max` | Setiap tugas yang mengubah tampilan/interaksi UI | Cari panduan dulu: `python search.py "<2–5 kata>" --domain ux` dan `--stack flutter`; verifikasi hasil cocok untuk **mobile Flutter**; jalankan Pre-Delivery Checklist di `DESIGN.md` §8 sebelum selesai |
| `ui-ux-pro-max:design-system` | Membuat/mengubah token & komponen | Ikuti token tiga lapis (`DESIGN_TOKENS.md`) dan spesifikasi state (`COMPONENTS.md`); jalankan validasi token (`DESIGN_TOKENS.md` §6) |
| `ui-ux-pro-max:design` | Merek, ikon aplikasi, aset toko | Ikuti `BRAND.md`; logo/ikon latar putih untuk aset pratinjau; ikon UI satu keluarga outline |
| `modern-web-guidance` | **Hanya** kode HTML/CSS/JS (prototipe, `web/index.html`, halaman web) | `search` → `retrieve` sebelum menulis kode; detail di `WEB_GUIDANCE.md`. **Jangan** dipakai untuk widget Flutter/Dart |

Hirarki rujukan desain: `design-system/marginku/pages/<layar>.md` > `design-system/marginku/MASTER.md` > `DESIGN.md` > `COMPONENTS.md`/`DESIGN_TOKENS.md` (token tetap sumber nilai). Jika hasil pencarian skill bertentangan dengan dokumen ini, dokumen ini menang; catat selisihnya di `TASKS.md`.

## Tech Stack (wajib)
- Flutter stable terbaru, Dart null-safety, `flutter_lints`
- DB: `drift` + `sqlite3_flutter_libs` (+ `path_provider`)
- State: `flutter_riverpod`
- Format: `intl` (locale `id_ID`)
- File: `file_picker`, `share_plus`, `csv`
- Font: `google_fonts` (IBM Plex Sans) atau aset font lokal (disarankan, tetap offline)
- Test: `flutter_test` (unit + widget), drift in-memory DB untuk test
- Tanpa Firebase, tanpa network, tanpa analytics.

## Struktur Folder
```
lib/
  main.dart
  app/            # MaterialApp, theme, bottom nav (IndexedStack)
  core/
    theme/        # primitives.dart, tokens.dart, component_tokens.dart, app_theme.dart
    money.dart    # formatRupiah, parse input uang
    units.dart    # BaseUnit enum, konversi kg->gram, l->ml
    calc.dart     # fungsi murni: unitPrice, recipeHpp, sellingPrice, rounding
  data/
    db/           # drift tables, database, migrations
    repositories/ # IngredientRepo, RecipeRepo, BackupRepo
  features/
    ingredients/  # list, form, providers
    recipes/      # list, editor, providers
    pricing/      # calculator screen, providers
    backup/       # export/import UI & service
test/
  core/ data/ features/
```

## Aturan Wajib
1. **Logika hitung hanya di `core/calc.dart`** sebagai fungsi murni (tanpa Flutter/DB). UI dan repository memanggilnya.
2. **HPP tidak disimpan di DB.** Hitung dari data terkini lewat query/stream.
3. Rumus & pembulatan harus identik dengan `SPEC.md` §2–§4. Semua test di `TESTING.md` harus ada dan lulus.
4. Cegah error floating point: bulatkan ke 6 desimal sebelum `ceil` (lihat `SPEC.md` §4).
5. Skema DB mengikuti `DATABASE.md` persis (nama kolom snake_case). Perubahan skema = naikkan `schemaVersion` + migrasi + update `DATABASE.md` & `BACKUP.md`.
6. Semua teks UI Bahasa Indonesia; format uang lewat `formatRupiah()`.
7. Ikuti `DESIGN.md`, `DESIGN_TOKENS.md`, dan `COMPONENTS.md`. Widget hanya memakai token semantic/component (tanpa `Color(0x...)`, spacing, radius keras di `lib/features`). Jangan menambah tab ke-4.
8. Validasi input di form **dan** di repository.
9. Operasi multi-tabel (restore, simpan resep + item) dalam satu transaksi.
10. Jangan menambah dependency di luar daftar tanpa alasan tertulis di PR/commit.

## Konvensi Kode
- Nama file `snake_case.dart`, kelas `PascalCase`, kolom DB `snake_case`.
- Widget kecil dan reusable; hindari file > 300 baris.
- Tidak ada angka/teks "ajaib": konstanta di `core/constants.dart`.
- Commit kecil per tugas: `feat(ingredients): add form validation`.

## Perintah
```bash
flutter pub get
dart run build_runner build --delete-conflicting-outputs   # drift codegen
flutter analyze
flutter test
flutter run
```

## Definition of Done (per tugas)
- [ ] `flutter analyze` bersih
- [ ] Test terkait ditulis dan lulus
- [ ] Sesuai `DESIGN.md` (empty state, error state, validasi) + Pre-Delivery Checklist §8 lulus
- [ ] Validasi token bersih (`DESIGN_TOKENS.md` §6)
- [ ] Centang tugas di `TASKS.md`

## Jangan Dilakukan
- Menambah login/cloud/sync.
- Menyimpan HPP atau harga satuan sebagai kolom turunan.
- Mengubah rumus tanpa memperbarui `SPEC.md` dan `TESTING.md`.
- Menghapus bahan yang masih dipakai resep.
