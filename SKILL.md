---
name: spec-kit
description: >-
  Spec-driven development kit milik user: 3 mega-template spesifikasi (PRODUCT/TECH/DESIGN),
  9-section coding standards, dan 10 panduan operasional untuk AI coding agent. Aktif saat
  user memulai project software baru, mengetik "spec-kit", "bikin spec", "tulis PRD",
  "product spec", "tech spec", "design spec", "mulai project baru", "kickoff project",
  "spesifikasi project", meminta dokumen requirements/arsitektur/design system, ATAU
  security research defensif ("bug-hunter", "bug bounty", "triage kerentanan", "audit
  keamanan defensif", "PoC vulnerability", verifikasi celah empiris) — framework Fox
  BugHunter Discipline ada di references/guides/BUG_HUNTER_FRAMEWORK.md.
  Wajib dipakai SEBELUM generate kode project baru — copy template dari templates/ ke repo
  project, jangan edit di sini.
---

# Spec-Kit — Spec-Driven Development Blueprint (milik user)

Skill ini adalah distilasi praktik spec-driven development dari berbagai sumber.
Berisi template spesifikasi + standar engineering + panduan operasional sebagai
cara baku memulai dan mengeksekusi project software bersama AI agent.

---

## 🚀 Bootstrap: Memulai Project Baru (urutan wajib)

### Step 1 — Baca Aturan Coding
Baca [`references/AGENTS.md`](references/AGENTS.md) — standar engineering yang mengikat:
- SRP, fungsi ≤40 baris, early returns, strict typing
- Layered architecture (Presentation / Business / Data)
- Anti-overengineering guardrails & action boundaries
- Git hygiene & secrets management (SOPS + age)

### Step 2 — Pahami 3 Template Spesifikasi
Baca [`templates/README.md`](templates/README.md) untuk workflow 3-template:

| Template | Fokus | Pertanyaan yang Dijawab |
|----------|-------|------------------------|
| [`templates/PRODUCT_SPEC.md`](templates/PRODUCT_SPEC.md) | Why + What + Requirements | Mengapa dibangun? Untuk siapa? Backlog & DoD? |
| [`templates/TECH_SPEC.md`](templates/TECH_SPEC.md) | How + Data + Contracts | Arsitektur? Stack? DDL? API? Validasi? Security? |
| [`templates/DESIGN_SPEC.md`](templates/DESIGN_SPEC.md) | Look & Feel | Design tokens? Typography? Component states? a11y? |

### Step 3 — Generate Spec di Repo Project
Di **repo project baru** (BUKAN di folder skill ini), buat 3 file spec dari template:
1. `PRODUCT_SPEC.md` — konteks bisnis & requirement
2. `TECH_SPEC.md` — arsitektur & kontrak teknis
3. `DESIGN_SPEC.md` — design system

### Step 4 — Validasi Spec
Stress-test spec dengan skill **`grill-me`** (sibling, standalone): interogasi
terarah untuk gali asumsi tersembunyi,
edge cases, dan keputusan arsitektur. Stateful — istilah resolved ditulis ke
`CONTEXT.md` di repo project, keputusan berat (lolos 3 gate) jadi ADR di `docs/adr/`,
sisanya dituangkan kembali ke spec.

### Step 5 — Plan & Eksekusi
`PLAN.md` (backlog atomik, 1 task = 1 commit) → TDD → commit atomik → review via fresh session.

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
| Validasi/interogasi spec | Skill `grill-me` (sibling) — bukan bagian spec-kit |
| Mencegah overengineering | [`references/guides/ANTI_OVERENGINEERING.md`](references/guides/ANTI_OVERENGINEERING.md) |
| Debugging & investigasi bug | Skill `bug-hunter` (sibling) — framework sama dengan `references/guides/BUG_HUNTER_FRAMEWORK.md` |
| Security, SOPS & secrets | [`references/guides/SECURITY_AND_SECRETS.md`](references/guides/SECURITY_AND_SECRETS.md) |
| UI/UX & design system | [`references/guides/UI_UX_DESIGN_SYSTEM.md`](references/guides/UI_UX_DESIGN_SYSTEM.md) |
| Testing & data integrity | [`references/guides/TESTING_AND_INTEGRITY.md`](references/guides/TESTING_AND_INTEGRITY.md) |
| Git workflow & .gitignore | [`references/guides/GIT_HYGIENE.md`](references/guides/GIT_HYGIENE.md) |
| Slide/presentasi dari notes | [`references/guides/SLIDE_AND_PRESENTATION.md`](references/guides/SLIDE_AND_PRESENTATION.md) |
| Distribusi binary CLI | [`references/guides/CLI_DISTRIBUTION.md`](references/guides/CLI_DISTRIBUTION.md) |

---

## ⚠️ Aturan Mutlak

1. Template bersifat **read-only** — copy ke repo project, isi di sana
2. **JANGAN** tulis data project-specific ke folder skill ini
3. Context window = resource berharga — routing table dulu, baca seperlunya
4. Spec hierarchy: `BRD(Why) → PRD(What) → TechSpec(How) → PLAN.md → Code`
5. Orchestrator pattern: main agent satu-satunya yang write/commit/push; worker subagent read-only
