# Guides — Index & Routing Table

> Jangan baca semua guide sekaligus. Gunakan routing table di bawah untuk menemukan yang relevan.
> Semua guide adalah panduan **teori/best practice** — bukan dokumentasi project spesifik.

---

## Routing Table — Baca Guide Berdasarkan Situasi

| Situasi | Guide yang Harus Dibaca |
|---------|------------------------|
| Mulai project baru dari 0 | `VIBE_CODING_MASTERGUIDE.md` + `specs/README.md` |
| Membangun multi-agent workflow | `AGENT_ARCHITECTURE_AND_WORKFLOW.md` |
| Ada terlalu banyak abstraksi/framework | `ANTI_OVERENGINEERING.md` |
| Ada bug sulit di-trace | `BUG_HUNTER_FRAMEWORK.md` |
| Setup secrets & credentials | `SECURITY_AND_SECRETS.md` |
| Build UI/design system | `UI_UX_DESIGN_SYSTEM.md` + `specs/DESIGN_SPEC.md` |
| Setup testing strategy | `TESTING_AND_INTEGRITY.md` |
| Setup .gitignore untuk repo baru | `GIT_HYGIENE.md` |
| Buat slide/presentasi dari notes | `SLIDE_AND_PRESENTATION.md` |
| Ship CLI tool/binary | `CLI_DISTRIBUTION.md` |

---

## Daftar Lengkap Guides (10 files)

### 🔵 Workflow & Process
| File | Isi Utama | Ukuran |
|------|-----------|--------|
| [`VIBE_CODING_MASTERGUIDE.md`](VIBE_CODING_MASTERGUIDE.md) | 5-Layer prep stack, 14 daily tips, agentic hacks (Matt Van Horn 80/20), SOP harian | ~6.8KB |
| [`AGENT_ARCHITECTURE_AND_WORKFLOW.md`](AGENT_ARCHITECTURE_AND_WORKFLOW.md) | Orchestrator pattern, Cursor workflow 18 best practices, /grill-me pointer (skill standalone), memory layers | ~5.7KB |
| [`ANTI_OVERENGINEERING.md`](ANTI_OVERENGINEERING.md) | Sol-class trap guardrails, stop signals, action boundaries, testing limits | ~9.6KB |

### 🔴 Quality & Security
| File | Isi Utama | Ukuran |
|------|-----------|--------|
| [`BUG_HUNTER_FRAMEWORK.md`](BUG_HUNTER_FRAMEWORK.md) | Discipline debugging, root cause analysis, reproducible steps | ~9KB |
| [`SECURITY_AND_SECRETS.md`](SECURITY_AND_SECRETS.md) | SOPS+Age setup, prompt injection defense, minimal-permission principle | ~3.5KB |
| [`TESTING_AND_INTEGRITY.md`](TESTING_AND_INTEGRITY.md) | TDD for AI-generated code, security test patterns, integrity checks | ~7.1KB |

### 🟢 UI & Design
| File | Isi Utama | Ukuran |
|------|-----------|--------|
| [`UI_UX_DESIGN_SYSTEM.md`](UI_UX_DESIGN_SYSTEM.md) | Design system prompting, component states, a11y checklist | ~8KB |

### 🟡 Tooling & Infrastructure
| File | Isi Utama | Ukuran |
|------|-----------|--------|
| [`GIT_HYGIENE.md`](GIT_HYGIENE.md) | Reverse allowlist .gitignore template, setup steps | ~2.3KB |
| [`SLIDE_AND_PRESENTATION.md`](SLIDE_AND_PRESENTATION.md) | Reverse prompting untuk slide deck dari notes mentah | ~8.5KB |
| [`CLI_DISTRIBUTION.md`](CLI_DISTRIBUTION.md) | Binary lite distribution, cross-platform packaging | ~8.3KB |

---

## Cara Baca Guide

1. Identifikasi situasimu di routing table di atas
2. Buka 1-2 guide yang paling relevan
3. Jangan baca semuanya — context window adalah resource berharga
4. Jika perlu multiple guides, baca summary/heading dulu sebelum full content
