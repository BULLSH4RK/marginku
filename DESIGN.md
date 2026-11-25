# DESIGN.md — UI/UX MarginKu

Gaya: modern-minimalis, fokus angka. Material 3, satu aksen warna, banyak ruang kosong, angka besar dan jelas.

## 1. Dasar Visual (ui-ux-pro-max + design-system)
- **Gaya:** Minimalism & Swiss Style (hasil `--design-system`, cocok untuk alat finansial/profesional). **Dial:** variance 3 (terpusat/minimal), motion 2 (halus), density 5 (standar).
- **Pola landing "Trust & Authority" tidak dipakai** (bukan landing page); pola app = 3 tab di §2–§3.
- **Token:** tiga lapis Primitive → Semantic → Component di `DESIGN_TOKENS.md`. Master + override: `design-system/marginku/MASTER.md` dan `pages/*.md` (override menang atas MASTER).
- **Komponen & state:** `COMPONENTS.md`. **Merek/suara:** `BRAND.md`.
- **Font:** IBM Plex Sans (angka `tabularFigures`). Skala spacing 4 dp, radius 8/12/16/24, target sentuh ≥ 48 dp.
- **Warna:** navy `#0F172A` (primary), emas `#A16207` (aksen kecil), hijau/amber/merah untuk status; nilai terang & gelap ada di `DESIGN_TOKENS.md` §2–§3.
- **Anti-pattern (hindari):** gaya main-main, gradien ungu/pink, emoji sebagai ikon, hover-translate, warna keras di widget, warna sebagai satu-satunya penanda.

## 2. Navigasi
`NavigationBar` bawah, 3 item:
| Tab | Ikon | Label |
|---|---|---|
| 1 | `inventory_2_outlined` | Bahan |
| 2 | `receipt_long_outlined` | Resep |
| 3 | `sell_outlined` | Harga Jual |

App bar tiap tab: judul + ikon `more_vert` → menu "Backup & Restore", "Tentang".

## 3. Layar

### 3.1 Bahan
```
[ AppBar: Bahan                      ⋮ ]
[ 🔍 Cari bahan...                     ]
┌────────────────────────────────────┐
│ Tepung Terigu          Rp 12 / gram │
│ Beli 1.000 gram · Rp 12.000         │
│ Dipakai 3 resep                      │
└────────────────────────────────────┘
                              (+ FAB)
```
- Tap kartu → bottom sheet edit. Geser kiri → hapus (dengan konfirmasi).
- Form: Nama · Satuan dasar (segmented gram/ml/pcs) · Jumlah beli + dropdown satuan input (gram/kg bila gram, ml/liter bila ml) · Harga beli.
- Preview di bawah form: `Harga satuan: Rp 15 / gram` (update live).
- Empty state: ikon + "Belum ada bahan" + tombol "Tambah Bahan Pertama".

### 3.2 Resep
- List kartu: nama, "Hasil 10 pcs", `HPP Rp 1.100 / pcs`, chip kecil `Harga jual Rp 1.500` bila ada, badge `Perlu ditinjau` (warning) bila tidak sinkron.
- Editor (layar penuh):
  1. Nama resep, Hasil jadi (+ label "porsi/unit"), Biaya tambahan.
  2. Section "Bahan" → baris: nama bahan · field takaran dengan suffix satuan · biaya baris (kanan, abu-abu). Geser untuk hapus. Tombol "+ Tambah bahan".
  3. Panel ringkasan sticky di bawah (kartu): Biaya bahan · Biaya tambahan · Total · **HPP per unit** (besar, `primary`).
  4. Tombol "Simpan" di app bar. Tombol "Atur Harga Jual" → pindah ke tab Harga Jual dengan resep terpilih.
- Empty state resep: "Belum ada resep. Tambahkan bahan dulu, lalu buat resep."
- Jika belum ada bahan: tombol tambah resep mengarahkan ke tab Bahan dengan snackbar.

