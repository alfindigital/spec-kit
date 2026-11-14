# Blueprint Review — Gate Sebelum Generate Spec

> **Fungsi:** Ringkasan keputusan hasil [`INTAKE.md`](INTAKE.md) yang WAJIB di-review
> dan di-approve user sebelum agent menulis PRODUCT_SPEC / TECH_SPEC / DESIGN_SPEC.
> Mencegah spec digenerate dari asumsi yang salah.
> **Cara Pakai:** Agent mengisi seluruh bagian dari hasil interview, user mengoreksi
> inline, lalu menulis `STATUS: APPROVED` di bagian akhir.

---

## 1. Identitas Project

- **Nama Project:** [Nama]
- **System Type:** [Personal App / Marketplace / SaaS B2B / Internal Tool / CLI / Bot / dsb.]
- **Tanggal Review:** [YYYY-MM-DD]
- **STATUS:** `DRAFT` ← ubah ke `APPROVED` setelah review selesai

## 2. System Actors / Entities

| Aktor | Peran | Sumber (narasi/interview) |
| :--- | :--- | :--- |
| [Aktor 1] | [Deskripsi wewenang] | [Q1 / narasi] |
| [Aktor 2] | [Deskripsi wewenang] | [Q1 / narasi] |

## 3. Core Feature Scope (checklist MVP)

> Fitur kandidat hasil ekstraksi — user mencentang yang masuk MVP, coret yang tidak.
> Item yang dicoret pindah ke Out-of-Scope di PRODUCT_SPEC 2.2.

- [ ] [Fitur 1]
- [ ] [Fitur 2]
- [ ] [Fitur 3]
- [ ] [Fitur 4]
- [ ] [Fitur 5]

## 4. Decision Log (jawaban interview)

| # | Topik | Pertanyaan | Jawaban | Open Decision? |
| :--- | :--- | :--- | :--- | :--- |
| Q1 | [topic] | [ringkas] | [jawaban testable] | Ya/Tidak |
| Q2 | [topic] | [ringkas] | [jawaban testable] | Ya/Tidak |

<!-- EXAMPLE:
| Q1 | business_rules | Mekanisme booking? | Instant booking; sistem menolak bila slot bentrok | Tidak |
| Q2 | integrations | Payment gateway? | Midtrans QRIS + VA; webhook verify signature | Tidak |
| Q3 | ops | Retensi data user? | Belum diputuskan | Ya — default: 30 hari soft-delete |
-->

**Open Decisions (belum diputuskan — dibawa ke spec sebagai assumption/risiko):**
1. [Keputusan terbuka + opsi default rekomendasi agent]

## 5. Language Preference

- **Naratif dokumen:** [Bahasa Indonesia | English] — penjelasan, requirement, rationale.
- **Teknis:** Selalu English — identifier, schema, nama tabel/field, code, path, error code.
  (Menjaga kompatibilitas compiler/tooling; tidak bisa diubah.)

## 6. Technology Stack Proposal

> Agent mengusulkan stack + alasan. User: `Accept All` atau koreksi per-baris.
> Stack mengikuti constraint project, bukan tren — lihat justifikasi tiap pilihan.

| Layer | Rekomendasi | Justifikasi | Keputusan User |
| :--- | :--- | :--- | :--- |
| Frontend | [mis. React / Next.js / Vue / Svelte] | [alasan] | Accept / Custom: ___ |
| Backend | [mis. Node+Express / Django / Go+Gin / Bun+Elysia] | [alasan] | Accept / Custom: ___ |
| Database | [mis. PostgreSQL / MongoDB / SQLite / MySQL] | [alasan] | Accept / Custom: ___ |
| Auth | [mis. Firebase Auth / Auth0 / JWT custom / OAuth] | [alasan] | Accept / Custom: ___ |
| Hosting/Deploy | [mis. Vercel / VPS / Cloudflare / Netlify] | [alasan] | Accept / Custom: ___ |
| Integrasi kunci | [mis. Stripe / Midtrans / Firebase RTDB / S3] | [alasan] | Accept / Custom: ___ |

## 7. Dokumen yang Akan Digenerate

> Bundle adaptif per kompleksitas — core selalu ada, satellite hanya bila relevan.

**Core (selalu):**
- [ ] `specs/PRODUCT_SPEC.md`
- [ ] `specs/TECH_SPEC.md`
- [ ] `specs/DESIGN_SPEC.md`

**Satellite (centang bila perlu):**
- [ ] `specs/USER_FLOW.md` — bila flow multi-aktor/multi-state kompleks
- [ ] `specs/ROADMAP.md` — bila delivery bertahap >1 fase/milestone
- [ ] `specs/SECURITY.md` — bila domain regulated, PII sensitif, payment, atau multi-tenant

## 8. Sign-off

- [ ] Semua section di atas sudah dikoreksi user
- [ ] Open decisions punya default atau dicatat eksplisit
- [ ] `STATUS: APPROVED` ditulis user di section 1

> Setelah APPROVED → agent generate spec sesuai bundle section 7 → lanjut validasi `grill-me`.
