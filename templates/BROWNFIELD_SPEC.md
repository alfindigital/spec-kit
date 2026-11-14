# Brownfield / Revamp Specification (Satellite Doc)

> **Kapan dipakai:** Project BUKAN greenfield — revamp 1:1 (rewrite sistem lama),
> merger N:1 (gabung beberapa app ke satu), atau adopsi codebase existing.
> Centang di BLUEPRINT_REVIEW §7.
> **Relasi:** Tetap generate 3 core specs — file ini menambah audit sistem lama,
> gap analysis, dan strategi transisi yang tidak dicover spec greenfield.
> **Cara Pakai:** Copy ke repo project sebagai `specs/BROWNFIELD_SPEC.md`.
> Section 5 (Merger) hanya diisi untuk skenario N:1 — hapus jika revamp biasa.

---

## 1. Audit Sistem Lama

### 1.1 Identitas

| Item | Detail |
| :--- | :--- |
| Nama sistem | [nama] |
| Stack | [framework + versi per layer: BE / FE / DB / runtime] |
| Repository | [path atau URL] |
| URL production | [url] |
| Ukuran DB | [approx] |

### 1.2 Inventaris Modul & Pain Level

| # | Modul | Status | User terdampak | Pain level | Catatan |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | [nama] | [Active/Deprecated] | [estimasi] | [Low/Med/High] | [masalah] |

<!-- EXAMPLE: modul "Laporan" — High pain, timeout di dataset >50k rows, jadi
alasan utama revamp. Modul "Legacy import" — Deprecated, jangan dimigrasi. -->

### 1.3 Kenapa Revamp (bukan refactor)

- [alasan konkret — mis. "framework EOL, upgrade path putus", "pisah BE/FE untuk scale"]
- [alasan — mis. "tech debt > biaya rewrite"]

### 1.4 Integrasi Existing yang Harus Survive

| Integrasi | Tipe | Kritis? | Catatan |
| :--- | :--- | :--- | :--- |
| [payment gateway] | API | ✅ | [kontrak yang tidak boleh berubah] |

## 2. Gap Analysis

### 2.1 Feature Gap

| Modul | Sistem lama | Sistem baru | Aksi |
| :--- | :--- | :--- | :--- |
| [modul] | ✅ ada | ✅ dipertahankan | Migrasi |
| [modul] | ✅ ada | ❌ dibuang | Drop — alasan: [kenapa] |
| [modul] | — | ✅ baru | Build |
| [modul] | ✅ basic | ✅ ditingkatkan | Improve — [apa yang berubah] |

### 2.2 Data Gap

| Tabel lama | Tabel baru | Strategi |
| :--- | :--- | :--- |
| [users] | [users] | Direct map / transform / drop — detail di §4 |
| [tabel] | — | Drop — alasan: [kenapa] |

### 2.3 API / Contract Gap

| Endpoint/kontrak lama | Baru | Catatan konsumen |
| :--- | :--- | :--- |
| [GET /admin/users] | [GET /api/v1/users] | [siapa yang masih konsumsi endpoint lama] |

## 3. Strategi Migrasi

- **Pendekatan terpilih:** [Big Bang / Phased / Parallel run / Strangler fig]
- **Alasan:** [kenapa pendekatan ini untuk konteks ini]

### Phase Plan

| Fase | Aktivitas | Exit criteria |
| :--- | :--- | :--- |
| 0 — Preparation | Audit + env baru + API contract | [verifiable] |
| 1 — Foundation | Auth, layout, CI/CD | [verifiable] |
| 2 — Core modules | Modul prioritas dari gap §2.1 | [verifiable] |
| 3 — Remaining | Sisa modul | [verifiable] |
| 4 — Cutover | Final data sync + DNS/traffic switch + freeze sistem lama | [verifiable] |

### Rollback

- **Trigger:** [kondisi kuantitatif — mis. error rate >5%, login gagal, data corrupt]
- **Prosedur:** [langkah mundur — DNS balik, restore, dsb + estimasi waktu]
- **Batas aman:** [sampai fase mana rollback masih murah]

## 4. Migrasi Data

> **Secrets:** koneksi DB lama/baru WAJIB via env/secret manager — DILARANG
> menulis host+credential aktual di file ini. Gunakan label (`OLD_DB`, `NEW_DB`).

### 4.1 Urutan & Mapping Tabel

> Master dulu, baru transaksi. Satu blok mapping per tabel yang butuh transform.

| # | Tabel lama → baru | Rows | Strategi |
| :--- | :--- | :--- | :--- |
| 1 | [users → users] | [n] | [direct / transform] |

`[old_table] → [new_table]`:

| Kolom lama | Kolom baru | Transform rule |
| :--- | :--- | :--- |
| [status tinyint] | [status varchar] | `0→inactive, 1→active` |
| [password md5] | — | SKIP — re-hash bcrypt via seeder/reset flow |

### 4.2 Verifikasi per Tabel

| Check | Query/metode | Expected |
| :--- | :--- | :--- |
| Row count | `SELECT count(*)` | = sumber |
| Required fields | `WHERE [field] IS NULL` | 0 |
| FK intact | [join check] | no orphan |
| Checksum sampling | `sha256` pada export | match |

### 4.3 Rollback data

[TRUNCATE + re-run / restore snapshot — prosedur eksplisit]

## 5. Merger N:1 (hanya jika menggabung beberapa app)

### 5.1 Inventaris Aplikasi

| App | Peran | Stack | Users | Nasib |
| :--- | :--- | :--- | :--- | :--- |
| A | Target (dipertahankan) | [stack] | [n] | Hidup |
| B | Sumber | [stack] | [n] | Drop pasca-migrasi |

### 5.2 Overlap & Konflik

| Fitur/data | Ada di | Resolusi |
| :--- | :--- | :--- |
| [user management] | A, B | [single source di A, migrasi B via mapping] |
| [nama route/model] | A, B | [rename sementara selama transisi] |

### 5.3 Decommission

- [kapan B boleh dimatikan — setelah verifikasi apa]
- [arsip data B — dump final + lokasi arsip]

## 6. Risk Register

| Risiko | Likelihood | Impact | Mitigasi |
| :--- | :--- | :--- | :--- |
| [data loss saat migrasi] | [Med] | [Critical] | [backup sebelum tiap batch + dry-run] |
| [user bingung UI baru] | [High] | [Med] | [komunikasi + panduan] |

---

> **Handoff:** Brownfield spec yang APPROVED men-feed core specs — TECH_SPEC
> memuat arsitektur target; gap §2 men-feed backlog PRODUCT_SPEC; phase §3
> men-feed ROADMAP.md/PLAN.md.
