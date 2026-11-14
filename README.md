# Blueprints — Software Planning & Specification Hub

Hub resmi di PC ini untuk merancang cetak biru software (PRD, Tech Spec, ERD, User Flow, dan Implementation Plan) sebelum mengeksekusi koding bersama AI coding agent.

Repository ini terhubung langsung dengan remote GitHub [`alfindigital/spec-kit`](https://github.com/alfindigital/spec-kit) dan terintegrasi dengan skill AI master `spec-kit`.

---

## 📁 Struktur Direktori

```
Documents/AI/blueprints/
├── README.md               # Peta hub & panduan pemilihan template
├── templates/              # Template spesifikasi siap di-copy ke repo proyek
│   ├── LEAN_PRD.md         # Format kilat 1-halaman (MVP / Micro-SaaS / Vibe Coding cepat)
│   ├── IMPLEMENTATION_PLAN.md # Checklist eksekusi atomik untuk AI agent
│   ├── PRODUCT_SPEC.md     # Mega-spec produk lengkap (Why + What + Requirements)
│   ├── TECH_SPEC.md        # Mega-spec teknis (Arsitektur + ERD/DDL + API Contracts)
│   ├── DESIGN_SPEC.md      # Spec desain UI/UX (Tokens, Layout, a11y, States)
│   ├── INTAKE.md           # Protokol wawancara gali ide awal
│   ├── BLUEPRINT_REVIEW.md # Gerbang review sebelum spec disetujui (STATUS: APPROVED)
│   ├── living-docs/        # Template dokumen pemantauan (CURRENT_STATE, TASKS, HANDOVER)
│   └── ops/                # Template kesiapan produksi & runbook
├── guides/                 # Panduan operasional engineering
│   ├── VIBE_CODING_MASTERGUIDE.md # Panduan lengkap alur vibe coding modern
│   ├── ANTI_OVERENGINEERING.md   # Prinsip YAGNI & pembatas kompleksitas
│   ├── TESTING_AND_INTEGRITY.md  # Standar testing & verifikasi
│   └── SECURITY_AND_SECRETS.md   # Manajemen rahasia & keamanan
├── references/             # Standar coding & studi kasus lapangan
│   ├── AGENTS.md           # Aturan baku engineering AI
│   └── case-studies/       # Studi kasus nyata praktisi (SariClean curated)
└── projects/               # Workspace drafting ide sebelum masuk ke Documents/Apps/
```

---

## 🎯 Panduan Pemilihan Template: Pakai yang Mana?

| Jenis Proyek / Kebutuhan | Template yang Digunakan | Waktu Pengerjaan Spec |
|---|---|---|
| **Micro-SaaS, MVP Cepat, Vibe Coding, Side Project** | [`templates/LEAN_PRD.md`](templates/LEAN_PRD.md) + [`templates/IMPLEMENTATION_PLAN.md`](templates/IMPLEMENTATION_PLAN.md) | 15–30 Menit |
| **Aplikasi Bisnis / Klien / Sistem Skala Penuh** | [`templates/PRODUCT_SPEC.md`](templates/PRODUCT_SPEC.md) + [`templates/TECH_SPEC.md`](templates/TECH_SPEC.md) | 1–3 Jam |
| **Aplikasi dengan UI/UX Kustom Tinggi** | Tambahkan [`templates/DESIGN_SPEC.md`](templates/DESIGN_SPEC.md) | Sesuai kebutuhan |
| **Fitur Baru pada Sistem yang Sudah Berjalan** | [`templates/BROWNFIELD_SPEC.md`](templates/BROWNFIELD_SPEC.md) | 30 Menit |

---

## 🚀 Alur Kerja Cepat (Quickstart)

1. **Inisiasi Ide:**
   Buat folder di `projects/<nama-ide>/` dan copy [`templates/LEAN_PRD.md`](templates/LEAN_PRD.md).
2. **Kunci Fondasi (Architecture & Data Gate):**
   Tulis problem inti, tentukan stack, dan definisikan skema database/ERD minimal.
3. **Uji Kritis (`/grill-me`):**
   Minta AI menguji logika spec untuk menemukan celah atau edge cases tersembunyi.
4. **Eksekusi di Folder Proyek:**
   Pindahkan spec yang sudah disetujui ke repo proyek (misal di `Documents/Apps/<proyek>/specs/`) dan jalankan koding bertahap bersama AI agent.
