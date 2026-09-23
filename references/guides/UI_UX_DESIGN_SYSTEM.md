# 🎨 Panduan & Blueprint Prompt UI/UX, Design System & Agentic Frontend

Dokumentasi ini merangkum **8 Blueprint Prompt & Workflow Eksekusi AI Coding Agent** (seperti Claude Code, Antigravity, Codex, Cursor) yang dirancang untuk menghasilkan antarmuka pengguna (UI/UX) berstandar industri (*anti-slop*), modular, aksesibel, dan konsisten.

---

## 📌 8 Blueprint Prompt Utama (Verbatim & Siap Pakai)

### 1. Design System (Pull Scattered UI into a Component Library)
> **Task:** abstract the scattered UI components, style tokens, and repeated interactions in this project into one unified Design System, a reusable and maintainable component library. Replace the hand-rolled one-off components with library components, route all new UI through the library with no bypassing, and add a review skill at the end. This is a refactor, so start in a worktree. For this task, write yourself a new end-to-end /goal: complete the whole plan, not just the next step, until the architecture, implementation, tests, review, and final result meet the standard. Split that goal into independent pieces, spawn as many parallel agents as needed to do it better and faster, and give each agent its own dedicated /goal that includes its expected deliverable, verification, and completion standard. Dispatch them concurrently, keep tracking progress in the right place, synthesize results as they return, resolve conflicts, continue implementation, run real-time validation after important steps, and finish with review, submission/commit when appropriate, and a final summary. Validation should cover the real end-to-end path, including browser/computer use, clicks, keyboard actions, and any necessary operation. Do not stop after partial progress unless blocked by missing credentials, destructive ambiguity, or conflicting requirements.

---

### 2. Accessibility (a11y) + Multi-Device
> **Task:** make sure regular, low-vision, keyboard-only, and different-device users can all reliably complete the core actions. Start the app locally from the current commit, walk the core paths at desktop / tablet / phone viewports, and fix responsive issues (overlap / overflow / truncation / horizontal scroll), contrast, font scaling, keyboard (Tab / Enter / Space / Esc), focus states, and the a11y semantics of images / icon buttons / forms / status. Record the issues, the fixes, and the remaining risks, and add a review skill at the end. For this task, write yourself a new end-to-end /goal: complete the whole plan, not just the next step, until the architecture, implementation, tests, review, and final result meet the standard. Split that goal into independent pieces, spawn as many parallel agents as needed to do it better and faster, and give each agent its own dedicated /goal that includes its expected deliverable, verification, and completion standard. Dispatch them concurrently, keep tracking progress in the right place, synthesize results as they return, resolve conflicts, continue implementation, run real-time validation after important steps, and finish with review, submission/commit when appropriate, and a final summary. Validation should cover the real end-to-end path, including browser/computer use, clicks, keyboard actions, and any necessary operation. Do not stop after partial progress unless blocked by missing credentials, destructive ambiguity, or conflicting requirements.

---

### 3. Standar DESIGN.md (Google Stitch Concept)
> **DESIGN.md** is a concept from Google Stitch: a plain-text design system that spells out color, type scale, spacing, radius, shadow, and component rules, so the agent reads it before every UI build and the style stops drifting. AGENTS.md is how to build it, DESIGN.md is how it should look. Don't want to write one? Steal it. Someone scraped 73 real ones (Stripe / Linear / Airbnb / Vercel) into http://github.com/VoltAgent/awesome-design-md. Drop one in your root and tell the agent to make all UI follow DESIGN.md.

---

### 4. UX Audit (Let It See the State First)
> Act as a senior UX designer auditing this app's core flows. Walk the real user path end to end. Find: weak information hierarchy, friction, missing feedback, error-prone steps, empty and error states that were never built. Rate each by severity and give a concrete fix.

---

