# Vibe Coding Masterguide

> **Sumber:** Distilasi dari 3 panduan: PANDUAN_VIBE_CODING, VIBE_CODING_PREPARATION_FRAMEWORK, AGENTIC_ENGINEERING_HACKS.
> Baca guide ini sebelum memulai sesi AI coding manapun.

---

## Section 1: Persiapan Sebelum Vibe Coding (5-Layer Stack)

> *"PRD ditulis untuk manusia. AI Agent membutuhkan Hard Constraints, Technical Contracts, dan Step-by-Step Verification."*

Siapkan 5 lapisan ini sebelum membuka IDE atau menjalankan agent:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Agent Rules (AGENTS.md / .cursorrules / CLAUDE.md)       │ → Pagar pembatas & konvensi kode
├─────────────────────────────────────────────────────────────┤
│ 2. Tech Stack Lock (Bahasa, Framework, DB, Package Manager) │ → Tidak boleh berubah mid-sprint
├─────────────────────────────────────────────────────────────┤
│ 3. Spec Hierarchy (PRODUCT_SPEC → TECH_SPEC → DESIGN_SPEC) │ → Blueprint sebelum kode
├─────────────────────────────────────────────────────────────┤
│ 4. Plan File (PLAN.md / task.md — backlog terurut & atomik) │ → Roadmap eksekusi
├─────────────────────────────────────────────────────────────┤
│ 5. Session Management (SESSION.md — context handover)       │ → Memori antar sesi
└─────────────────────────────────────────────────────────────┘
```

**Hierarki Dokumen Wajib:**
$$\text{BRD (Why)} \longrightarrow \text{PRD (What)} \longrightarrow \text{Tech Spec (How)} \xrightarrow{\text{/grill-me}} \text{PLAN.md} \longrightarrow \text{Code}$$

**Aturan Pra-Coding:**
- Lock tech stack di TECH_SPEC sebelum mulai — jangan ganti mid-sprint
- Jangan coding tanpa spec yang sudah di-review dengan `/grill-me`
- Gunakan `SESSION.md` sebagai buffer memori sebelum `/clear` atau ganti sesi

---

## Section 2: Workflow Harian & 14 Tips Operasional

### 2.1 Manajemen Konteks (Anti-Context Rot)

**Context rot** = kondisi saat model mulai mengalami degradasi karena konteks terlalu penuh.

| Gejala | Solusi |
|--------|--------|
| AI mulai memperkenalkan bug baru | `/clear` + buka sesi baru |
| Jawaban mulai berputar/repetitif | Refresh dengan paste `SESSION.md` |
| AI "lupa" keputusan arsitektur | Tambah ke AGENTS.md atau rules |
| Sesi sudah > 2 jam | Simpan progress ke SESSION.md, mulai sesi baru |

**Kapan pakai `/clear`:** Setelah menyelesaikan satu backlog atomik, sebelum memulai backlog baru yang scope-nya berbeda.

### 2.2 14 Tips Harian Vibe Coding

1. **Spec First, Always** — Jangan coding sebelum spec selesai dan di-validate
2. **Satu Backlog = Satu Sesi** — Jangan campurkan scope berbeda dalam satu sesi AI
3. **Commit Atomik** — 1 fitur = 1 commit. Jangan batching banyak perubahan
4. **TDD untuk Logic Kompleks** — Tulis test dulu untuk kalkulasi, validasi, dan edge cases
5. **Model Routing** — Gunakan model cepat untuk boilerplate, model pintar untuk arsitektur
6. **Fresh Session Diff Review** — Review `git diff` di sesi baru sebelum merge ke main
7. **SESSION.md sebagai Handover** — Update sebelum `/clear` atau ganti AI
8. **Anti-Bloat Rule** — Hapus rules yang sudah tidak relevan dari AGENTS.md secara berkala
9. **Screenshot untuk UI Bugs** — Kirim screenshot langsung, bukan deskripsi teks
10. **"Analyze First" Prompt** — Minta AI analisis + plan sebelum langsung coding
11. **Batasi Scope Eksplisit** — Sebutkan apa yang OUT of scope di setiap prompt
12. **Idempotency Check** — Pastikan setiap operasi finansial/stok bisa diulang dengan aman
13. **Error Boundary Wajib** — Semua fungsi eksternal wajib ada error handling
14. **Jangan Trust Blind Merge** — Selalu baca diff sebelum `git merge` atau `git push`

### 2.3 SOP Harian AI Coding

```
Mulai Hari
  ↓ Baca SESSION.md (konteks kemarin)
  ↓ Pilih 1 backlog dari PLAN.md
  ↓ Tulis prompt: "Baca AGENTS.md dan [spec relevan]. Task: [backlog]. Out of scope: [batasan]"
  ↓ Review output — jangan accept blind
  ↓ Run tests
  ↓ Commit atomik
  ↓ Update SESSION.md jika mau stop/ganti konteks
Selesai
```

### 2.4 Tool & Model Alternatif (Cost-Efficient)

| Use Case | Tool Rekomendasi |
|----------|-----------------|
| Boilerplate & scaffolding | Gemini Flash / GPT-4o mini |
| Arsitektur & review kode | Claude Sonnet / Gemini Pro |
| Debug error kompleks | Claude Opus / o3 |
| UI wireframe | v0.dev / Bolt.new |
| CI/CD scripts | Codex CLI / gh Copilot |

---

## Section 3: Advanced Agentic Engineering Hacks

> Sumber: Matt Van Horn — *"80% planning, 20% execution. No IDE. Just plan.md files."*

### 3.1 The Core Philosophy

**Pola pikir 80/20 yang terbalik:**
- ❌ Tradisional: 80% nulis kode manual, 20% planning
- ✅ Agentic: **80% planning terstruktur (`plan.md`)**, 20% eksekusi mekanis oleh agent

Human bertugas sebagai **Human Signal** — bukan yang menulis kode, tapi yang mengarahkan, review, dan memutuskan.

### 3.2 Plan-Driven Development

**Setiap sesi agent harus dimulai dengan:**
1. Buat atau update `plan.md` — list task atomik dengan urutan yang jelas
2. Berikan plan ke agent: *"Baca plan.md ini. Kerjakan task #1. Stop setelah selesai."*
3. Review output sebelum lanjut ke task berikutnya
4. Update plan.md setelah setiap task selesai

**Format plan.md yang efektif:**
```markdown
## Sprint: [Nama Fitur]
- [ ] Task 1: [Deskripsi atomik + acceptance criteria]
- [ ] Task 2: [Deskripsi atomik + acceptance criteria]
- [x] Task 3: DONE — [ringkasan apa yang berubah]
```

### 3.3 Context Window Management

- **Rule of Thumb:** Context > 70% penuh → buat sesi baru dengan summary
- **Anti-Pattern:** Biarkan agent bekerja > 100 iterasi tanpa check-in
- **Pattern:** "Stop and report" setiap milestone kecil

### 3.4 Guardrails Anti-Overengineering

Sebelum agent menulis kode, tanyakan:
1. Apakah ini bisa diselesaikan dengan ≤20 baris?
2. Apakah ini membutuhkan abstraksi baru, atau cukup edit in-place?
3. Apakah agent sedang membuat framework, padahal yang diminta hanya satu fungsi?

Jika jawaban menunjukkan overengineering → reset prompt dengan batasan yang lebih ketat.
