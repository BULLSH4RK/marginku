# Instructions for AI Coding Agent

Ketika membangun atau memodifikasi UI/UX aplikasi ini, selalu ikuti aturan berikut:

1. **Baca Referensi Sistem Desain:** Selalu rujuk `DESIGN_SYSTEM.md` dan `COMPONENTS.md` sebelum membuat halaman atau komponen baru.
2. **Konsistensi Visual:**
   - Gunakan warna latar utama `Paper Beige` (`#F5F5DC`) dan permukaan kartu/input `White` (`#FFFFFF`), dipadukan dengan `Black` (`#000000`) serta aksen `Accent Teal` (`#00C2CB`), `Accent Magenta` (`#FF00FF`), dan `Accent Yellow` (`#FFF000`).
   - Hindari *drop-shadow* buram ala Material/iOS; gunakan *hard offset shadow* (`x y 0px #000000`).
3. **Aksesibilitas & Interaksi:**
   - Pastikan semua tombol, link, tab, dan item interaktif memenuhi batas minimal ukuran sentuh `44px` jika memungkinkan.
   - Jangan pernah menghilangkan `:focus-visible`.
   - Jaga agar setiap bagian/layar memiliki satu aksi utama yang paling menonjol.
4. **Struktur & Kode Bersih:**
   - Buat komponen yang *reusable* (Button, Input, Card, Badge, Modal, Toast, Alert, Table, Pagination, Stepper, EmptyState).
   - Definisikan CSS variables / design tokens di awal stylesheet agar mudah diubah dan konsisten di seluruh aplikasi.
