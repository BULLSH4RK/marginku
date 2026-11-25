# Neo-Brutalist UI Components Specification

## Buttons
Semua tombol memiliki border hitam tegas (`#000000`), tinggi/area klik minimal `44px`, dan feedback aktif/hover yang jelas (seperti pergeseran offset shadow atau perubahan warna solid).
- **Filled (Primary):** Latar `Accent Teal` (`#00C2CB`), border hitam tebal, teks kontras.
- **Outline (Primary / Secondary):** Latar `White` (`#FFFFFF`) atau `Paper Beige` (`#F5F5DC`), border hitam tebal, teks hitam.
- **Text (Tertiary):** Tanpa kotak penuh / tampilan minimalis, teks jelas, tetap menjaga `:focus-visible` dan area klik yang layak.
- **Disabled:** Tampilan redup/non-aktif (`opacity` rendah atau warna netral muted), `cursor-not-allowed`, tanpa aksi.
- **Destructive (Danger):** Menggunakan aksen `Accent Magenta` (`#FF00FF`) atau border/fill peringatan dengan teks/ikon kontras tinggi.
- **Icon (Only):** Tombol khusus ikon (misalnya `[X]` atau `[+]`) dengan border hitam tegas dan ukuran minimal `44px x 44px`.

## Inputs & Form Controls
- **Text Input:** Latar `White` (`#FFFFFF`), border hitam tebal (`2px`–`3px`), sudut tajam/minimal, padding nyaman, teks `18px` atau `14px` sesuai kebutuhan form.
- **Error State:** Border/aksen merah atau `Accent Magenta` (`#FF00FF`) disertai pesan error yang jelas (`ERROR MESSAGE!`).
- **Helper Text:** Teks keterangan singkat di bawah input (`Caption` `14px`).
- **Controls:**
  - **Checkbox & Radio:** Kotak/lingkaran berborder hitam tebal; saat aktif menampilkan tanda centang (`✓`) atau titik/isi solid dengan aksen warna (`Black` / `Accent Teal` / `Accent Yellow`).
  - **Toggle / Switch (`OFF` / `ON`):** Indikator status biner yang jelas (`OFF` dan `ON`) dengan border hitam tebal dan warna aksen saat `ON` (`Accent Yellow` / `Accent Teal`).

## Navigation & Progress
- **Tabs:** Tab horizontal dengan border hitam; tab aktif menonjol dengan warna aksen (`Accent Teal`) atau highlight kontras.
- **Steps:** Indikator langkah (`STEP ONE`, `STEP TWO`, `STEP THREE`) dengan nomor berbingkai/lingkaran dan garis penghubung tegas.
- **Pagination:** Tombol `< PREV`, nomor halaman (`1`, `2`, `3`, `...`, `10`), dan `NEXT >` dengan border hitam tebal; halaman aktif menggunakan fill `Accent Teal` (`#00C2CB`).
- **Breadcrumbs:** Jejak navigasi `HOME > SECTION > CURRENT PAGE` dengan pemisah `>` dan penanda halaman aktif.

## Data Display
- **Card:** Kontainer berlatar `White` (`#FFFFFF`) atau aksen, border hitam tebal, bayangan offset (`shadow-1` / `shadow-2`), berisi judul tegas, deskripsi, dan tombol aksi.
- **List Item:** Baris item dengan avatar/ikon di kiri, judul + deskripsi singkat di tengah, dan indikator/panah (`>`) di kanan; tinggi/area klik nyaman (`>= 44px`).
- **Badge:** Label kecil berborder hitam dengan warna aksen (`Accent Teal`, `Accent Yellow`, `Accent Magenta`, atau `White`) untuk status/tag (`NEW`, `ALERT`, dll.).
- **Avatar:** Bingkai persegi/lingkaran berborder hitam tebal berisi foto, ikon user, atau inisial (misalnya `HM`).
- **Tooltip:** Kotak informasi pop-up berborder hitam tebal dengan teks penjelasan singkat dan kontras tinggi.
- **Table:** Tabel bergaris tegas dengan header berlatar gelap/kontras (misalnya `Black` `#000000` dengan teks putih), indikator sort pada kolom, baris data yang rapi, dan kolom `ACTIONS >`.

## Feedback
- **Toast Stack:** Notifikasi bertumpuk dengan border hitam tebal, warna aksen (`Accent Teal`, `Accent Yellow`, `Accent Magenta`), ikon status, pesan, dan tombol tutup (`X`).
- **Alert Banner:** Banner informasi/peringatan penuh lebar dengan border hitam tebal, ikon status, judul/pesan, dan warna aksen.
- **Modal Confirm:** Dialog konfirmasi di tengah layar dengan border hitam tebal, bayangan tegas, judul `MODAL CONFIRM`, pesan, serta tombol aksi (`CANCEL` dan `CONFIRM`).
- **Progress Bar:** Bilah progres berborder hitam tebal dengan isian warna aksen (`Accent Teal`) dan label persentase (misalnya `70%`).
- **Empty State:** Tampilan keadaan kosong berborder hitam tebal dengan judul (`NO ITEMS YET`), deskripsi singkat, dan tombol CTA utama (`CREATE YOUR FIRST ITEM`).
