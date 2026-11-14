# Implementation Plan — [Nama Proyek / Fitur]

> **Tujuan:** Panduan eksekusi atomik untuk AI coding agent (Claude Code / Antigravity / Cursor / Kiro).
> **Aturan:** 1 Task = 1 Perubahan Teruji. Dilarang lanjut ke task berikutnya sebelum verifikasi lolos.

---

## 1. Konteks & Ringkasan Perubahan

* **Fitur / Proyek:** [Nama Fitur]
* **Target Outcome:** [Hasil akhir yang diharapkan]
* **Dokumen Rujukan:** [`specs/PRD.md`](specs/PRD.md) atau [`specs/TECH_SPEC.md`](specs/TECH_SPEC.md)

---

## 2. Rencana Eksekusi Bertahap

### Fase 1: Setup & Groundwork
- [ ] **Task 1.1: [Nama Task]**
  * *Tindakan:* Buat struktur folder / pasang dependency.
  * *Files terdampak:* `package.json`, `src/config.ts`
  * *Perintah verifikasi:* `npm run build`

### Fase 2: Database & Business Logic
- [ ] **Task 2.1: [Nama Task]**
  * *Tindakan:* Tulis skema DDL / migrasi database.
  * *Files terdampak:* `src/db/schema.ts`
  * *Perintah verifikasi:* `npx drizzle-kit push` atau query uji
- [ ] **Task 2.2: [Nama Task]**
  * *Tindakan:* Implementasi controller / handler logika inti.
  * *Files terdampak:* `src/services/action.ts`
  * *Perintah verifikasi:* Jalankan unit test / script test scratch

### Fase 3: Interface & Integrasi End-to-End
- [ ] **Task 3.1: [Nama Task]**
  * *Tindakan:* Sambungkan API ke komponen UI frontend.
  * *Files terdampak:* `src/components/MainForm.tsx`
  * *Perintah verifikasi:* Buka di browser, lakukan form submission

### Fase 4: Hardening & Rilis
- [ ] **Task 4.1: [Nama Task]**
  * *Tindakan:* Bersihkan console.log sisa debug, audit zero-secret.
  * *Perintah verifikasi:* `git diff` review
- [ ] **Task 4.2: [Nama Task]**
  * *Tindakan:* Deploy ke lingkungan live.
  * *Perintah verifikasi:* `curl -fsS https://<domain>/api/health`

---

## 3. Catatan Masalah & Mitigasi (Issue Tracker)

| Issue / Edge Case | Solusi / Keputusan | Status |
|---|---|---|
| *Contoh: Auth token expire* | *Auto-refresh via interceptor* | *Resolved* |