### 5. Rebuild One Page with Real Taste
> Act as a product designer and rebuild this page to feel like Linear / Stripe. Pin down the style first: a restrained palette, a clear type scale, enough whitespace, consistent radius and shadow. Then build. Only touch the visuals, no new features, and walk me through every change.

---

### 6. Get Every Interaction and State Right
> Act as a senior frontend engineer and get every interaction and state right. Cover: loading skeletons, empty states, error states, hover / focus / disabled, plus transitions and feedback on the key actions. Make it reusable and accessible, then deliver the component structure and full implementation.

---

### 7. Build a Landing Page that Converts
> Act as a conversion-minded designer and build a landing page. Nail the one-line value prop and the single action you want from the visitor first. Then lay out: hero, social proof, feature sections, FAQ, CTA. Clean visuals, clear hierarchy, mobile-first. Deliver a complete, shippable page.

---

### 8. Make the Form Usable
> Act as a senior frontend engineer and make this form usable. Cover: real-time validation, clear inline errors, sensible defaults, keyboard and autofill friendly, submitting and success / failure states, long forms split into steps. Deliver the full implementation.

---

## 🔍 Fact Check & Analisis Teknis Komunitas

Berdasarkan telaah kritis terhadap klaim, referensi link, dan diskusi komunitas seputar prompt ini:

| Topik / Klaim | Status | Analisis & Verifikasi Faktual |
| :--- | :---: | :--- |
| **Konsep DESIGN.md (Google Stitch)** | **VALID** | DESIGN.md adalah spesifikasi *plain-text design system* yang diinisiasi dalam ekosistem Google Stitch / agentic coding. LLM sangat efektif membaca token dalam format markdown sederhana dibandingkan file Figma binary atau CSS masif. AGENTS.md mengatur *how to build* (logika & arsitektur), sedangkan DESIGN.md mengatur *how it should look* (visual & estetika). |
| **Repo VoltAgent/awesome-design-md** | **VALID** | Repositori GitHub VoltAgent/awesome-design-md adalah katalog publik nyata dan open-source (gratis) yang mengumpulkan 70+ template DESIGN.md dari produk seperti Stripe, Linear, Vercel, dan Airbnb. Tuduhan bahwa tautan tersebut  scam/berbayar terbukti **salah/misinformasi**. |
| **LLM & Taste Estetika Bawaan** | **VALID** | Model AI (Claude, GPT, Gemini) secara bawaan dioptimasi untuk penyelesaian instruksi fungsional dan kecepatan, bukan memenangkan penghargaan desain. Tanpa batasan token warna dan skala font spesifik, AI akan menghasilkan AI slop (desain generik). Menyuplai DESIGN.md memberikan pagar pembatas konkret agar AI tidak menebak-nebak (*style drifting*). |
| **Git Worktree & Subagent Isolation** | **BEST PRACTICE** | Mengisolasi refactor besar ke *git worktree* dan mendelegasikan verifikasi e2e ke subagent paralel mencegah rusaknya branch utama dan memastikan testing responsivitas serta aksesibilitas tervalidasi secara riil. |

---

## 🛠️ Ringkasan Pola & Aturan Eksekusi Agent

1. **Pemisahan Peran Markdown:**
   * AGENTS.md: Aturan teknis koding, modularitas, boundary checks, dan alur build/test.
   * DESIGN.md: Acuan visual (palet warna, tipografi, spacing, radius, shadow, state komponen).
2. **Isolasi Refactor:**
   * Selalu gunakan git worktree saat melakukan refactor UI besar-besaran agar kode branch utama tetap stabil.
3. **Penyelesaian End-to-End (/goal):**
   * Perintahkan agent untuk menyelesaikan seluruh siklus (plan -> build -> test -> review -> commit) secara otonom.
4. **Verifikasi Nyata (Browser / Computer Use):**
   * Validasi bukan sekadar kode lulus linting, melainkan interaksi nyata (klik tombol, input form, responsivitas multi-device, navigasi keyboard Tab/Enter).
