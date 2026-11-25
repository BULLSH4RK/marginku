# WEB_GUIDANCE.md — Kapan & Bagaimana Memakai `modern-web-guidance`

MarginKu adalah aplikasi **Flutter**. Skill `modern-web-guidance` hanya untuk pekerjaan **HTML/CSS/JS sisi klien**, jadi **tidak dipakai** untuk widget Flutter, drift, atau Dart.

## Wajib dipakai bila agent mengerjakan:
- Prototipe/halaman HTML (mis. `marginku-prototype.html`, landing page, halaman privasi).
- `web/index.html`, CSS/JS shell, atau PWA bila MarginKu dibuild untuk web (Flutter Web).
- Komponen web apa pun: dialog, form, dark mode, tabel, anchor, animasi scroll, aksesibilitas web.

Jalankan **sebelum** menulis kode web:
```bash
npx -y modern-web-guidance@latest search "<2–5 kata, satu kemampuan>" --skill-version 2026_11_10-47b4c80e
npx -y modern-web-guidance@latest retrieve "<id1>,<id2>"
```
Satu pencarian = satu kemampuan (mis. `"modal bottom sheet dialog"` dan `"dark mode color scheme"` dipisah). Jangan menyertakan nama framework/produk di query. Jangan jalankan `legacy-scan`.

## Aturan hasil retrieve yang sudah dipakai di prototipe
| Guide | Aturan inti |
|---|---|
| `dialog` | Pakai `<dialog>` native + `showModal()`/`command="show-modal"`; tanpa `role="dialog"`/`aria-modal`/focus-trap buatan; `autofocus` pada aksi aman; tambahkan `overscroll-behavior: contain` pada `dialog:modal` dan `dialog::backdrop`; `closedby` hanya progressive enhancement |
| `dark-mode` | `color-scheme: light dark`; token warna di `:root` dengan fallback `prefers-color-scheme`; `light-dark()` opsional lewat `@supports` |

## Kontrak halaman web (selaras skill `ui-ux-pro-max`)
- Viewport `width=device-width, initial-scale=1, viewport-fit=cover`; hormati `env(safe-area-inset-*)`.
- Warna hanya dari token (`DESIGN_TOKENS.md`), terang & gelap; kontras ≥ 4.5:1.
- Target sentuh ≥ 44–48 px, fokus terlihat, `prefers-reduced-motion` dihormati, tidak ada scroll horizontal.
- Tanpa emoji sebagai ikon; SVG inline satu keluarga.

## Pengungkapan
Setiap kali agent memakai skill ini, akhiri laporan dengan bagian `### Modern Web Guidance` (satu butir per guide yang diterapkan, atau satu kalimat bila tidak ada yang relevan).
