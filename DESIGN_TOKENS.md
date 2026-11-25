# DESIGN_TOKENS.md — Token Tiga Lapis MarginKu

Mengikuti skill `design-system`: **Primitive → Semantic → Component**. Nilai bergaya CSS (`--x`) berlaku sebagai nama acuan; di Flutter dipetakan ke Dart (§5). `px` = `dp`.

Aturan: widget **hanya** boleh memakai token semantic/component. Tidak ada `Color(0x...)`, angka spacing, atau radius langsung di `lib/features/**` (lihat §6).

## 1. Primitive (nilai mentah)

### Warna
| Token | Nilai | Token | Nilai |
|---|---|---|---|
| `--color-slate-50` | `#F8FAFC` | `--color-slate-700` | `#334155` |
| `--color-slate-100` | `#F1F5F9` | `--color-slate-800` | `#1E293B` |
| `--color-slate-150` | `#E8ECF1` | `--color-slate-900` | `#0F172A` |
| `--color-slate-200` | `#E2E8F0` | `--color-slate-950` | `#020617` |
| `--color-slate-300` | `#CBD5E1` | `--color-blue-200` | `#BFDBFE` |
| `--color-slate-400` | `#94A3B8` | `--color-blue-900` | `#1E3A8A` |
| `--color-slate-600` | `#475569` | `--color-amber-700` | `#A16207` |
| `--color-amber-400` | `#FBBF24` | `--color-amber-800` | `#B45309` |
| `--color-green-700` | `#15803D` | `--color-green-400` | `#4ADE80` |
| `--color-red-600` | `#DC2626` | `--color-red-400` | `#F87171` |
| `--color-white` | `#FFFFFF` | | |

### Spacing (basis 4)
`--space-1: 4` · `--space-2: 8` · `--space-3: 12` · `--space-4: 16` · `--space-6: 24` · `--space-8: 32` · `--space-12: 48`

### Radius
`--radius-sm: 8` · `--radius-md: 12` · `--radius-lg: 16` · `--radius-xl: 24` · `--radius-full: 999`

### Tipografi
Font: **IBM Plex Sans** (fallback Roboto).
| Token | Ukuran | Bobot | Dipakai untuk |
|---|---|---|---|
| `--font-caption` | 12 / 1.4 | 400 | caption, helper |
| `--font-body` | 14–16 / 1.5 | 400 | isi, field |
| `--font-title` | 16 / 1.4 | 600 | judul kartu |
| `--font-heading` | 20 / 1.3 | 600 | judul layar |
| `--font-display` | 32 / 1.2 | 700 | harga jual akhir |
| `--font-metric` | 24 / 1.2 | 700 | HPP per unit |
Angka uang: `FontFeature.tabularFigures()`.

### Motion & lainnya
`--duration-fast: 150ms` · `--duration-base: 200ms` · `--duration-slow: 300ms` · `--ease-standard: easeInOut` · `--opacity-disabled: 0.5` · `--touch-min: 48` · `--ring-width: 2` · `--ring-offset: 2`

## 2. Semantic — Terang

| Token | Referensi primitive | Hex |
|---|---|---|
| `--color-background` | slate-50 | `#F8FAFC` |
| `--color-foreground` | slate-950 | `#020617` |
| `--color-card` | white | `#FFFFFF` |
| `--color-card-foreground` | slate-950 | `#020617` |
| `--color-primary` | slate-900 | `#0F172A` |
| `--color-on-primary` | white | `#FFFFFF` |
| `--color-secondary` | blue-900 | `#1E3A8A` |
| `--color-on-secondary` | white | `#FFFFFF` |
| `--color-accent` | amber-700 | `#A16207` |
| `--color-on-accent` | white | `#FFFFFF` |
| `--color-muted` | slate-150 | `#E8ECF1` |
| `--color-muted-foreground` | slate-600 | `#475569` |
| `--color-border` | slate-200 | `#E2E8F0` |
| `--color-ring` | slate-900 | `#0F172A` |
| `--color-success` | green-700 | `#15803D` |
| `--color-warning` | amber-800 | `#B45309` |
| `--color-destructive` | red-600 | `#DC2626` |
| `--color-on-destructive` | white | `#FFFFFF` |

