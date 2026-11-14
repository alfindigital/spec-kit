# Security Specification (Satellite Doc)

> **Kapan dipakai:** Domain regulated (fintech/kesehatan), PII sensitif, payment,
> atau multi-tenant — centang di BLUEPRINT_REVIEW §7. Untuk app personal sederhana,
> TECH_SPEC Part 5 sudah cukup — jangan duplikasi.
> **Relasi:** File ini = deep-dive security. TECH_SPEC Part 5 tetap memuat baseline
> (input validation, idempotency, secrets); file ini menambah threat model, data
> classification, dan compliance mapping.

---

## 1. Threat Model Summary

| Aset | Ancaman Utama | Aktor Ancaman | Trust Boundary |
| :--- | :--- | :--- | :--- |
| [mis. data transaksi] | [mis. tampering, leak] | [external / insider] | [client↔API, API↔DB] |
| [mis. kredensial user] | [mis. credential stuffing] | [external] | [auth endpoint] |

**Attack surface utama:** [endpoint publik, webhook, upload, admin panel — daftar singkat]

<!-- EXAMPLE:
| Saldo user | Tampering saldo via replay/double-spend | External + insider | client↔API, API↔DB |
| Webhook payment | Forged "paid" notification | External | Midtrans→API webhook |
Attack surface: POST /api/checkout, POST /webhooks/midtrans, POST /uploads, /admin/*
-->

## 2. Data Classification & Handling

| Kelas Data | Contoh | Penyimpanan | At Rest | In Transit | Retensi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Public | [katalog] | [DB] | — | TLS | — |
| Internal | [preferensi user] | [DB] | — | TLS | [durasi] |
| Confidential | [email, HP, alamat] | [DB + kolom] | encrypted | TLS | [durasi + hapus] |
| Restricted | [KTP, kartu, token pembayaran] | [JANGAN disimpan / vault pihak ketiga] | — | TLS only | — |

> **Aturan mutlak:** data Restricted tidak pernah masuk log, analytics, atau response API
> di luar endpoint otorisasi.

## 3. Authentication & Authorization

- **AuthN:** [metode — OAuth provider / JWT / magic link; MFA bila wajib]
- **Session/Token:** [TTL, rotasi, revocation, storage cookie httpOnly vs bearer]
- **AuthZ matrix:** ref PRODUCT_SPEC 3.1 — tambahan di sini:

| Resource | Rule | Enforcement Point |
| :--- | :--- | :--- |
| [resource] | [mis. owner-only via RLS] | [DB policy / middleware / service] |

## 4. Abuse Cases & Anti-Abuse

| Abuse Case | Vektor | Countermeasure |
| :--- | :--- | :--- |
| [mis. spam akun] | [signup endpoint] | [rate limit + email verify + Turnstile] |
| [mis. enum user ID] | [sequential ID] | [UUIDv7 non-guessable + RLS] |
| [mis. webhook palsu] | [payment webhook] | [signature verify + idempotency] |

## 5. Secrets & Key Management

- Engine: Mozilla SOPS + age (baseline — detail di [`references/guides/SECURITY_AND_SECRETS.md`](../references/guides/SECURITY_AND_SECRETS.md))
- Rotasi: [jadwal/prosedur]
- Yang TIDAK BOLEH masuk repo: [daftar eksplisit]

## 6. Audit & Logging Security

- **Wajib dicatat (immutable):** [login, mutasi saldo, perubahan permission, export data]
- **Dilarang dicatat:** [password, token, Restricted data, full payload PII]
- **Log integrity:** [append-only / hash chain / terpisah dari app DB]

## 7. Compliance Mapping

| Regulasi | Kewajiban | Implementasi di Spec |
| :--- | :--- | :--- |
| [UU PDP / GDPR / OJK] | [consent, right to delete, breach notify] | [section/backlog terkait] |

## 8. Security Testing & Verification

- [ ] Dependency audit di CI (`audit`/`osv-scanner`) — block pada critical
- [ ] AuthZ negative tests (user A tidak bisa akses resource user B)
- [ ] Webhook signature & replay tests
- [ ] Rate-limit & abuse tests pada endpoint publik
- [ ] Secrets-scan pada pre-commit / CI

## 9. Incident Response (minimum)

- **Kontak/escalation:** [siapa yang dipanggil]
- **Langkah pertama saat breach:** [revoke keys → contain → assess scope → notify]
- **Breach notification:** [kewajiban & deadline sesuai regulasi section 7]
