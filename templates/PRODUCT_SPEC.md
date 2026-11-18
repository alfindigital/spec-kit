# Product Specification Template

> **Dokumen Terpadu:** Menggabungkan Business Requirements (WHY), Product Requirements (WHAT), dan Software Requirements (REQUIREMENTS & DoD) dalam satu file.
> **Cara Pakai:** Copy file ini ke repo project baru, lalu isi setiap section sesuai konteks project Anda. Hapus komentar `<!-- EXAMPLE -->` setelah diganti.

---

## Part 1: Business Requirements (WHY)

> Menjawab pertanyaan **"WHY"** — alasan bisnis, regulasi, dan model finansial. Agar AI Agent tidak mengarang aturan bisnis.

### 1.1 Executive Summary & Business Objectives
- **Nama Proyek/Inisiatif:** [Nama Proyek]
- **Latar Belakang Masalah Bisnis:** [Masalah apa yang dihadapi pasar/organisasi?]
- **Tujuan Finansial & Operasional:** [Contoh: Menurunkan churn rate 15%, meningkatkan revenue per user]
- **Target Audiens & Pasar:** [Siapa segmen target pengguna?]

### 1.2 Business Rules & Financial Logic (Aturan Mutlak)
*Tuliskan aturan logika bisnis yang TIDAK BOLEH dilanggar:*
- **Monetisasi & Pricing:** [Tier harga, skema subscription, komisi, batas diskon]
- **Batasan Transaksi:** [Limit per hari, batas minimal penarikan, kuota free tier]
- **Refund & Pembatalan:** [Kondisi refund, masa retur produk]

<!-- EXAMPLE: "Diskon promo maksimal 20%, tidak dapat digabung dengan flash sale." — Ganti dengan aturan bisnis project Anda. -->

### 1.3 User Roles, Hierarchy & Permissions

| Role | Tanggung Jawab | Izin Otoritas | Batasan Mutlak |
| :--- | :--- | :--- | :--- |
| **[Role 1]** | [Deskripsi] | [Izin] | [Batasan] |
| **[Role 2]** | [Deskripsi] | [Izin] | [Batasan] |

<!-- EXAMPLE: Contoh role matrix e-commerce:
| Super Admin | Pemilik sistem | Akses penuh audit & config | Tidak boleh transaksi fiktif |
| Customer | Pembeli akhir | Beli, bayar, lihat transaksi sendiri | Tidak bisa lihat data user lain |
-->

### 1.4 Compliance, Security & Legal Constraints
- **Perlindungan Data Pribadi:** [UU PDP / GDPR: data KTP, HP, email wajib dienkripsi]
- **Audit Trail:** [Setiap mutasi saldo/uang dicatat ke immutable log, tanpa hard delete]
- **Regulasi Industri:** [OJK/BI untuk fintech, HIPAA untuk kesehatan, dll.]

### 1.5 Success Metrics & KPI (Canonical)

> **SINGLE SOURCE** — semua metrik keberhasilan (bisnis DAN produk) didefinisikan di sini.
> Part lain cukup mereferensikan bagian ini; jangan duplikasi tabel KPI di tempat lain.

| Metrik | Target | Kategori |
| :--- | :--- | :--- |
| [Metrik 1] | [Target terukur] | Bisnis / Produk |
| [Metrik 2] | [Target terukur] | Bisnis / Produk |

<!-- EXAMPLE:
| Waktu checkout | < 60 detik | Produk |
| Retensi 30 hari | > 40% | Bisnis |
| Kapasitas jam sibuk | 1.000 transaksi bersamaan | Operasional |
-->

- **Kapasitas Skalabilitas:** [Contoh: 1.000 transaksi bersamaan saat jam sibuk]

### 1.6 Risk Analysis & Mitigation

> Identifikasi risiko PALING MAHAL lebih dulu — bukan daftar laundry semua kemungkinan.
> Setiap risiko wajib punya mitigation konkret, bukan "akan diperhatikan".

| Risiko | Impact | Probabilitas | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| [Risiko 1: mis. inkonsistensi data antar sistem] | [High/Med/Low] | [High/Med/Low] | [Strategi konkret: pola, fallback, monitoring] |
| [Risiko 2: mis. kebocoran data lintas user] | [High/Med/Low] | [High/Med/Low] | [Strategi konkret] |
| [Risiko 3: mis. biaya pihak ketiga melonjak] | [High/Med/Low] | [High/Med/Low] | [Strategi konkret] |

