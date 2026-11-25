# TASKS.md — Urutan Pengerjaan untuk AI Agent

Kerjakan berurutan. Centang `[x]` setelah selesai dan lulus Definition of Done di `AGENTS.md`. Satu milestone = satu atau beberapa commit kecil.

## M0 — Setup
- [ ] `flutter create marginku` (org sesuai kebutuhan), hapus template counter
- [ ] Tambah dependency: `drift`, `sqlite3_flutter_libs`, `path_provider`, `flutter_riverpod`, `intl`, `file_picker`, `share_plus`, `csv`; dev: `drift_dev`, `build_runner`, `flutter_lints`
- [ ] Buat struktur folder sesuai `AGENTS.md`
- [ ] Token tiga lapis di `lib/core/theme/` (primitives → tokens → component_tokens → app_theme), terang & gelap sesuai `DESIGN_TOKENS.md`
- [ ] Font IBM Plex Sans (aset lokal disarankan), `FontFeature.tabularFigures` untuk uang
- [ ] Skrip/aturan validasi token (`DESIGN_TOKENS.md` §6) masuk ke alur commit/CI
- [ ] `NavigationBar` 3 tab dengan `IndexedStack`, placeholder tiap tab
- [ ] `Intl.defaultLocale = 'id_ID'` + `initializeDateFormatting`

## M1 — Core (murni, tanpa UI)
- [ ] `core/units.dart`: `BaseUnit`, konversi input → satuan dasar
- [ ] `core/money.dart`: `formatRupiah`, `parseNumber` (koma/titik)
- [ ] `core/calc.dart`: `unitPrice`, `recipeTotals`, `roundPrice`, `computePricing`
- [ ] Unit test `TESTING.md` §1 (U, H, P, N, R, C, format uang) semua hijau

## M2 — Database
- [ ] Tabel drift sesuai `DATABASE.md` (CHECK, UNIQUE, FK, index), `foreign_keys = ON`
- [ ] `IngredientRepo`: CRUD, search, cek pemakaian, watch
- [ ] `RecipeRepo`: CRUD + item dalam transaksi, `watchSummaries()` dengan HPP
- [ ] Test database `TESTING.md` §2 (D1–D9)

## M2b — Komponen Dasar (design-system)
- [ ] Implement komponen `COMPONENTS.md`: Button, MoneyField/QtyField, Card, Chip, Segmented, BottomSheet, ResultCard, Badge, EmptyState, ConfirmDialog
- [ ] Semua state (default/pressed/focus/disabled/error) + widget test kontras & font scale 2.0

## M3 — Tab Bahan
- [ ] List + search + empty state
- [ ] Bottom sheet form (konversi satuan input, preview harga satuan live, validasi)
- [ ] Hapus dengan konfirmasi + blokir bila terpakai
- [ ] Snackbar "N resep ikut terhitung ulang" setelah ubah harga
- [ ] Widget test W1–W3

## M4 — Tab Resep
- [ ] List resep (HPP, harga jual, badge "Perlu ditinjau")
- [ ] Editor: field resep, baris bahan, picker bahan, panel ringkasan sticky
- [ ] Simpan transaksional; hapus resep
- [ ] Arahkan ke tab Bahan bila belum ada bahan
- [ ] Widget test W4, W7

## M5 — Tab Harga Jual
- [ ] Sumber HPP (resep/manual), mode persen/nominal, chip pembulatan
- [ ] Kartu hasil live (harga akhir, untung, markup, margin, sebelum pembulatan)
- [ ] "Simpan ke Resep"; tombol "Atur Harga Jual" dari editor resep
- [ ] Widget test W5–W6

## M6 — Backup & Restore
- [ ] `BackupRepo.exportJson/importJson` + validasi + transaksi
- [ ] Layar Backup & Restore, share sheet, file picker, dialog konfirmasi
- [ ] Export CSV (3 file)
- [ ] Test B1–B8
- [ ] (P2) Import CSV

## M7 — Polish & Rilis
- [ ] Pre-Delivery Checklist `DESIGN.md` §8 lulus penuh (dark mode, font scale 200%, 360 dp, reduced motion) (W8)
- [ ] Ikon aplikasi & aset sesuai `BRAND.md` (`flutter_launcher_icons`)
- [ ] (Bila ada web) ikuti `WEB_GUIDANCE.md`; bagian `### Modern Web Guidance` di laporan
- [ ] Empty/error state semua layar, teks Bahasa Indonesia konsisten
- [ ] Ikon aplikasi & nama (`MarginKu`), hapus permission yang tidak perlu (INTERNET)
- [ ] Manual QA (`TESTING.md` §4a), `flutter analyze` + `flutter test` hijau
- [ ] Build release APK/AAB

## Backlog (v1.1+)
Duplikat resep, pengingat backup, satuan `lusin/kodi`, riwayat harga bahan, biaya tenaga kerja per resep.
