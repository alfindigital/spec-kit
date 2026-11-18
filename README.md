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
├── references/             # Standar coding & studi kasus lapangan
│   ├── AGENTS.md           # Aturan baku engineering AI
│   ├── guides/            # Panduan operasional engineering
│   └── case-studies/       # Studi kasus nyata praktisi (SariClean curated)
└── projects/               # Workspace drafting ide sebelum masuk ke Documents/Apps/
```

---

## Mulai

1. Pilih Lean atau Full dengan [aturan spec-kit](SKILL.md#pilih-alur-sebelum-menulis-spec).
2. Ambil dokumen dari [panduan template](templates/README.md). Untuk draft ide
   yang belum punya repo, gunakan `projects/<nama-ide>/`.
3. Simpan spec dan satu `PLAN.md` di repo proyek. Isi kriteria selesai sesuai
   target pemakaian, lalu verifikasi alur utama ketika perubahan siap.
