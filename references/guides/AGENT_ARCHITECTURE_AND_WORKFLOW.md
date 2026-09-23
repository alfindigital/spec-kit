# Agent Architecture & Multi-Agent Workflow

> **Sumber:** Distilasi dari CURSOR_AGENTIC_WORKFLOW_GUIDE, FRONTIER_AGENT_OS, GRILL_ME_CODEX_WORKFLOW.
> Gunakan guide ini saat mengerjakan task yang melibatkan multi-agent orchestration atau workflow kompleks.

---

## Section 1: Prinsip Dasar Agentic Workflow

### 1.1 Anatomy Agent Modern

Setiap AI Agent yang efektif memiliki 4 komponen inti:

```
┌─────────────────────────────────────────┐
│ AGENT = LLM + Memory + Tools + Identity │
├─────────────────────────────────────────┤
│ LLM      → Model reasoning             │
│ Memory   → Context window + vector DB  │
│ Tools    → File ops, bash, browser     │
│ Identity → System prompt / AGENTS.md   │
└─────────────────────────────────────────┘
```

### 1.2 Pola Orchestration: Orchestrator + Workers

**Aturan Fundamental:**
- **Main Orchestrator** = satu-satunya entitas yang WRITE ke file, commit, dan push
- **Worker Subagents** = read-only, riset, analisis, dan explorasi SAJA
- Worker DILARANG mutasi file, edit config, atau pollute working tree

```
[User] → [Orchestrator] → delegate → [Worker A: Research]
                        → delegate → [Worker B: Analysis]
                        ← gather ←  [Worker A: Result]
                        ← gather ←  [Worker B: Result]
                        → WRITE FILES, COMMIT, PUSH
```

### 1.3 Hierarki Keputusan

| Keputusan | Siapa yang Memutuskan |
|-----------|----------------------|
| Arsitektur sistem | Human + Orchestrator |
| Implementasi detail | Worker / Orchestrator |
| File write & commit | Orchestrator only |
| Deploy ke production | Human only |

---

## Section 2: Cursor Agentic Workflow (Praktik Lapangan)

### 2.1 Mode Agent vs Mode Chat

| Mode | Gunakan Untuk | Hindari Untuk |
|------|---------------|---------------|
| **Agent Mode** | Multi-file edits, scaffolding, refactoring | Review arsitektur, brainstorming |
| **Chat Mode** | Review, debug reasoning, planning | Direct file edits (bisa context-drift) |
| **Composer** | Spesifik file edits dengan kontrol penuh | Large-scale refactoring |

### 2.2 Prompt Engineering untuk Agentic Task

**Template prompt yang efektif:**
```
Konteks: [Baca AGENTS.md dan specs/TECH_SPEC.md]
Task: [Deskripsi task atomik — 1 fungsi/1 modul]
Batasan (Out of Scope): [Daftar apa yang TIDAK boleh diubah]
Acceptance Criteria: [Kondisi yang harus terpenuhi agar DONE]
Stop Condition: [Kapan harus berhenti dan report ke user]
```

### 2.3 Anti-Pattern yang Harus Dihindari

1. **Infinite Agent Loop** — Agent terus berputar tanpa stop condition → selalu tambah "stop after completing X and report"
2. **Context Bleed** — Agent membawa konteks proyek A ke proyek B → gunakan `/clear` antar project
3. **Blind Merge** — Menerima diff tanpa review → selalu `git diff` sebelum commit
4. **Scope Creep by Agent** — Agent tambah fitur yang tidak diminta → `AGENTS.md` harus eksplisit soal scope

---

## Section 3: Frontier Agent OS — Konsep Sistem Agen Modern

### 3.1 Memory Architecture

```
┌──────────────────────────────────────┐
│           AGENT MEMORY LAYERS        │
├──────────────────────────────────────┤
│ L1: Context Window (Active)          │ ← Percakapan saat ini
│ L2: SESSION.md (Short-term)          │ ← Handover antar sesi
│ L3: AGENTS.md (Long-term Rules)      │ ← Rules permanen
│ L4: Vector DB / Hindsight (Semantic) │ ← Knowledge base terpersist
└──────────────────────────────────────┘
```

### 3.2 Tool Hierarchy

Urutan prioritas saat agent memilih tool:
1. **Read-only tools dulu** (file_read, grep) — pahami konteks sebelum mutasi
2. **Targeted writes** (edit specific lines) — bukan rewrite seluruh file
3. **Command execution** (bash, npm, git) — hanya jika diperlukan
4. **Network calls** — terakhir, hanya jika tidak ada alternatif lokal

### 3.3 Hybrid Cost & Token Strategy

| Skenario | Tool yang Digunakan |
|----------|---------------------|
| Planning & spec writing | Chat web (Claude Web / ChatGPT Plus) — hemat API token |
| File edits & execution | Local CLI (Antigravity / Claude Code / Codex) |
| PR review | GitHub web + AI Copilot — bukan local API |
| Debug production | Local high-reasoning model |

---

## Section 4: /grill-me — Validasi Spec Sebelum Coding

> **Sudah jadi skill standalone** — protokol lengkap ada di skill `grill-me`
> (sibling, di library skill yang sama). Section ini hanya ringkasan integrasi.

Gunakan `/grill-me` saat: setelah spec baru ditulis, sebelum implementasi/`PLAN.md`,
setelah major refactoring decision, atau saat ada ambiguitas/asumsi tersembunyi.

Yang divalidasi: asumsi tersembunyi, edge cases, keputusan arsitektur,
dependency & blast radius, Definition of Done.

**Output stateful** (ditulis saat sesi berjalan, bukan di akhir):
- Istilah resolved → `CONTEXT.md` (glossary, di repo project)
- Keputusan lolos 3 gate (hard to reverse + surprising + real trade-off) → `docs/adr/ADR-NNNN-*.md`
- Sisa keputusan → dituangkan kembali ke spec sebelum sesi ditutup
