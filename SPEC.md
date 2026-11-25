# SPEC.md — Spesifikasi Teknis MarginKu

## 1. Arsitektur
- Layered: `UI (features/*)` → `Providers (Riverpod)` → `Repositories` → `drift DB`.
- `core/calc.dart` berisi fungsi murni; dipakai provider untuk menurunkan nilai dari data DB.
- Reaktif: list bahan/resep memakai `watch` (stream drift). Ubah harga bahan → stream resep terpicu → HPP otomatis ter-update.
- Navigasi: `Scaffold` + `NavigationBar` (3 destinasi) + `IndexedStack` agar state tiap tab terjaga.

## 2. Rumus Inti
```
Harga Satuan      = pack_price / pack_qty
Biaya Baris       = qty_used * Harga Satuan(bahan)
Total Biaya       = SUM(Biaya Baris) + extra_cost
HPP per Unit      = Total Biaya / yield_qty
```
Mode persen : `Harga Awal = HPP * (1 + markup_pct/100)`
Mode nominal: `Harga Awal = HPP + nominal`

Setelah pembulatan:
```
Untung per Unit = Harga Jual - HPP
Markup Aktual % = Untung / HPP * 100        (HPP = 0 -> tampil "-")
Margin %        = Untung / Harga Jual * 100 (Harga Jual = 0 -> tampil "-")
```

## 3. Satuan
| Satuan dasar | Input boleh | Konversi ke dasar |
|---|---|---|
| `gram` | `gram`, `kg` | kg × 1000 |
| `ml` | `ml`, `liter` | liter × 1000 |
| `pcs` | `pcs`, `lusin`, `kodi`* | lusin × 12, kodi × 20 |

*`lusin`/`kodi` opsional (P2). Konversi hanya saat input form; DB selalu menyimpan satuan dasar. Takaran di resep selalu dalam satuan dasar bahan (tampilkan label satuan di samping field).

## 4. Pembulatan
```dart
enum RoundingMode { up, nearest }  // default: up

double roundPrice(double raw, int step, {RoundingMode mode = RoundingMode.up}) {
  if (step <= 0) return raw;
  final x = double.parse((raw / step).toStringAsFixed(6)); // buang noise floating point
  final n = mode == RoundingMode.up ? x.ceilToDouble() : x.roundToDouble();
  return n * step;
}
```
- Wajib normalisasi `toStringAsFixed(6)` sebelum `ceil`. Contoh bug: `1000 * 1.1 = 1100.0000000000002` → tanpa normalisasi menjadi 1200 (step 100).
- `step = 0` berarti tanpa pembulatan (tampilkan 2 desimal maksimal dibulatkan ke rupiah terdekat).
- Pilihan cepat step: 100, 500, 1000, custom.

## 4a. Presisi Uang
- Kolom DB `real` sesuai skema; perhitungan `double`.
- Tampilkan uang dibulatkan ke rupiah penuh (`Rp 1.100`), kecuali harga satuan kecil: tampilkan hingga 2 desimal bila < Rp 100 (mis. `Rp 15,5 / gram`). Hapus nol di belakang koma.
- Jangan membulatkan nilai di tengah perhitungan; bulatkan hanya saat tampil, kecuali pembulatan harga jual.

## 5. Validasi
| Field | Aturan | Pesan |
|---|---|---|
| Nama bahan | wajib, ≤ 60 char, unik (case-insensitive) | "Nama bahan wajib diisi" / "Nama bahan sudah ada" |
| `pack_qty` | > 0 | "Jumlah beli harus lebih dari 0" |
| `pack_price` | ≥ 0 | "Harga tidak boleh negatif" |
| Nama resep | wajib, ≤ 60 char | "Nama resep wajib diisi" |
| `yield_qty` | > 0 | "Hasil jadi harus lebih dari 0" |
| `extra_cost` | ≥ 0 | "Biaya tambahan tidak boleh negatif" |
| `qty_used` | > 0 | "Takaran harus lebih dari 0" |
| `markup_pct` | ≥ 0, ≤ 10000 | "Persen untung tidak valid" |
| Nominal untung | ≥ 0 | "Nominal tidak boleh negatif" |

