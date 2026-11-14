# Specs — Panduan Workflow Spec-Kit

> **Fungsi:** Folder ini berisi **template spesifikasi** yang menjadi blueprint saat memulai project baru. Template bersifat **read-only** — copy ke repo project, lalu isi sesuai konteks.

---

## Workflow Spesifikasi

$$\text{INTAKE (interview)} \longrightarrow \text{BLUEPRINT\_REVIEW (gate)} \longrightarrow \text{Core Specs} \xrightarrow{\text{/grill-me}} \text{PLAN.md} \longrightarrow \text{Code}$$

## Pilihan Jalur: Rapid / Lean vs Full Enterprise

| Jalur | Template Utama | Cocok Untuk | Waktu Pengerjaan |
|---|---|---|---|
| **Rapid Vibe Coding** | [`LEAN_PRD.md`](LEAN_PRD.md) + [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) | MVP cepat, Micro-SaaS, tools internal, validasi pasar | 15–30 menit |
| **Full Spec-Driven** | `INTAKE` $\rightarrow$ `BLUEPRINT_REVIEW` $\rightarrow$ [`PRODUCT_SPEC.md`](PRODUCT_SPEC.md) + [`TECH_SPEC.md`](TECH_SPEC.md) | Aplikasi skala penuh, sistem multi-aktor, produk klien | 1–3 jam |

## Gate Documents (Step 0)

| # | File | Fokus | Pertanyaan Utama |
|---|------|-------|------------------|
| 0a | [`INTAKE.md`](INTAKE.md) | **Adaptive Interview** | Apa yang belum diketahui dari ide mentah? Roles, business rules, edge cases, integrasi? |
| 0b | [`BLUEPRINT_REVIEW.md`](BLUEPRINT_REVIEW.md) | **Approval Gate** | Apakah aktor, scope fitur, stack, dan open decisions sudah benar sebelum spec ditulis? |

## 3 Template Core (selalu digenerate)

| # | File | Fokus | Pertanyaan Utama |
|---|------|-------|------------------|
| 1 | [`PRODUCT_SPEC.md`](PRODUCT_SPEC.md) | **Why + What + Requirements** | Mengapa dibangun? Untuk siapa? Apa backlog & DoD? |
| 2 | [`TECH_SPEC.md`](TECH_SPEC.md) | **How + Data + Contracts** | Arsitektur? Tech stack? DDL? API? Validasi? Security? |
| 3 | [`DESIGN_SPEC.md`](DESIGN_SPEC.md) | **Look & Feel** | Design tokens? Typography? Component states? Accessibility? |

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

## Cara Pakai

1. **Intake:** jalankan protokol `INTAKE.md` — ide → ekstraksi draft → 5–8 pertanyaan adaptif
2. **Review:** isi `BLUEPRINT_REVIEW.md`, user koreksi & tulis `STATUS: APPROVED`
3. **Copy** template terpilih ke folder `specs/` di repo project baru
4. **Isi** setiap section berdasarkan hasil interview — hapus komentar `<!-- EXAMPLE -->`
5. **Validasi** dengan skill `tanya-dulu` mode GRILL (`/grill-me`) — stress-test asumsi & edge cases; output-nya: istilah → `CONTEXT.md`, keputusan berat → `docs/adr/`, sisanya → balik ke spec
6. **Buat** `PLAN.md` berdasarkan specs yang sudah divalidasi (pecah milestone aktif bila ada `ROADMAP.md`)
7. **Eksekusi** dengan TDD — commit atomik per backlog
8. **Sync:** perubahan requirement → jalankan SPEC_SYNC rule (lihat `../SKILL.md` Aturan Mutlak #6) — list dok terdampak sebelum apply

## Aturan

- **JANGAN** edit template di folder ini — selalu copy ke project repo
- **JANGAN** tambah file baru di folder ini tanpa update `README.md` ini + routing di `../SKILL.md`
- Semua contoh ditandai `<!-- EXAMPLE -->` — ganti dengan data project Anda
- **Security boundary mutlak untuk `living-docs/` dan `ops/`:** dokumen ini masuk
  git repo — DILARANG menulis credential/IP/secret aktual; hanya referensi
  `.env`/vault (detail di `living-docs/README.md` dan `ops/README.md`)
