# Specs — Panduan Workflow 3-Template

> **Fungsi:** Folder ini berisi **3 mega-template spesifikasi** yang menjadi blueprint saat memulai project baru. Template ini bersifat **read-only** — copy ke repo project, lalu isi sesuai konteks.

---

## Workflow Spesifikasi

$$\text{PRODUCT\_SPEC (Why + What)} \longrightarrow \text{TECH\_SPEC (How + Data)} \longrightarrow \text{DESIGN\_SPEC (Look \& Feel)} \xrightarrow{\text{/grill-me}} \text{PLAN.md} \longrightarrow \text{Code}$$

## 3 Template

| # | File | Fokus | Pertanyaan Utama |
|---|------|-------|------------------|
| 1 | [`PRODUCT_SPEC.md`](PRODUCT_SPEC.md) | **Why + What + Requirements** | Mengapa dibangun? Untuk siapa? Apa backlog & DoD? |
| 2 | [`TECH_SPEC.md`](TECH_SPEC.md) | **How + Data + Contracts** | Arsitektur? Tech stack? DDL? API? Validasi? Security? |
| 3 | [`DESIGN_SPEC.md`](DESIGN_SPEC.md) | **Look & Feel** | Design tokens? Typography? Component states? Accessibility? |

## Cara Pakai

1. **Copy** ketiga file ke folder `specs/` di repo project baru
2. **Isi** setiap section sesuai konteks project — hapus komentar `<!-- EXAMPLE -->`
3. **Validasi** dengan skill `grill-me` (standalone) — stress-test asumsi & edge cases; output-nya: istilah → `CONTEXT.md`, keputusan berat → `docs/adr/`, sisanya → balik ke spec
4. **Buat** `PLAN.md` berdasarkan specs yang sudah divalidasi
5. **Eksekusi** dengan TDD — commit atomik per backlog

## Aturan

- **JANGAN** edit template di folder ini — selalu copy ke project repo
- **JANGAN** tambah file baru di folder ini tanpa update `HARNESS.md`
- Semua contoh ditandai `<!-- EXAMPLE -->` — ganti dengan data project Anda
