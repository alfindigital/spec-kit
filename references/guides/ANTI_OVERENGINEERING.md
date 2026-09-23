# 🛡️ Panduan Anti-Overengineering & Agent Orchestration (Sol-Class Guardrails)

Dokumentasi ini merangkum prinsip, aturan batas kerja (*action boundaries*), dan arsitektur orkestrasi subagent berdasarkan diskusi praktisi (*Discord Community Insight*) seputar penggunaan model AI *high-reasoning* (Sol/Luna/o-series/Claude/Gemini) dalam *vibe coding* dan *agentic engineering*.

---

## 📌 Filosofi Inti: "Sol-Class Trap"

> *"Sol-class models don't write wrong code. They write too much code.*  
> *You ask for a config change, you get a framework. You fix a bug, you get an adapter layer.*  
> *Tests pass, CI green, looks professional. Then you touch one thing next week and there are four layers between you and the logic."*

Model penalaran tinggi (*high reasoning / high thinking effort*) memiliki kelemahan psikologis bawaan: **mereka selalu ingin membuktikan kepintaran dengan membuat arsitektur berlapis-lapis**. Kode yang dihasilkan seringkali lulus semua tes otomatis (CI/CD hijau), namun meninggalkan utang teknis (*technical debt*) yang sangat parah karena abstraksi prematur.

---

## 1. Modus Kegagalan AI (*Failure Modes*)

Berikut adalah 6 kebiasaan buruk AI yang wajib diwaspadai dan dicegah:

| No | Modus Kegagalan | Manifestasi / Gejala | Dampak Buruk |
| :--- | :--- | :--- | :--- |
| **1** | **Surface Patching** | Memperbaiki gejala di permukaan tanpa memahami *root cause* asli. | Menumpuk tambalan, *compat layers*, atau implementasi ganda. |
| **2** | **Over-Designing Edge Cases** | Merancang sistem untuk kasus ekstrem/langka yang belum tentu pernah terjadi. | Biaya perawatan sehari-hari menjadi sangat mahal dan lambat. |
| **3** | **Wrong Premise Trap** | Memulai dari asumsi atau premis yang salah sejak awal. | Tidak ada jumlah penalaran yang bisa memperbaiki titik awal yang salah. |
| **4** | **Guessing over Reading** | Menebak atau melakukan pencarian (*grep/search*) acak berulang tanpa membaca kode sumber langsung. | Halusinasi struktur file dan penambahan layer redundan. |
| **5** | **Test-Covered Scope Creep** | Menyelipkan abstraksi/fitur baru berkedok "menambahkan unit test agar aman". | Lingkup tugas membengkak tanpa disadari. |
| **6** | **Dual Logic Maintenance** | Membuat fungsi/class baru paralel untuk mempertahankan kode lama yang seharusnya di-refactor di tempat. | *Duplicate logic*, ketidakkonsistenan state. |

---

## 2. Batas Tindakan (*Action Boundaries & Stop Signals*)

Sebelum AI mulai mengedit atau membuat file, wajib melakukan **Validasi 4 Kuadran**:
1. **What the user actually wants:** Apa inti masalah yang diminta pengguna?
2. **Scope for this task:** Berapa minimal file yang perlu diubah?
3. **What's explicitly out of scope:** Batasan hal-hal yang TIDAK boleh disentuh.
4. **Definition of Done (DoD):** Kapan tugas dinyatakan tuntas tanpa menambah pekerjaan baru?

### 🛑 Sinyal Berhenti Segera (Stop Signals):
Hentikan eksekusi dan lakukan evaluasi ulang jika AI mendapati dirinya:
* Menambahkan *abstractions*, *frameworks*, atau *config layers* yang tidak diminta secara eksplisit.
* Menyentuh file di luar skop tiket/instruksi pengguna.
* Menumpuk *constraints* baru hanya untuk memuaskan *constraints* yang dibuat sebelumnya.
* Mengubah lebih dari 2–3 file ketika perbaikan bug cukup dilakukan pada 1–2 baris.
* Menggunakan dalih penambahan unit test untuk terus membangun kode baru.

---

## 3. Disiplin Pengujian (*Testing Boundaries*)

> **Aturan Emas:** Tes berfungsi **hanya** untuk membuktikan penerimaan perubahan saat ini (*acceptance criteria*). Tidak lebih, tidak kurang.

* **Utamakan Tes yang Sudah Ada:** Jika suite test yang ada sudah mencakup logika yang diubah dan lulus (PASS), **dilarang membuat file test baru**.
* **Kapan Test Baru Diizinkan?**
  1. Perubahan mengubah perilaku (*behavior*) yang belum di-cover oleh test lama, **ATAU**
  2. Pengguna meminta penulisan test secara eksplisit.
* **Batas Maksimal Test Baru:**
  * Maksimal **1 Main Path** (alur sukses utama) + **1 Critical Failure Path** (alur kegagalan kritis).