<!-- EXAMPLE:
| PostgreSQL dan Firebase RTDB drift | High | Medium | Transactional outbox, idempotent writes, rekonsiliasi harian |
| Cross-user data leak via query salah | High | Low | RLS, security rules, integration test wajib |
| Biaya push notification meledak | Medium | Medium | Cap event per user, batch update, alert budget |
-->

---

## Part 2: Product Requirements (WHAT)

> Menjawab pertanyaan **"WHAT"** — persona, scope MVP, dan user journey. Kontrak fungsional antara Product Owner dan AI Agent.

### 2.1 Product Overview & Goals
- **Nama Produk / Fitur:** [Nama]
- **Tujuan Utama:** [Masalah pengguna yang diselesaikan]
- **Target Persona:** [Siapa pengguna akhir?]

### 2.2 Scope & Boundaries
- **In-Scope:** [Daftar pekerjaan WAJIB diselesaikan dalam rilis ini]
- **Out-of-Scope:** [Daftar hal yang TIDAK dikerjakan — mencegah scope creep]

### 2.3 Minimum Viable Product (MVP) Features
1. **[Fitur 1]**: [Deskripsi fungsi dan alur]
2. **[Fitur 2]**: [Deskripsi fungsi dan alur]
3. **[Fitur 3]**: [Deskripsi fungsi dan alur]

### 2.4 User Stories & Acceptance Criteria
- **US-01:** Sebagai [Role], saya ingin [Aksi], sehingga [Manfaat].
  - **Acceptance Criteria:**
    - [ ] [Kondisi 1 yang harus terpenuhi]
    - [ ] [Kondisi 2 yang harus terpenuhi]

### 2.5 Constraints & Assumptions

> Bedakan tegas: **Constraint** = batasan yang tidak bisa dilanggar (teknis/legal/bisnis).
> **Assumption** = hal yang diasumsikan benar — kalau salah, spec harus direvisi.
> Mencatat assumption secara jujur lebih baik daripada memaksakan kepastian palsu.

**Constraints:**
- [Batasan 1: mis. online-only, tidak ada offline mode]
- [Batasan 2: mis. budget hosting < Rp500rb/bulan]
- [Batasan 3: mis. harus jalan di serverless execution limit]

**Assumptions:**
- [Asumsi 1: mis. user punya koneksi internet stabil]
- [Asumsi 2: mis. provider pihak ketiga sudah dikonfigurasi sebelum launch]
- [Asumsi 3: mis. semua timestamp disimpan UTC, ditampilkan timezone lokal]

<!-- EXAMPLE: "Asumsi: user memberikan izin notifikasi browser." — kalau tidak,
fitur push reminder harus punya fallback in-app badge. -->

### 2.6 Catatan Lokasi Kanonik

- **NFR** (performa, availability, security target) → didefinisikan di **Part 3.6**, bukan di sini.
- **KPI & metrik sukses** → didefinisikan di **Part 1.5**, bukan di sini.
- **Target device & responsive breakpoint** → **DESIGN_SPEC.md §4**; **kompatibilitas minimum** (browser/runtime versi, OS) → tulis sebagai constraint di **§2.5** atas.
- **Kontrak API envelope** (success/error shape) → canonical di **TECH_SPEC.md §3.1** — spec ini hanya menuntut envelope konsisten ada, bukan mendefinisikannya ulang.

---

## Part 3: Software Requirements & Definition of Done

> Mendetailkan **backlog fungsional**, relasi antar-backlog, edge cases, dan **Definition of Done** yang ketat sebelum kode dieksekusi.

### 3.1 Matriks Aktor & Peran Sistem

| Role ID | Nama Aktor | Deskripsi Wewenang | Batasan Akses |
| :--- | :--- | :--- | :--- |
| `ACT-01` | [Aktor 1] | [Wewenang] | [Batasan] |
| `ACT-02` | [Aktor 2] | [Wewenang] | [Batasan] |

<!-- EXAMPLE:
| `ACT-01` | Guest / Anonymous | Pengunjung publik belum login | Hanya read-only katalog |
| `ACT-02` | Customer / Member | Pengguna terdaftar | Akses cart, checkout, profile |
| `ACT-03` | Admin / Operator | Pengelola internal | CRUD master data, audit log |
-->

