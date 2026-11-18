---
name: spec-kit
description: >-
  Spec-driven development kit: alur Lean atau Full sesuai risiko, intake dan blueprint review,
  SPEC_SYNC, coding standards. Aktif saat mulai project software baru ("spec-kit", "bikin spec", "tulis PRD",
  "kickoff project"), brownfield ("revamp", "rewrite", "migrasi sistem lama"), living docs ("CURRENT_STATE",
  "handover"), runbook server, dan security research defensif ("bug-hunter", "bug bounty", "PoC vulnerability").
  Wajib SEBELUM generate kode project baru.
---

# Spec-Kit — Spec-Driven Development Blueprint (milik user)

Skill ini adalah distilasi praktik spec-driven development dari berbagai sumber.
Berisi template spesifikasi + standar engineering + panduan operasional sebagai
cara baku memulai dan mengeksekusi project software bersama AI agent.

---

## Pilih alur sebelum menulis spec

Pakai **Lean** untuk pekerjaan dengan cakupan jelas, dampak rendah, dan mudah
dibalik. Pakai **Full** bila perubahan menyentuh pembayaran, data sensitif atau
izin akses penting, migrasi produksi, kontrak lintas sistem, atau keputusan lain
yang mahal untuk dibalik. Untuk repo yang sudah berjalan, baca perilaku dan
dokumen yang ada lebih dulu, lalu terapkan alur pada perubahan yang diminta.
Jika risiko tinggi baru diketahui saat mengerjakan Lean, hentikan bagian yang
terdampak dan bawa keputusan itu ke review Full.

Gunakan [aturan engineering](references/AGENTS.md) sesuai konteks repo.

### Lean

1. Ambil tujuan, cakupan, dan batas dari brief. Tanyakan hanya keputusan yang
   mengubah perilaku, risiko, atau bukti selesai. Untuk celah yang bisa ditunda,
   catat asumsi atau keputusan terbukanya.
2. Isi [LEAN_PRD.md](templates/LEAN_PRD.md) sebagai kontrak hasil. Turunkan tugas
   ke satu `PLAN.md` dari [template plan](templates/IMPLEMENTATION_PLAN.md).
3. Periksa skenario utama, satu kegagalan yang relevan, dan target verifikasi.
   Catat keputusan yang sudah diberikan owner. Jika owner sudah meminta
   eksekusi dan scope cukup jelas, lanjutkan tanpa approval blueprint tambahan.

### Full

1. Ikuti protokol [INTAKE.md](templates/INTAKE.md). Jumlah pertanyaan mengikuti
   keputusan material yang belum terjawab.
2. Ringkas scope, risiko, keputusan terbuka dan paket dokumen di
   [BLUEPRINT_REVIEW.md](templates/BLUEPRINT_REVIEW.md). Lanjut setelah owner
   menyetujui ringkasan di chat atau dokumen. Catat sumber persetujuannya.
3. Isi atau perbarui PRODUCT_SPEC dan TECH_SPEC. Tambahkan DESIGN_SPEC jika
   perlu mendefinisikan UI, serta dokumen tambahan sesuai
   [panduan template](templates/README.md). Untuk revamp atau migrasi,
   audit kondisi lama dan pertahankan perilaku yang disepakati.
4. Uji asumsi dan skenario gagal dengan `tanya-dulu` mode grill. Tulis istilah
   yang sudah disepakati di `CONTEXT.md` bila diperlukan. Gunakan ADR untuk
   keputusan berat yang perlu diingat. Selesaikan keputusan yang menghambat
   pekerjaan berikutnya sebelum eksekusi bagian tersebut.
5. Turunkan spesifikasi tervalidasi ke satu `PLAN.md`. Jika ada roadmap,
   rinci milestone aktif saja. Kaitkan tugas dengan hasil dan verifikasinya.

Saat mengeksekusi plan, ikuti cara kerja repo untuk pengujian dan commit.
Untuk Full, gunakan TDD untuk perilaku yang berubah dan minta review segar
sebelum pekerjaan berisiko dinyatakan selesai.

### Perawatan dokumen

- Pakai [living docs](templates/living-docs/README.md) bila state dan handover belum
  tertangani oleh dokumen yang dipakai lintas sesi. Perbarui bagian yang berubah.
- Pakai [ops docs](templates/ops/README.md) bila ada proses yang perlu dioperasikan
  dan diverifikasi. Pakai [readiness gate](templates/ops/PRODUCTION_READINESS.md)
  bila targetnya go-live produksi.
