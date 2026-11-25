# BRAND.md — Identitas MarginKu

Mengikuti skill `design` (sub-skill `brand` & icon). Sumber kebenaran untuk suara, nama, logo, dan ikon aplikasi.

## Inti
- **Nama:** MarginKu (satu kata, huruf besar M dan K).
- **Tagline:** "Hitung HPP, tentukan harga, tahu untungmu."
- **Janji:** hasil hitungan jelas, cepat, dan tetap di HP-mu (tanpa akun).
- **Kepribadian:** tepercaya, tenang, praktis, tidak menggurui.

## Suara & Nada (Bahasa Indonesia)
| Lakukan | Hindari |
|---|---|
| Kalimat pendek, kata kerja jelas: "Tambah bahan", "Simpan ke resep" | Jargon akuntansi ("laba kotor", "COGS") |
| "Untung per unit", "HPP per unit" (jelaskan HPP sekali di tooltip) | Nada menghakimi/bercanda berlebihan |
| Pesan error membantu: "Jumlah beli harus lebih dari 0" | Pesan teknis ("Exception", kode error) |
| Sapaan netral "kamu" bila perlu | Campur formal/santai dalam satu layar |

Tooltip HPP: "HPP (Harga Pokok Penjualan) = biaya membuat 1 unit produk."

## Warna & Tipografi
Mengacu pada `DESIGN_TOKENS.md` (navy `#0F172A` sebagai warna merek, emas `#A16207` sebagai sorotan kecil). Font IBM Plex Sans.

## Logo (arah desain)
- Bentuk: monogram "M" sederhana yang membentuk grafik naik/margin (garis diagonal naik pada kaki kanan M).
- Gaya: geometris, datar, satu warna merek + satu sorotan emas; tanpa gradien, tanpa efek 3D.
- Versi: ikon (monogram), wordmark horizontal, mono (hitam/putih).
- Ruang aman: minimal tinggi huruf "M" di semua sisi. Ukuran minimum ikon 24 dp.
- Latar logo untuk aset presentasi/stor: putih.

## Ikon Aplikasi (Android/iOS)
- Adaptive icon: latar navy solid `#0F172A`, foreground monogram putih + sorotan emas, aman dalam zona 66% tengah.
- Ekspor 1024×1024 (store), 512×512 (Play), plus monochrome layer untuk themed icon Android.
- Gunakan `flutter_launcher_icons` untuk generate.

## Ikon UI
Satu keluarga outline (Material Symbols Outlined), stroke konsisten, ukuran 24 dp. Dilarang emoji sebagai ikon. Ikon dekoratif disembunyikan dari screen reader; ikon bermakna punya label.

## Aset Toko (opsional)
Nama pendek: "MarginKu: Hitung HPP". Deskripsi: aplikasi offline hitung HPP dan harga jual untuk UMKM. Tangkapan layar: 3 tab, mode terang, data contoh `Brownies kukus`.
