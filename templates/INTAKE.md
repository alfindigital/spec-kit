# Intake & Adaptive Interview — Step 0 Spec-Kit

> **Fungsi:** Mengubah ide 1 paragraf menjadi konteks terstruktur SEBELUM spec ditulis.
> Agent tidak boleh langsung mengarang spec dari ide mentah — keputusan tersembunyi
> harus digali dulu lewat interview adaptif.
> **Cara Pakai:** Copy file ini ke repo project baru, jalankan protokol, lalu hasilnya
> dirangkum di [`BLUEPRINT_REVIEW.md`](BLUEPRINT_REVIEW.md) untuk gate persetujuan.

---

## Protokol Interview (urutan wajib)

### Fase 1 — Idea Input
Minta user mendeskripsikan produk dalam bahasa natural:
- Apa yang dibangun? Untuk siapa? Masalah apa yang diselesaikan?
- Constraint yang sudah diketahui (budget, deadline, stack wajib, regulasi).

> Tidak perlu format formal. 1 paragraf cukup — sisanya digali lewat pertanyaan.

### Fase 2 — Ekstraksi Draft (oleh Agent)
Dari deskripsi mentah, agent mengekstrak kandidat awal (belum final):
- **System type:** [personal app / marketplace / SaaS B2B / internal tool / dsb.]
- **Kandidat aktor:** [role yang terdeteksi dari narasi]
- **Kandidat fitur:** [fitur yang disebut eksplisit]
- **Kandidat constraint:** [batasan yang disebut eksplisit]
- **Knowledge gaps:** [daftar keputusan yang BELUM dijawab narasi — inilah bahan pertanyaan]

### Fase 3 — Pertanyaan Adaptif (5–8 soal)
Agent menyusun pertanyaan **hanya untuk gap yang belum terjawab**. Jangan tanyakan
yang sudah jelas dari narasi. Format tiap pertanyaan:

```
TOPIC: <roles | business_rules | edge_cases | integrations | ops | scope | ux | data>
Q<n>: <pertanyaan spesifik, bukan generik>
  a) <opsi konkret + implikasi 1 kalimat>
  b) <opsi konkret + implikasi 1 kalimat>
  c) <opsi konkret + implikasi 1 kalimat>
  d) [custom] — jawab dengan kata sendiri bila opsi tidak cocok
```

<!-- EXAMPLE:
TOPIC: business_rules
Q3: Bagaimana mekanisme booking bekerja?
  a) Instant booking — user langsung terkonfirmasi, sistem menolak bila bentrok
  b) Request & approval — merchant menyetujui manual, ada SLA respon
  c) Subscription slot — user berlangganan slot berulang mingguan
  d) [custom]
-->

**Aturan pertanyaan:**
1. Maksimal 8 soal per ronde; boleh ronde ke-2 HANYA bila jawaban ronde 1 membuka gap baru yang material.
2. Tiap opsi jawaban menyebut implikasinya (biaya, kompleksitas, UX) — bukan opsi abstrak.
3. Prioritaskan gap dengan blast radius terbesar: model bisnis > data/auth > integrasi > edge case > kosmetik.
4. "Saya tidak tahu / belum diputuskan" adalah jawaban SAH — catat sebagai **Open Decision**, jangan dipaksa.

### Fase 4 — Jawaban Testable
Jawaban yang berguna mendeskripsikan perilaku observable, bukan kata sifat:
- ✅ "Admin dapat suspend akun; user yang di-suspend tidak bisa memulai sesi baru."
- ❌ "Admin punya kontrol penuh."

Bila jawaban user abstrak, agent wajib menanyakan follow-up konkret ATAU menuliskannya
sebagai Open Decision dengan opsi default yang direkomendasikan.

### Fase 5 — Rangkuman ke Blueprint Review
Semua hasil (ekstraksi draft + jawaban + open decisions) dituangkan ke
[`BLUEPRINT_REVIEW.md`](BLUEPRINT_REVIEW.md). User me-review dan mengoreksi
SEBELUM spec digenerate — jangan skip gate ini.

---

## Mapping Jawaban → Spec (traceability)

| Topik Jawaban | Masuk ke |
| :--- | :--- |
| roles, scope, business_rules | PRODUCT_SPEC Part 1 & 3 |
| edge_cases, ops | PRODUCT_SPEC 3.2 backlog + TECH_SPEC Part 5 |
| integrations | TECH_SPEC 1.4 + 5.x |
| ux, platform | DESIGN_SPEC |
| data | TECH_SPEC Part 2 |
| Open Decisions | CONTEXT.md project + risiko di PRODUCT_SPEC 1.6 |

## Larangan
- Jangan generate spec sebelum BLUEPRINT_REVIEW di-approve.
- Jangan tanya hal yang sudah dijawab narasi user — itu membuang kesabaran user.
- Jangan terjemahkan jawaban abstrak menjadi "kepastian" di spec; pertahankan sebagai Open Decision.
