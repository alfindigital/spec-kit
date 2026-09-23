# Design Specification Template

> **Tujuan:** Panduan visual, sistem token, serta spesifikasi interaksi UI/UX untuk menjaga konsistensi frontend.
> **Cara Pakai:** Copy ke repo project, isi tokens dan komponen sesuai design system project Anda.
> **Referensi Prompt:** Lihat [`guides/UI_UX_DESIGN_SYSTEM.md`](../guides/UI_UX_DESIGN_SYSTEM.md) untuk 8 template prompt UI/UX.
> **Template Komunitas:** [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md)

---

## 1. Design System & Component Library

- **Framework UI:** [misal: Shadcn UI + Radix UI + Lucide Icons]
- **Styling:** [misal: Tailwind CSS + CSS Variables]
- **Pendekatan:** Token-driven, bukan hardcoded CSS values

---

## 2. Design Tokens

### 2.1 Color Palette

| Token | Nilai | Penggunaan |
| :--- | :--- | :--- |
| `--primary` | [Kode warna] | CTA utama, link aktif |
| `--primary-foreground` | [Kode warna] | Teks di atas primary |
| `--secondary` | [Kode warna] | Aksen sekunder |
| `--background` | [Kode warna] | Background halaman |
| `--surface` | [Kode warna] | Card, panel, dialog |
| `--muted` | [Kode warna] | Background disabled |
| `--destructive` | [Kode warna] | Error, hapus, bahaya |
| `--success` | [Kode warna] | Sukses, konfirmasi |
| `--warning` | [Kode warna] | Peringatan |
| `--info` | [Kode warna] | Informasi netral |

<!-- EXAMPLE: Contoh palette SaaS modern:
| `--primary` | `hsl(222, 47%, 31%)` | Deep navy blue |
| `--destructive` | `hsl(0, 84%, 60%)` | Red-500 |
| `--success` | `hsl(142, 71%, 45%)` | Green-600 |
-->

### 2.2 Typography Scale

| Token | Nilai | Penggunaan |
| :--- | :--- | :--- |
| `--font-sans` | [Font Family] | Teks utama |
| `--font-mono` | [Monospace Font] | Kode, terminal |
| `--text-h1` | [size/line-height] | Heading utama |
| `--text-h2` | [size/line-height] | Sub-heading |
| `--text-h3` | [size/line-height] | Section heading |
| `--text-body` | [size/line-height] | Paragraf |
| `--text-sm` | [size/line-height] | Caption, label |

### 2.3 Spacing, Radius & Elevation

| Token | Nilai | Penggunaan |
| :--- | :--- | :--- |
| `--space-unit` | [4px / 8px] | Base grid unit |
| `--radius-sm` | [4px] | Input, badge |
| `--radius-md` | [8px] | Card, button |
| `--radius-lg` | [12px] | Modal, dialog |
| `--radius-full` | [9999px] | Avatar, pill |
| `--shadow-sm` | [value] | Subtle depth |
| `--shadow-md` | [value] | Card elevation |
| `--shadow-lg` | [value] | Modal, dropdown |

---

## 3. Component States & Edge Cases

Setiap komponen interaktif **WAJIB** menangani 7 state:

| # | State | Deskripsi | Implementasi |
| :--- | :--- | :--- | :--- |
| 1 | **Default** | Tampilan normal | Base styling |
| 2 | **Hover** | Mouse di atas elemen | `:hover` pseudo-class |
| 3 | **Focus-visible** | Keyboard navigation | `:focus-visible` outline |
| 4 | **Active / Pressed** | Sedang diklik | `:active` + scale transform |
| 5 | **Disabled** | Tidak aktif | `opacity: 0.5` + `cursor: not-allowed` |
| 6 | **Loading** | Menunggu data | Skeleton shimmer animation |
| 7 | **Error** | Input gagal / API error | Border destructive + pesan error |

**Tambahan untuk container/list:**
- **Empty State** — Ilustrasi + CTA saat belum ada data
- **Partial Load** — Infinite scroll / pagination skeleton

---

## 4. Responsive Breakpoints

| Breakpoint | Lebar Minimum | Target Device |
| :--- | :--- | :--- |
| `sm` | 640px | Mobile landscape |
| `md` | 768px | Tablet portrait |
| `lg` | 1024px | Tablet landscape / laptop |
| `xl` | 1280px | Desktop |
| `2xl` | 1536px | Large desktop |

---

## 5. Accessibility (a11y) & UX Integrity

### 5.1 Keyboard Navigation
- Navigasi penuh via `Tab`, `Enter`, `Space`, `Esc`
- Focus trap di modal/dialog (focus tidak keluar dari dialog yang terbuka)
- Skip link ke konten utama

### 5.2 Color & Contrast
- Rasio kontras minimal **4.5:1** untuk teks normal (WCAG AA)
- Rasio kontras minimal **3:1** untuk teks besar dan elemen UI
- Jangan gunakan warna sebagai **satu-satunya** indikator status

### 5.3 Semantic HTML
- Gunakan elemen semantik (`<nav>`, `<main>`, `<button>`, `<dialog>`) sebelum ARIA manual
- Semua gambar punya `alt` text deskriptif
- Form input punya `<label>` yang terhubung via `htmlFor`/`id`

---

## 6. Animation & Motion

| Properti | Nilai | Penggunaan |
| :--- | :--- | :--- |
| `--transition-fast` | `150ms ease` | Hover, focus |
| `--transition-base` | `200ms ease` | Toggle, expand |
| `--transition-slow` | `300ms ease` | Modal, page transition |

**Aturan:** Hormati `prefers-reduced-motion: reduce` — matikan animasi non-esensial.
