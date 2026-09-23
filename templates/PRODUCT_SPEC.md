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

### 1.5 Success Metrics & KPI Bisnis
- **Metrik Utama:** [Contoh: Waktu checkout < 60 detik, retensi 30 hari > 40%]
- **Kapasitas Skalabilitas:** [Contoh: 1.000 transaksi bersamaan saat jam sibuk]

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

### 2.5 Non-Functional & Technical Requirements
- **Platform:** [Web Responsive / Mobile / Desktop]
- **Kinerja:** [Waktu respon < 1 detik]
- **Kompatibilitas:** [Browser modern: Chrome, Safari, Edge, Firefox]

### 2.6 Success Metrics (KPI)
- [Metrik adopsi, retensi, konversi, dll.]

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
* **Deskripsi:** [Deskripsi fungsional]
* **Input Parameters:** [parameter dan tipe data]
* **Output / State Change:** [perubahan state sistem]
* **Edge Cases & Guardrails:** [skenario gagal dan proteksinya]

<!-- EXAMPLE: Backlog e-commerce (Auth, Cart, Checkout) — lihat archive/legacy/specs/SRS.md untuk contoh lengkap. -->

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

**Sukses (`HTTP 200/201`):**
```json
{
  "success": true,
  "data": { ... },
  "meta": { "timestamp": "...", "request_id": "req_uuidv7" }
}
```

**Error (`HTTP 4xx/5xx`):**
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Pesan user-friendly.",
    "details": [{ "field": "...", "issue": "..." }]
  }
}
```

### 3.6 Non-Functional Requirements (NFR)

| Kategori | Target | Keterangan |
| :--- | :--- | :--- |
| **Performa** | p95 < 200ms pada 100 concurrent requests | Endpoint utama |
| **Availability** | 99.9% uptime | Graceful degradation saat service eksternal down |
| **Audit Trail** | Immutable log | Semua mutasi data sensitif dicatat permanen |