## 3. Semantic — Gelap (diuji terpisah, bukan turunan otomatis)

| Token | Referensi primitive | Hex |
|---|---|---|
| `--color-background` | slate-950 | `#020617` |
| `--color-foreground` | slate-50 | `#F8FAFC` |
| `--color-card` | slate-900 | `#0F172A` |
| `--color-card-foreground` | slate-50 | `#F8FAFC` |
| `--color-primary` | blue-200 | `#BFDBFE` |
| `--color-on-primary` | slate-900 | `#0F172A` |
| `--color-secondary` | slate-800 | `#1E293B` |
| `--color-on-secondary` | slate-50 | `#F8FAFC` |
| `--color-accent` | amber-400 | `#FBBF24` |
| `--color-on-accent` | slate-900 | `#0F172A` |
| `--color-muted` | slate-800 | `#1E293B` |
| `--color-muted-foreground` | slate-400 | `#94A3B8` |
| `--color-border` | slate-700 | `#334155` |
| `--color-ring` | blue-200 | `#BFDBFE` |
| `--color-success` | green-400 | `#4ADE80` |
| `--color-warning` | amber-400 | `#FBBF24` |
| `--color-destructive` | red-400 | `#F87171` |
| `--color-on-destructive` | slate-950 | `#020617` |

Wajib: teks normal ≥ 4.5:1 di kedua mode; border/status dapat dibedakan di kedua mode.

## 4. Component Tokens

| Komponen | Token → referensi semantic/primitive |
|---|---|
| Button | `--button-bg` → primary · `--button-fg` → on-primary · `--button-radius` → radius-md · `--button-height` → touch-min · `--button-padding-x` → space-4 |
| Button sekunder | `--button-secondary-border` → primary · fg → primary · bg → transparan |
| Input | `--input-bg` → background · `--input-border` → border · `--input-focus-ring` → ring · `--input-radius` → radius-md · `--input-height` → touch-min |
| Card | `--card-bg` → card · `--card-border` → border · `--card-padding` → space-4 · `--card-radius` → radius-lg |
| Chip | `--chip-bg` → muted · `--chip-selected-bg` → primary · `--chip-selected-fg` → on-primary · `--chip-radius` → radius-full · `--chip-height` → touch-min |
| Bottom sheet | `--sheet-bg` → card · `--sheet-radius` → radius-xl (atas) · `--sheet-scrim` → foreground @ 50% |
| Nav bar | `--nav-bg` → card · `--nav-selected` → primary · `--nav-unselected` → muted-foreground · `--nav-height` → 64 |
| Result card | `--result-bg` → card · `--result-profit` → success · `--result-loss` → destructive · `--result-metric` → font-display |
| Badge | `--badge-warning-bg` → warning @ 12% · `--badge-warning-fg` → warning |

## 5. Pemetaan ke Flutter
```
lib/core/theme/
  primitives.dart        # const Color/double mentah (hanya file ini boleh punya Color(0x...))
  tokens.dart            # ThemeExtension<MarginKuTokens>: semantic light + dark
  component_tokens.dart  # const ButtonTokens, InputTokens, CardTokens, ChipTokens, ...
  app_theme.dart         # ThemeData light & dark dari tokens (useMaterial3: true)
```
- Akses: `Theme.of(context).extension<MarginKuTokens>()!` lewat helper `context.tokens`.
- `ColorScheme` dibangun dari semantic tokens; `ThemeMode.system` default.
- Ganti tema = ganti lapis semantic saja; primitive dan component tidak berubah.

## 6. Validasi Token (setara `validate-tokens`)
```bash
# tidak boleh ada nilai keras di luar core/theme
grep -rnE "Color\(0x|Colors\.[a-z]+" lib --include=*.dart | grep -v "lib/core/theme" && echo "GAGAL: warna keras" || echo OK
grep -rnE "EdgeInsets\.(all|symmetric|only)\([0-9]" lib/features --include=*.dart && echo "GAGAL: spacing keras" || echo OK
```
Jalankan sebelum commit. Pengecualian harus dicatat di `TASKS.md`.
