# TESTING.md — Rencana & Test Case MarginKu

Semua test case di bawah **wajib** ada sebagai unit test otomatis. Toleransi angka: `closeTo(x, 1e-9)` kecuali dinyatakan lain.

## 1. Unit Test — `core/calc.dart`

### 1.1 Harga Satuan
| # | pack_price | pack_qty | Hasil |
|---|---|---|---|
| U1 | 15000 | 1000 | 15 |
| U2 | 20000 | 1000 | 20 |
| U3 | 12500 | 12 | 1041,6667 (toleransi 1e-4) |
| U4 | 0 | 500 | 0 |

### 1.2 HPP per Unit
Data: Tepung 500 gram @ Rp 12/gram, Gula 200 gram @ Rp 15/gram, biaya tambahan 2000, yield 10.
| # | Kasus | HPP |
|---|---|---|
| H1 | (500×12 + 200×15 + 2000) / 10 | **1100** |
| H2 | Gula naik menjadi Rp 18/gram | (6000 + 3600 + 2000) / 10 = **1160** |
| H3 | Tanpa bahan, extra 2000, yield 4 | 500 |
| H4 | yield 1, tanpa extra | = total biaya bahan |
| H5 | yield 0 | lempar `ArgumentError` / validasi gagal |

### 1.3 Harga Jual — Mode Persen
| # | HPP | markup % | step | Harga awal | Harga jual | Untung | Markup aktual | Margin |
|---|---|---|---|---|---|---|---|---|
| P1 | 1100 | 30 | 500 | 1430 | **1500** | 400 | 36,36% | 26,67% |
| P2 | 1000 | 50 | 500 | 1500 | **1500** | 500 | 50% | 33,33% |
| P3 | 1000 | 10 | 100 | 1100 | **1100** | 100 | 10% | 9,09% |
| P4 | 1000 | 0 | 500 | 1000 | **1000** | 0 | 0% | 0% |
| P5 | 1100 | 30 | 0 | 1430 | **1430** | 330 | 30% | 23,08% |
| P6 | 1234 | 25 | 1000 | 1542,5 | **2000** | 766 | 62,07% | 38,3% |

> **P3 wajib lulus**: `1000 * 1.1 = 1100.0000000000002` — tanpa normalisasi hasilnya salah (1200).

### 1.4 Harga Jual — Mode Nominal
| # | HPP | nominal | step | Harga jual | Untung |
|---|---|---|---|---|---|
| N1 | 1100 | 1000 | 500 | **2500** (2100 dibulatkan naik) | 1400 |
| N2 | 1100 | 400 | 500 | **1500** | 400 |
| N3 | 1100 | 0 | 500 | **1500** (1100 dibulatkan naik) | 400 |

### 1.5 Pembulatan `roundPrice`
| # | raw | step | mode | Hasil |
|---|---|---|---|---|
| R1 | 1430 | 500 | up | 1500 |
| R2 | 1500 | 500 | up | 1500 |
| R3 | 1501 | 500 | up | 2000 |
| R4 | 1749 | 500 | nearest | 1500 |
| R5 | 1750 | 500 | nearest | 2000 |
| R6 | 1430 | 0 | up | 1430 |
| R7 | 0 | 500 | up | 0 |

### 1.6 Edge Case Persentase
- HPP = 0 → `markupPct == null`, UI tampil "-".
- Harga jual = 0 → `marginPct == null`.
- Untung negatif (nominal menghasilkan harga < HPP tidak mungkin pada mode persen; tetap uji fungsi dengan harga manual) → tampil `−`.

### 1.7 Konversi Satuan
| # | Input | Hasil (satuan dasar) |
|---|---|---|
| C1 | 2 kg | 2000 gram |
| C2 | 0,5 liter | 500 ml |
| C3 | 750 gram | 750 gram |
| C4 | 1 lusin | 12 pcs (jika fitur aktif) |

### 1.8 Format Uang
| Nilai | Tampil |
|---|---|
| 1500 | `Rp 1.500` |
| 1234567 | `Rp 1.234.567` |
| 15 (harga satuan) | `Rp 15` |
| 15.5 (harga satuan) | `Rp 15,5` |
| 0 | `Rp 0` |

## 2. Test Database (drift in-memory)
- D1: insert bahan nama sama beda huruf besar/kecil → gagal (unik NOCASE).
- D2: insert `pack_qty = 0` → gagal CHECK.
- D3: hapus resep → `recipe_items` ikut hilang.
- D4: hapus bahan yang dipakai → ditolak; repository mengembalikan daftar resep pemakai.
- D5: insert dua `recipe_items` dengan `(recipe_id, ingredient_id)` sama → gagal.
- D6: **Propagasi harga**: buat resep H1, ubah harga gula (15 → 18) → stream HPP resep emit 1160 tanpa menyimpan ulang resep.
- D7: query SQL HPP (`DATABASE.md`) = hasil `core/calc.dart` untuk 20 resep acak (property-style test).
- D8: simpan resep + item gagal di tengah (item invalid) → rollback, data resep lama utuh.
- D9: `foreign_keys` aktif (insert item dengan `recipe_id` tidak ada → gagal).

## 3. Test Backup
- B1: round-trip: seed data → `exportJson()` → DB kosong → `importJson()` → semua baris & id identik.
- B2: `app != "MarginKu"` → `BackupException`.
- B3: `schema_version` lebih baru → ditolak.
- B4: item menunjuk `ingredient_id` tidak ada → ditolak, DB lama utuh.
- B5: JSON rusak/bukan JSON → ditolak dengan pesan ramah.
- B6: `selling_price: null` tetap null setelah restore.
- B7: CSV export: header sesuai `BACKUP.md`, nilai dengan koma dalam nama di-escape benar.
- B8: setelah restore, insert baris baru mendapat `id` > max id (sequence benar).

## 4. Widget Test
- W1: Tab Bahan kosong → tampil empty state; tambah bahan → muncul di list dengan `Rp X / satuan` benar.
- W2: Form bahan: preview harga satuan berubah live saat mengetik.
- W3: Hapus bahan terpakai → dialog blokir dengan nama resep.
- W4: Editor resep: tambah baris bahan → panel HPP berubah; takaran 0 → error dan tombol simpan nonaktif.
- W5: Tab Harga Jual: pilih resep H1, markup 30, step 500 → kartu hasil menampilkan `Rp 1.500`, `Rp 400`, `36,4%`.
- W6: Ganti mode ke Nominal → hasil dihitung ulang.
- W7: Badge "Perlu ditinjau" muncul setelah harga bahan naik pada resep yang sudah punya `selling_price`.
- W8: Dark mode: tidak ada overflow pada layar 360×640 dan font scale 2.0.

## 4a. Manual QA (sebelum rilis)
Buat 5 bahan + 3 resep; ubah harga bahan bersama; backup → hapus data/instal ulang → restore; cek angka sama. Uji tanpa internet (mode pesawat).

## 5. Kriteria Lulus
`flutter analyze` bersih, `flutter test` hijau, cakupan `core/` ≥ 95%.