* **Larangan Keras dalam Testing:**
  * ❌ Dilarang memperluas cakupan test demi mengejar "kelengkapan teoretis".
  * ❌ Dilarang melakukan *backfill testing* pada modul lain yang tidak berhubungan.
  * ❌ Dilarang menambahkan *test framework* atau infrastruktur testing baru jika proyek sudah memiliki harness sendiri.
  * ❌ Dilarang membuat matriks snapshot raksasa atau *parameterized grids* berlebihan.
  * ❌ Dilarang menguji implementasi internal (*private functions*) jika kontrak publik tidak berubah.
* **Pertanyaan Wajib Sebelum Menulis Test:**
  > *"Kriteria penerimaan (requirement) mana yang secara spesifik diverifikasi oleh test ini?"*

---

## 4. Arsitektur Agent Orchestration (Multi-Agent Setup)

Saat menangani dataset besar, audit kode masif, atau investigasi bug lintas-repo, menggunakan 1 sesi chat tunggal akan menyebabkan *context degradation* (memori penuh dan halusinasi).

Gunakan arsitektur **Hub-and-Spoke (Orchestrator vs Read-Only Workers)**:

```
                          ┌───────────────────────────┐
                          │   MAIN AGENT / LEAD       │
                          │   (Interactive / State)   │
                          └─────────────┬─────────────┘
                                        │
           ┌────────────────────────────┼────────────────────────────┐
           │ (Spawn Read-Only)          │ (Spawn Read-Only)          │ (Spawn Read-Only)
           ▼                            ▼                            ▼
┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
│  Worker 1: Herschel  │     │   Worker 2: Mill     │     │   Worker 3: Kepler   │
│  - Audit Data Schema │     │  - Trace Call Graph  │     │  - Log / Evidence    │
│  - STRICTLY READ-ONLY│     │  - STRICTLY READ-ONLY│     │  - STRICTLY READ-ONLY│
└──────────┬───────────┘     └──────────┬───────────┘     └──────────┬───────────┘
           │                            │                            │
           └────────────────────────────┼────────────────────────────┘
                                        │ (Lapor Temuan / Sintesis)
                                        ▼
                          ┌───────────────────────────┐
                          │ Rekonsiliasi Hasil Temuan │
                          │ Tulis File / Artefak Akhir│
                          │ Git Commit & Push         │
                          └───────────────────────────┘
```

### Karakteristik Peran:
1. **Main Agent (Orchestrator):**
   * Satu-satunya entitas yang memiliki izin tulis (*write access*).
   * Merekonsiliasi temuan dari semua subagent.
   * Menulis kode final, mengupdate dokumentasi, dan melakukan `git commit` serta `git push`.
2. **Worker Subagents (Herschel, Mill, dll):**
   * Diberi batasan ketat: **`STRICTLY READ-ONLY`**.
   * Dilarang mengedit file, dilarang menjalankan migrasi database destruktif, dilarang membuat branch/worktree tanpa mandat.
   * Bertugas murni mengumpulkan bukti (*evidence capture*), memetakan relasi data, atau mengaudit log secara paralel.

---

## 5. Strategi Efisiensi Token & Biaya (Hybrid Tier Setup)

Untuk memaksimalkan paket langganan (seperti ChatGPT Plus atau Claude Pro) tanpa cepat kehabisan kuota:

| Lapisan Pekerjaan | Platform Terbaik | Mode / Model | Alasan Efisiensi |
| :--- | :--- | :--- | :--- |
| **Brainstorming & Planning** | Web UI (ChatGPT / Claude) | High Reasoning (o-series / Sonnet) | Kuota web melimpah; cocok untuk menyusun PRD, FRD, dan RFC tanpa membakar token API lokal. |
| **Audit PR via GitHub** | Web UI / Integrasi GitHub | Standard / Web Search | Membaca diff besar secara online tanpa membebani disk lokal. |
| **Local Coding & File Edit** | Local CLI (Codex / Antigravity) | High Reasoning + Tool Use | Hanya dipanggil saat butuh menyentuh file lokal, menjalankan compiler, dan uji coba real-time. |
| **Data Mass Audit** | Local Multi-Agent | Subagents (Read-Only) | Memecah beban konteks agar tidak ada satu prompt yang menghabiskan token context window. |

---

## 6. Template Snippet untuk `AGENTS.md` Proyek Lain

Jika Anda menginisialisasi repositori baru, salin blok berikut ke `AGENTS.md` Anda:

```markdown
## Anti-Overengineering Guardrails
- Core Rule: "Sol-class models don't write wrong code. They write too much code."
- Never build abstractions, wrappers, or config layers unless explicitly requested.
- Before coding: define exact scope, out-of-scope, and definition of done.
- Root-cause fix: modify 1-2 lines in-place instead of piling on patches.
- Testing limits: max 1 main path + 1 critical failure path. If existing tests pass, do not add new test files.
- Subagents: exploration and audits must run in STRICTLY READ-ONLY mode.
```