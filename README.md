# spec-kit

**Spec-driven development kit untuk AI coding agent** — 3 mega-template spesifikasi,
standar engineering 9-section, dan 11 panduan operasional sebagai cara baku memulai
dan mengeksekusi project software bersama AI agent.

A distilled spec-driven development kit for AI coding agents: spec templates,
engineering standards, and operational guides that turn "vibe coding" into a
repeatable pipeline.

## Struktur

```
spec-kit/
├── SKILL.md                    # Entry point skill — bootstrap workflow
├── templates/
│   ├── PRODUCT_SPEC.md         # WHY + WHAT: BRD + PRD + SRS + DoD (satu file)
│   ├── TECH_SPEC.md            # HOW: arsitektur, stack, ERD/DDL, API contract,
│   │                           #       validasi, security, testing
│   ├── DESIGN_SPEC.md          # Look & feel: design tokens, typography,
│   │                           #       component states, a11y
│   ├── gitignore.allowlist     # Reverse-allowlist .gitignore (default *, !path)
│   └── README.md               # Panduan workflow 3-template
└── references/
    ├── AGENTS.md               # Standar engineering yang mengikat
    └── guides/                 # 11 panduan operasional (routing table di dalam)
```

## Pipeline

```
PRODUCT_SPEC → TECH_SPEC → DESIGN_SPEC → /grill-me → PLAN.md → Code (TDD)
```

`/grill-me` adalah skill pendamping **opsional** (tidak termasuk repo ini):
interogasi terarah untuk stress-test spec sebelum coding. Alternatifnya:
interogasi manual — gali asumsi tersembunyi, edge cases, dependency & blast
radius, dan Definition of Done.

## Install

Copy folder ini ke direktori skill agent kamu, atau via skills installer:

```bash
npx skills@latest add alfindigital/spec-kit
```

Lalu di repo project baru, jalankan bootstrap di `SKILL.md`:
1. Baca `references/AGENTS.md` (aturan coding)
2. Copy 3 template ke `specs/` repo project, isi sesuai konteks
3. Validasi spec (grill/interogasi)
4. `PLAN.md` backlog atomik → eksekusi TDD → commit atomik

## Prinsip

- **Spec hierarchy:** `BRD (Why) → PRD (What) → Tech Spec (How) → PLAN → Code`
- **Templates read-only** — copy ke repo project, jangan edit di sini
- **Context window = resource** — routing table dulu, baca seperlunya
- **1 backlog = 1 commit** — atomic, reviewable, revertable

## License

[MIT](LICENSE)
