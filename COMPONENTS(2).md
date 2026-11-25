# COMPONENTS.md — Spesifikasi Komponen & State MarginKu

Mengikuti skill `design-system` (Component Spec Pattern + States & Variants). Semua nilai berupa token dari `DESIGN_TOKENS.md`. Di mobile, "hover" diganti **pressed**.

## State Umum
| State | Pemicu | Perubahan visual |
|---|---|---|
| default | — | tampilan dasar |
| pressed | sentuh | ripple/overlay 8–12%, tanpa menggeser layout |
| focus | keyboard/aksesibilitas | ring 2 dp `ring`, offset 2 dp |
| disabled | tidak valid | opacity 0.5, tidak interaktif, alasan jelas di teks bantu |
| loading | proses async | indikator kecil, tombol nonaktif |
| error | validasi gagal | border `destructive`, pesan di bawah field |

Prioritas bila bertumpuk: disabled > loading > pressed > focus > default. Transisi 150–200 ms, hormati reduced motion.

## Button
| Properti | Primary | Secondary | Destructive | Disabled |
|---|---|---|---|---|
| Latar | primary | transparan | destructive | muted |
| Teks | on-primary | primary | on-destructive | muted-foreground |
| Border | — | 1.5 dp primary | — | — |
| Tinggi | 48 | 48 | 48 | 48 |
| Radius | radius-md | radius-md | radius-md | radius-md |
- Label kata kerja jelas ("Simpan", "Ganti Semua Data"). Ikon-saja wajib punya label semantik/tooltip.
- Maks satu tombol primary per layar/sheet.

## Input (TextFormField)
- Tinggi ≥ 48, radius-md, border `border`; fokus: border `ring` + ring 2 dp.
- Label selalu terlihat di atas field (bukan placeholder saja); helper di bawah; error di bawah field yang sama (teks + ikon, bukan warna saja).
- `MoneyField`: prefix `Rp`, keyboard numerik, format ribuan `id_ID`. `QtyField`: suffix satuan dasar.
- Pakai `Form` + `GlobalKey<FormState>` dan `TextEditingController`.

## Card
- `card` + border 1 dp `border`, radius-lg, padding space-4. Kartu yang bisa ditekan: pressed state + `Semantics(button: true)`.
- Tanpa bayangan; tanpa transform saat ditekan.

## Chip (pembulatan, filter)
| | Default | Selected | Disabled |
|---|---|---|---|
| Latar | muted | primary | muted |
| Teks | foreground | on-primary | muted-foreground |
- Tinggi 48 (area sentuh), radius penuh; selected diumumkan lewat `Semantics(selected: true)`; jangan hanya warna (tambahkan centang ikon).

## Segmented Control
Dua/tiga opsi (Persen|Nominal, Dari Resep|HPP Manual). Selected = primary/on-primary. Tinggi 48. Perubahan mengumumkan nilai baru.

## Bottom Sheet (form bahan, picker bahan)
- Radius atas 24, scrim 50%, drag handle dengan alternatif tombol "Tutup/Batal".
- Fokus awal ke field pertama; ditutup lewat back/gesture; tombol Simpan tidak tertutup keyboard (`isScrollControlled`, padding `viewInsets`).
- Perubahan belum disimpan + tutup → konfirmasi "Buang perubahan?" (aksi aman terfokus).

## Navigation Bar
3 item (Bahan, Resep, Harga Jual), tinggi 64 + safe area. Selected = `primary` + label tebal; unselected = `muted-foreground`. Label selalu tampil. State tiap tab dipertahankan (`IndexedStack`).

## Result Card (Harga Jual)
| Elemen | Token |
|---|---|
| Harga Jual Akhir | font-display, foreground |
| Untung per unit | font-title; `success` + `+` (positif) / `destructive` + `−` dan ikon (negatif) |
| Markup aktual, Margin | font-body, muted-foreground label + foreground nilai |
| Sebelum pembulatan | font-caption, muted-foreground; tampil bila berbeda |
Seluruh kartu `Semantics(liveRegion: true)` agar perubahan hasil dibacakan.

## Badge "Perlu ditinjau"
Latar `warning` 12%, teks `warning`, ikon info, radius-full, tinggi 24 (tidak interaktif). Teks wajib ada.

## Snackbar / Dialog
- Snackbar: sukses netral; error `destructive` + ikon. Durasi 4 dtk, tidak menutup nav.
- Dialog konfirmasi destruktif: aksi aman ("Batal") terfokus awal; tombol destruktif diberi label spesifik.

## Empty & Error State
Ikon outline 48 dp, 1 kalimat jelas, 1 aksi. Error DB/file: pesan ramah + tombol coba lagi bila relevan.

## Aksesibilitas Komponen
Kontras ≥ 4.5:1 (terang & gelap), target ≥ 48 dp & jarak ≥ 8 dp, urutan fokus = urutan visual, font scale 200% tanpa terpotong, alternatif non-gesture untuk swipe/drag, tidak mengandalkan warna saja.
