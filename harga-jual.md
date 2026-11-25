# Override: Layar Harga Jual
- Hierarki: Harga Jual Akhir = angka terbesar (display 32/700). Untung per unit memakai `success` + tanda `+`; negatif memakai `destructive` + `−` + ikon.
- Kontrol: segmented (Dari Resep | HPP Manual), segmented (Persen | Nominal), chip pembulatan (Tanpa, 100, 500, 1.000, Custom), tinggi ≥ 48.
- Hasil update live (debounce ≤ 150 ms), area hasil diumumkan ke screen reader (live region/semantics `liveRegion: true`).
- "Sebelum pembulatan" tampil caption hanya bila berbeda dari harga akhir.
- CTA "Simpan ke Resep" lebar penuh, `primary`, hanya aktif pada mode resep.