### 3.2 Rincian Backlog Fungsional

#### Backlog `BL-001`: [Nama Backlog]
* **Parent Epic:** [EPIC-XXX]
* **Aktor:** [ACT-XX]
* **Sisi:** [User-facing | System-facing]
* **Deskripsi:** [Deskripsi fungsional]
* **Input Parameters:** [parameter dan tipe data]
* **Output / State Change:** [perubahan state sistem]
* **Edge Cases & Guardrails:** [skenario gagal dan proteksinya]

> **Aturan Sisi:**
> - **User-facing** — aksi yang bisa dilakukan aktor lewat UI/API publik (create task, filter, snooze reminder).
> - **System-facing** — kewajiban sistem yang TIDAK pernah jadi user story: enforcement security (RLS, JWT verify), sinkronisasi antar-service, retry/fallback, soft-delete lifecycle, rate limiting, audit trail.
> System-facing backlog sering lolos kalau spec cuma berisi user stories — wajib didaftarkan eksplisit agar agent tidak mengabaikannya.

<!-- EXAMPLE System-facing:
`BL-015` — Sisi: System-facing — "System me-retry pengiriman push notification 1x; setelah gagal, munculkan in-app badge."
`BL-016` — Sisi: System-facing — "System menegakkan isolasi data per owner via row-level security."
-->

<!-- EXAMPLE: Daftar backlog untuk e-commerce biasanya mencakup Auth (BL-001), Cart (BL-002), Checkout (BL-003), Webhook Payment (BL-004, System-facing). -->

### 3.3 Dependency Matrix

AI Agent **dilarang mengeksekusi** backlog hilir sebelum backlog hulu memenuhi DoD:

| Backlog ID | Prasyarat | Dibutuhkan Oleh | Blast Radius |
| :--- | :--- | :--- | :--- |
| `BL-001` | - (Fondasi) | BL-002 | [Tinggi/Sedang/Rendah] |
| `BL-002` | BL-001 | BL-003 | [Tinggi/Sedang/Rendah] |

### 3.4 Definition of Done (DoD) — Standar Universal

Suatu backlog dinyatakan **DONE** hanya jika memenuhi SELURUH checklist:

#### A. Kode & Arsitektur
- [ ] **SRP & Ukuran Fungsi:** Tidak ada fungsi > 30–40 baris
- [ ] **Layering Compliance:** Logika bisnis tidak bocor ke UI atau API controller
- [ ] **Type-Safety:** 100% strict typing, tanpa `any`
- [ ] **Zero Hardcoded Values:** Semua konstanta diekstrak ke enum

#### B. Pengujian & Keamanan
- [ ] **Unit Tests Passing:** Semua fungsi kalkulasi & validasi lulus tes
- [ ] **Integration Test:** Alur end-to-end database terbukti berhasil
- [ ] **Input Sanitization:** Bebas SQLi, XSS, command injection
- [ ] **Error Boundary:** Semua skenario gagal mengembalikan pesan terstruktur

#### C. Dokumentasi & Git
- [ ] **Git Working Tree Bersih:** Tidak ada file debug atau `.env` bocor
- [ ] **Checkpoint Handover:** `SESSION.md` diperbarui sebelum sesi ditutup

### 3.5 Kontrak API Response (Envelope Standar)

> **SINGLE SOURCE** — definisi envelope canonical ada di `TECH_SPEC.md §3.1`
> (Standard Response Envelope). Spec ini hanya menetapkan REQUIREMENT-nya:
>
> - [ ] Semua endpoint memakai envelope sukses/error yang konsisten (bukan shape ad-hoc per endpoint)
> - [ ] Error response membawa `code` machine-readable + `message` user-friendly + `details` field-level
> - [ ] Sukses membawa `meta.request_id` untuk traceability
>
> Definisi schema/format persis → lihat TECH_SPEC Part 3. Jangan definisikan ulang di sini.

### 3.6 Non-Functional Requirements (NFR)

| Kategori | Target | Keterangan |
| :--- | :--- | :--- |
| **Performa** | p95 < 200ms pada 100 concurrent requests | Endpoint utama |
| **Availability** | 99.9% uptime | Graceful degradation saat service eksternal down |
| **Audit Trail** | Immutable log | Semua mutasi data sensitif dicatat permanen |