### 3.3 Harga Jual
```
Sumber HPP    [ Dari Resep | HPP Manual ]
Resep         [ Brownies Kukus        ▾ ]
HPP per unit  Rp 1.100

Target untung [ Persen | Nominal ]
              [ 30        %  ]

Pembulatan    (Tanpa)(100)(●500)(1.000)(Custom)

┌──────────────────────────────────┐
│ Harga Jual Akhir                  │
│ Rp 1.500                  (32/700)│
│ Sebelum pembulatan: Rp 1.430      │
│──────────────────────────────────│
│ Untung / unit   Rp 400  (hijau)   │
│ Markup aktual   36,4%             │
│ Margin          26,7%             │
└──────────────────────────────────┘
[ Simpan ke Resep ]
```
- Hasil update live saat mengetik (debounce ≤ 150 ms).
- HPP 0 → tampil kartu info "Isi HPP atau pilih resep dulu".

### 3.4 Backup & Restore (halaman dari menu)
- Kartu "Backup (JSON)" → tombol "Simpan / Bagikan Backup".
- Kartu "Export CSV".
- Kartu "Restore" → pilih file → dialog ringkasan (jumlah bahan/resep) + peringatan "Semua data saat ini akan diganti" → tombol "Ganti Semua Data".
- Tampilkan "Backup terakhir: tanggal" (simpan di shared_preferences atau lewat nama file; opsional).

## 4. Komponen Reusable
`MoneyText`, `MoneyField`, `QtyField(suffix)`, `UnitSegmented`, `ResultCard`, `SectionHeader`, `EmptyState`, `ConfirmDialog`, `IngredientPickerSheet`.

## 5. Teks & Nada
Bahasa Indonesia santai-sopan, singkat. Gunakan "HPP (Harga Pokok Penjualan)" pada tooltip pertama kali. Hindari jargon akuntansi: pakai "Untung per unit", bukan "Laba kotor".

## 6. State & Umpan Balik
- Loading: skeleton/indikator kecil (DB lokal cepat, jangan blokir layar).
- Sukses: snackbar singkat. Error: snackbar merah + pesan jelas.
- Form: validasi saat kehilangan fokus + saat simpan; tombol simpan nonaktif bila tidak valid.
- Keyboard numerik untuk semua field angka; `textInputAction: next`.

## 7. Aksesibilitas
Semantics label pada ikon, dukung font scaling 200%, jangan mengandalkan warna saja (untung negatif diberi tanda `−` dan ikon).

## 8. Pre-Delivery Checklist (wajib sebelum tugas UI dianggap selesai)
Dari `ui-ux-pro-max` (references/pro-rules.md, scoped app UI):
- [ ] Tanpa emoji sebagai ikon; satu keluarga ikon outline konsisten
- [ ] Semua elemen tappable punya pressed feedback tanpa menggeser layout; target ≥ 48 dp
- [ ] Kontras teks ≥ 4.5:1 di **terang dan gelap** (diuji terpisah); border/status terbedakan di kedua mode
- [ ] Tidak ada warna/spacing/radius keras (jalankan validasi token `DESIGN_TOKENS.md` §6)
- [ ] Safe area dihormati (nav bawah, panel HPP sticky, CTA); konten scroll tidak tertutup bar/keyboard
- [ ] Uji 360 dp, landscape, tablet, **font scale 200%**, reduced motion
- [ ] Form: label terlihat, error dekat field, helper text, `Form`+`TextEditingController`
- [ ] Ikon bermakna berlabel; ikon dekoratif disembunyikan dari screen reader; urutan fokus = urutan visual
- [ ] Alternatif non-gesture untuk swipe/drag; tidak mengandalkan warna saja
- [ ] Animasi 150–250 ms, bermakna, hormati reduced motion

Prototipe acuan perilaku: `marginku-prototype.html` (aturan web: `WEB_GUIDANCE.md`).
