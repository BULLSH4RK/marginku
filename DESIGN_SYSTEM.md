# Neo-Brutalist Design System

**Tagline:** BOLD. LOUD. SYSTEMATIC. USABLE.

## Core Usage Rules
- **Thick Black Strokes:** Gunakan border hitam pekat (`#000000`) yang tegas pada komponen utama, kartu, tombol, input, dan tabel.
- **Squared Corners / Limited Radii:** Utamakan sudut tajam (`0px`) atau radius sangat terbatas (`0px`–`4px`) sesuai spesifikasi komponen; hindari rounded-pill yang berlebihan kecuali pada elemen spesifik seperti toggle/avatar kecil jika diperlukan.
- **Solid Accent Blocks:** Gunakan blok warna solid tanpa gradasi lembut atau efek blur kaca (*glassmorphism*).
- **Strict Grid Alignment:** Susun layout secara sistematis mengikuti grid yang jelas dan rapi.
- **High Contrast:** Pertahankan kontras tinggi antara teks, latar belakang, dan border agar mudah dibaca.
- **Focus Always Visible:** Setiap elemen interaktif (tombol, input, link, tab, kontrol) wajib memiliki indikator `:focus-visible` yang jelas dan kontras.
- **Minimum Hit Area:** Pertahankan area sentuh/klik minimal **44px** (`min-h-[44px]` / `min-w-[44px]`) untuk elemen interaktif utama.
- **One Primary Action:** Batasi agar setiap section atau konteks aksi memiliki satu aksi utama (*Primary Action*) yang menonjol.

## Color Tokens
| Token Name | Hex Value | Role / Usage |
| :--- | :--- | :--- |
| `Accent Teal` | `#00C2CB` | Primary accent, active tab/pagination/step, progress fill, info/primary action |
| `Accent Magenta` | `#FF00FF` | Secondary/highlight accent, destructive/danger accent, badge/toast/alert highlight |
| `Accent Yellow` | `#FFF000` | Warning/highlight accent, step/badge/toast/alert accent, CTA highlight |
| `Black` | `#000000` | Primary strokes/borders, primary text, hard offset shadows, high-contrast surfaces |
| `White` | `#FFFFFF` | Surface/card/input background, text on dark/accent surfaces |
| `Paper Beige` | `#F5F5DC` | Main canvas / page background |

## Typography Scale
Gunakan tipografi sans-serif tebal (*bold* / *extra-bold* / *black*) untuk heading dan label, serta font yang bersih dan mudah dibaca untuk body text.

| Level | Size | Usage |
| :--- | :--- | :--- |
| `H1` | `96px` | Giant Heading (display / hero) |
| `H2` | `64px` | Large Heading |
| `H3` | `48px` | Medium Heading |
| `H4` | `32px` | Small Heading |
| `Body` | `18px` | Default reading text |
| `Caption` | `14px` | Smaller descriptions, helper text, metadata |

## Spacing Scale
Gunakan skala spasi berbasis kelipatan genap/grid:
- **Micro / Component Spacing:** `4px`, `8px`, `12px`, `16px`, `24px`, `32px`, `48px`
- **Section / Layout Spacing:** `32px`, `48px`, `64px`, `96px`

## Elevation Tokens (Hard Offset Shadows)
Gunakan *hard-edged offset shadow* (tanpa blur atau dengan blur minimal sesuai token):
- `shadow-0`: None (`0px 0px 0px #000000`)
- `shadow-1`: Subtle (`2px 2px 0px #000000` atau `4px` offset)
- `shadow-2`: Prominent (`6px 6px 0px #000000` atau `12px` offset)

## Icons
- Gunakan ikon berketebalan garis tegas (*bold stroke* / *solid fill*) yang selaras dengan estetika brutalist.
- Ikon umum yang dipakai: close (`X`), check, settings/gear, arrow right, user, trash/delete, search, menu/hamburger, plus, warning triangle.
