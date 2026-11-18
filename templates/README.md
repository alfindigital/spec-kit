# Specs — Panduan Workflow Spec-Kit

> **Fungsi:** Folder ini berisi **template spesifikasi** yang menjadi blueprint saat memulai project baru. Template bersifat **read-only** — copy ke repo project, lalu isi sesuai konteks.

---

## Pilih paket

Ikuti [aturan pemilihan alur di SKILL.md](../SKILL.md#pilih-alur-sebelum-menulis-spec).

| Alur | Dokumen |
|---|---|
| Lean | [LEAN_PRD.md](LEAN_PRD.md) dan [template plan](IMPLEMENTATION_PLAN.md) untuk satu `PLAN.md` |
| Full | Protokol [INTAKE.md](INTAKE.md), persetujuan [BLUEPRINT_REVIEW.md](BLUEPRINT_REVIEW.md), spec yang dipilih, dan satu `PLAN.md` |

## Dokumen Full

| # | File | Fokus | Pertanyaan Utama |
|---|------|-------|------------------|
| 1 | [`PRODUCT_SPEC.md`](PRODUCT_SPEC.md) | **Why + What + Requirements** | Mengapa dibangun? Untuk siapa? Apa backlog & DoD? |
| 2 | [`TECH_SPEC.md`](TECH_SPEC.md) | **How + Data + Contracts** | Arsitektur? Tech stack? DDL? API? Validasi? Security? |
| Jika perlu mendefinisikan UI | [`DESIGN_SPEC.md`](DESIGN_SPEC.md) | **Look & Feel** | Design tokens? Typography? Component states? Accessibility? |

## Satellite Docs (generate "when needed" — centang di BLUEPRINT_REVIEW §7)

| File | Kapan Dipakai |
|------|---------------|
| [`USER_FLOW.md`](USER_FLOW.md) | Flow multi-aktor/multi-state kompleks |
| [`ROADMAP.md`](ROADMAP.md) | Delivery bertahap >1 milestone — men-feed PLAN.md |
| [`SECURITY.md`](SECURITY.md) | Domain regulated, PII sensitif, payment, multi-tenant |
| [`BROWNFIELD_SPEC.md`](BROWNFIELD_SPEC.md) | Revamp 1:1, merger N:1, adopsi codebase existing — audit sistem lama + gap + strategi migrasi |

## Template Packs (folder — copy per kebutuhan)

| Folder | Kapan Dipakai |
|--------|---------------|
| [`living-docs/`](living-docs/README.md) | Project multi-sesi/agent — dokumen hidup yang di-update tiap task (CURRENT_STATE, TASKS, MODULE_MAP, KNOWN_ISSUES, TECHNICAL_DEBT, VERSION, HANDOVER, …) |
| [`ops/`](ops/README.md) | Project punya proses server — runbook supervisor/systemd/cron/backup/monitoring, topology, production readiness gate |

## Language Rule (formal)

- **Naratif** (penjelasan, requirement, rationale) → **Bahasa Indonesia** (kecuali user minta English)
- **Teknis** (identifier, schema, nama tabel/field, code, path, error code) → **selalu English** — menjaga kompatibilitas compiler/tooling

## Cara pakai

Isi atau perbarui dokumen terpilih di repo proyek. Simpan keputusan dan kriteria
selesai di spec, serta tugas dan bukti pelaksanaannya di `PLAN.md`. Saat requirement
berubah, gunakan SPEC_SYNC dari [SKILL.md](../SKILL.md) untuk meninjau dokumen terdampak.

## Aturan

- **JANGAN** edit template di folder ini — selalu copy ke project repo
- **JANGAN** tambah file baru di folder ini tanpa update `README.md` ini + routing di `../SKILL.md`
- Semua contoh ditandai `<!-- EXAMPLE -->` — ganti dengan data project Anda
- **Security boundary mutlak untuk `living-docs/` dan `ops/`:** dokumen ini masuk
  git repo — DILARANG menulis credential/IP/secret aktual; hanya referensi
  `.env`/vault (detail di `living-docs/README.md` dan `ops/README.md`)