- Untuk tiga pemakaian berikutnya pada pekerjaan yang sudah masuk antrean,
  tambahkan satu baris di catatan penutup yang sudah dipakai proyek. Catat jalur,
  waktu kerja sampai scope disepakati, jumlah keputusan yang dibuka kembali
  setelah coding mulai, serta dokumen yang dipakai untuk implementasi atau
  handover. Setelah tiga pekerjaan, gunakan ketiga catatan itu untuk menilai
  waktu yang dihemat dan pekerjaan ulang sebelum mengubah aturan lagi.

### Bonus — Git Hygiene
Untuk repo baru, copy [`templates/gitignore.allowlist`](templates/gitignore.allowlist)
sebagai `.gitignore` reverse-allowlist (default `*`, unignore eksplisit `!path`).

---

## 📚 Routing Table — Baca Reference Sesuai Situasi

JANGAN baca semua reference sekaligus (~70KB). Baca hanya yang relevan:

| Situasi / Task | File yang Dibaca |
|----------------|------------------|
| Workflow harian AI coding | [`references/guides/VIBE_CODING_MASTERGUIDE.md`](references/guides/VIBE_CODING_MASTERGUIDE.md) |
| Multi-agent orchestration | [`references/guides/AGENT_ARCHITECTURE_AND_WORKFLOW.md`](references/guides/AGENT_ARCHITECTURE_AND_WORKFLOW.md) |
| Validasi/interogasi spec | Skill `tanya-dulu` mode grill (sibling) — bukan bagian spec-kit |
| Mencegah overengineering | [`references/guides/ANTI_OVERENGINEERING.md`](references/guides/ANTI_OVERENGINEERING.md) |
| Debugging & investigasi bug | [`references/guides/BUG_HUNTER_FRAMEWORK.md`](references/guides/BUG_HUNTER_FRAMEWORK.md) + skill `systematic-debugging` (sibling) |
| Security, SOPS & secrets | [`references/guides/SECURITY_AND_SECRETS.md`](references/guides/SECURITY_AND_SECRETS.md) |
| UI/UX & design system | [`references/guides/UI_UX_DESIGN_SYSTEM.md`](references/guides/UI_UX_DESIGN_SYSTEM.md) |
| Testing & data integrity | [`references/guides/TESTING_AND_INTEGRITY.md`](references/guides/TESTING_AND_INTEGRITY.md) |
| Git workflow & .gitignore | [`references/guides/GIT_HYGIENE.md`](references/guides/GIT_HYGIENE.md) |
| Slide/presentasi dari notes | [`references/guides/SLIDE_AND_PRESENTATION.md`](references/guides/SLIDE_AND_PRESENTATION.md) |
| Distribusi binary CLI | [`references/guides/CLI_DISTRIBUTION.md`](references/guides/CLI_DISTRIBUTION.md) |
| Revamp/merger sistem existing | [`templates/BROWNFIELD_SPEC.md`](templates/BROWNFIELD_SPEC.md) |
| Update state project per task | [`templates/living-docs/README.md`](templates/living-docs/README.md) |
| Setup proses server / runbook | [`templates/ops/README.md`](templates/ops/README.md) + `RUNBOOK.md` |
| Gate sebelum go-live | [`templates/ops/PRODUCTION_READINESS.md`](templates/ops/PRODUCTION_READINESS.md) |

---

## ⚠️ Aturan Mutlak

1. Template bersifat **read-only** — copy ke repo project, isi di sana
2. **JANGAN** tulis data project-specific ke folder skill ini
3. Context window = resource berharga — routing table dulu, baca seperlunya
4. Spec hierarchy: `BRD(Why) → PRD(What) → TechSpec(How) → PLAN.md → Code`
5. Orchestrator pattern: main agent satu-satunya yang write/commit/push; worker subagent read-only
6. **SPEC_SYNC (impact analysis manual):** saat mengubah SATU requirement/section
   di spec manapun, WAJIB sebelum apply:
   - List semua section di spec lain yang terdampak (mis. tambah fitur upload →
     PRODUCT_SPEC backlog + TECH_SPEC storage/API + DESIGN_SPEC component)
   - Tampilkan daftar dampak ke user → apply bersamaan, jangan update 1 dokumen
     lalu lupa sisanya
   - Catat perubahan sebagai entry change-log di repo project (commit message
     atau `CHANGELOG.md`) agar "request → dok terdampak → hasil" terlacak
7. **Language rule:** naratif spec Bahasa Indonesia (kecuali user minta English);
   identifier, schema, nama tabel/field, code, path, error code SELALU English
8. **Secrets boundary (mutlak):** tidak ada markdown di repo project — specs,
   living-docs, maupun ops — yang boleh memuat credential/token/IP/secret aktual.
   Selalu referensi `.env`/vault/secret manager (`See .env: X`, `{value_from_env}`).
   Template pack `living-docs/` & `ops/` sudah membawa aturan ini di README-nya.