Input uang/angka menerima koma atau titik sebagai desimal; abaikan pemisah ribuan saat parsing.

## 6. Perilaku Layar

### 6.1 Bahan
- List: nama, `Rp X / satuan`, ringkasan "Beli 1000 gram · Rp 15.000". Urut nama A–Z. Search di atas.
- Tambah/edit via bottom sheet. Preview harga satuan live saat mengetik.
- Hapus: konfirmasi. Jika dipakai resep → blok + tampilkan nama resep pemakai.
- Setelah edit harga: snackbar "Harga diperbarui · N resep ikut terhitung ulang".

### 6.2 Resep
- List: nama, hasil jadi, HPP per unit. FAB tambah.
- Editor: field nama, hasil jadi, biaya tambahan; section "Bahan" berisi baris (nama bahan, takaran + satuan, biaya baris); tombol "Tambah bahan" membuka picker bahan (search).
- Panel ringkasan sticky di bawah: Total Biaya Bahan, Biaya Tambahan, Total Biaya, **HPP per Unit**.
- Jika bahan dalam resep harganya 0 → ikon peringatan di baris.
- Simpan resep + item dalam satu transaksi. Hapus resep menghapus item-nya (cascade).

### 6.3 Harga Jual
1. Segmented: `Dari Resep` | `HPP Manual`.
2. Resep: dropdown/search resep → HPP terisi (read-only, ikut live). Manual: field HPP.
3. Segmented target: `Persen (%)` | `Nominal (Rp)`. Prefill dari `markup_pct` resep bila ada.
4. Chip pembulatan: `Tanpa`, `100`, `500`, `1.000`, `Custom`. Prefill dari `rounding_step`.
5. Kartu hasil: Harga Jual Akhir (besar), Untung per Unit, Markup Aktual, Margin. Jika harga setelah pembulatan ≠ harga awal, tampil kecil "Sebelum pembulatan: Rp X".
6. Tombol "Simpan ke Resep" (hanya mode resep): simpan `markup_pct` (mode nominal → `nominal / HPP * 100`, 2 desimal), `rounding_step`, `selling_price`.
7. Di detail/list resep tampil "Harga jual: Rp X". Bila `selling_price` ≠ hasil hitung ulang → badge "Perlu ditinjau".

## 7. Provider (Riverpod) — nama yang disarankan
- `ingredientsStreamProvider(query)`
- `recipesWithHppProvider(query)` → `List<RecipeSummary{recipe, hpp, itemCount}>`
- `recipeDetailProvider(id)` → `RecipeDetail{recipe, lines, totals}`
- `pricingStateProvider` (StateNotifier/Notifier: source, hppManual, mode, value, step)
- `pricingResultProvider` → `PricingResult{raw, final, profit, markupPct, marginPct}`
- `backupServiceProvider`

## 8. Model Domain (Dart)
```dart
class RecipeLine { Ingredient ingredient; double qtyUsed; double unitPrice; double cost; }
class RecipeTotals { double ingredientsCost; double extraCost; double totalCost; double hpp; }
class PricingResult { double hpp; double raw; double finalPrice; double profit; double? markupPct; double? marginPct; }
```

## 9. Penanganan Error
- Error DB/file → snackbar pesan ramah + log debug; jangan crash.
- Restore gagal → data lama utuh (transaksi rollback).
- Semua dialog destruktif memakai konfirmasi dengan teks aksi jelas ("Hapus", "Ganti Semua Data").

## 10. Lokalisasi & Format
- `Intl.defaultLocale = 'id_ID'`; `NumberFormat.currency(locale:'id_ID', symbol:'Rp ', decimalDigits:0)`.
- Persen tampil 1–2 desimal, hapus nol belakang (`30%`, `36,36%`).
