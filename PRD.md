# PRD — MarginKu

> Versi 1.0 · Platform: Android (utama), iOS (opsional) · Stack: Flutter · Data: lokal, tanpa login

## 1. Ringkasan
MarginKu adalah aplikasi mobile offline untuk pelaku UMKM (kuliner, kerajinan, jasa produksi kecil) yang menghitung **HPP** dari bahan baku dan membantu menentukan **harga jual** serta **estimasi untung**. Semua data tersimpan di HP; tidak ada akun, tidak ada server. Data bisa di-backup dan di-restore lewat file lokal (JSON/CSV).

## 2. Masalah
- Pelaku UMKM menentukan harga jual dengan feeling, bukan hitungan HPP.
- Harga bahan naik-turun, tetapi HPP tidak pernah dihitung ulang.
- Spreadsheet terlalu ribet di HP; aplikasi lain mewajibkan login/internet.

## 3. Target Pengguna
| Persona | Kebutuhan |
|---|---|
| Pemilik usaha makanan/minuman rumahan | Tahu HPP per porsi dan harga jual yang aman |
| Penjual online (reseller bahan/produk) | Update harga bahan cepat, lihat dampak ke semua produk |
| Pemula tanpa latar belakang akuntansi | UI sederhana, istilah mudah, hasil langsung tampil |

## 4. Tujuan & Metrik Sukses
| Tujuan | Metrik |
|---|---|
| Hitung HPP cepat | Resep 5 bahan selesai dibuat < 3 menit |
| Hasil selalu akurat | Semua test case di `TESTING.md` lulus |
| Aman dipakai offline | 100% fitur jalan tanpa internet |
| Data tidak hilang | Backup → restore menghasilkan data identik |

## 5. Non-Goals (v1)
Login/akun, sinkronisasi cloud, multi-user, kasir/transaksi penjualan, stok & pembelian, laporan keuangan, pajak, multi-mata-uang, konversi satuan antar jenis (gram↔ml).

## 6. Struktur Aplikasi
Bottom navigation 3 tab: **Bahan · Resep · Harga Jual**. Backup/Restore diakses dari ikon menu di app bar (bukan tab ke-4).

## 7. Kebutuhan Fungsional

### 7.1 Tab Bahan
| ID | Kebutuhan | Prioritas |
|---|---|---|
| ING-01 | Tambah/ubah/hapus bahan: nama, satuan dasar (`gram`/`ml`/`pcs`), jumlah beli, harga beli | P0 |
| ING-02 | Tampilkan harga satuan kecil otomatis, mis. `Rp 15 / gram` | P0 |
| ING-03 | Input jumlah boleh dalam `kg`/`liter`; otomatis dikonversi ke `gram`/`ml` saat disimpan | P0 |
| ING-04 | Edit harga/jumlah beli otomatis mengubah HPP semua resep pemakai (HPP dihitung dinamis, tidak disimpan) | P0 |
| ING-05 | Pencarian nama bahan | P0 |
| ING-06 | Bahan yang dipakai resep tidak bisa dihapus; tampilkan daftar resep pemakai | P0 |
| ING-07 | Tampilkan jumlah resep yang memakai bahan di list | P1 |

### 7.2 Tab Resep
| ID | Kebutuhan | Prioritas |
|---|---|---|
| REC-01 | Tambah/ubah/hapus resep: nama, jumlah hasil jadi (`yield_qty`), biaya tambahan (kemasan/overhead) | P0 |
| REC-02 | Tambah bahan ke resep + isi takaran (`qty_used`) dalam satuan dasar bahan | P0 |
| REC-03 | Tampilkan biaya per baris bahan, total biaya bahan, total biaya, dan **HPP per unit** secara live | P0 |
| REC-04 | Satu bahan hanya boleh sekali per resep | P0 |
| REC-05 | Duplikat resep | P1 |
| REC-06 | Pencarian resep; tampilkan HPP per unit di list | P0 |

### 7.3 Tab Harga Jual
| ID | Kebutuhan | Prioritas |
|---|---|---|
| PRC-01 | Sumber HPP: pilih resep **atau** isi HPP manual | P0 |
| PRC-02 | Mode target untung: **persen (markup)** atau **nominal rupiah** | P0 |
| PRC-03 | Pembulatan: kelipatan `Rp 100/500/1.000` atau custom, arah ke atas (default) | P0 |
| PRC-04 | Tampilkan Harga Jual Akhir, Untung per Unit, Persen Untung Aktual (markup) dan margin setelah pembulatan | P0 |
| PRC-05 | Tombol "Simpan ke resep" menyimpan `markup_pct`, `rounding_step`, `selling_price` | P0 |
| PRC-06 | Peringatan jika `selling_price` tersimpan tidak sama dengan hasil hitung HPP terbaru | P1 |

### 7.4 Backup & Restore
| ID | Kebutuhan | Prioritas |
|---|---|---|
| BAK-01 | Export backup JSON lengkap (share sheet / simpan ke file) | P0 |
| BAK-02 | Restore dari JSON dengan validasi + konfirmasi (mode: ganti semua data) | P0 |
| BAK-03 | Export CSV (`ingredients.csv`, `recipes.csv`, `recipe_items.csv`) | P1 |
| BAK-04 | Import CSV (3 file dipilih sekaligus) | P2 |

## 8. Aturan Bisnis
1. Harga satuan = harga beli ÷ jumlah beli (dalam satuan dasar).
2. HPP per unit = (Σ takaran × harga satuan + biaya tambahan) ÷ jumlah hasil jadi.
3. Harga jual awal = HPP × (1 + persen/100); mode nominal: HPP + nominal.
4. Pembulatan ke atas ke kelipatan step agar untung tidak lebih kecil dari target.
5. Untung per unit = harga jual − HPP. Markup aktual = untung ÷ HPP × 100. Margin = untung ÷ harga jual × 100.
6. `jumlah beli`, `yield_qty`, dan `takaran` harus > 0; harga, biaya tambahan, persen ≥ 0.
7. HPP **tidak pernah disimpan**; selalu dihitung dari data terkini.

Detail rumus & edge case: `SPEC.md`.

## 9. Kebutuhan Non-Fungsional
- Offline penuh, tanpa permission internet.
- Hitung ulang HPP < 100 ms untuk 500 bahan & 200 resep.
- Semua teks UI Bahasa Indonesia; format uang `Rp 1.500` (locale `id_ID`).
- Tidak ada analytics, iklan, atau pelacakan.
- Dukungan layar kecil (360 dp), mode terang & gelap (token tiga lapis), font scaling 200%, reduced motion, target sentuh ≥ 48 dp, kontras ≥ 4.5:1.

## 10. Asumsi
- Satu pengguna per perangkat; mata uang Rupiah.
- Satuan dasar hanya `gram`, `ml`, `pcs`.
- Pengguna melakukan backup manual (tidak ada auto-backup di v1).

## 11. Roadmap
- **v1.0**: semua P0 + P1.
- **v1.1**: import CSV, duplikat resep, auto-backup mengingatkan.
- **v2**: satuan custom, biaya tenaga kerja per resep, riwayat harga bahan.

## 12. Dokumen Terkait
`AGENTS.md` · `SPEC.md` · `DATABASE.md` · `BRAND.md` · `DESIGN.md` · `DESIGN_TOKENS.md` · `COMPONENTS.md` · `design-system/marginku/MASTER.md` (+ `pages/`) · `BACKUP.md` · `TESTING.md` · `TASKS.md` · `WEB_GUIDANCE.md`
