# 🦊 Panduan Disiplin Bug Hunter & Security Research (Fox Framework)

Dokumen ini memformalkan metodologi riset keamanan, triage bug bounty, dan audit kode defensif tingkat lanjut berbasis **Fox — BugHunter Discipline**. Kerangka kerja ini dirancang untuk mencegah **halusinasi AI, false positive, dan laporan sampah (*junk reports*)** melalui 6 gerbang disiplin empiris (*Discipline Gates*).

---

## 🎯 1. Filosofi & Konsep "Soul.md" Persona Fox

Di komunitas AI Agent dan keamanan siber, *System Prompt* atau `soul.md` mendefinisikan batas kognitif, prioritas, dan metodologi kerja sebuah model AI:
* **Tanpa Disiplin:** AI agent sering kali melakukan "overclaim", menganggap pesan error biasa sebagai SQL Injection kritis, atau mengklaim RCE hanya karena server lambat merespons.
* **Dengan Fox Discipline:** Setiap klaim kerentanan wajib melalui pembuktian matematis, telemetri empiris, dan perbandingan respon 3 arah sebelum disimpulkan sebagai celah keamanan valid.

---

## 🔄 2. The 6-Phase BugHunter Engagement Loop

Setiap pengujian atau audit keamanan wajib bergerak melalui 6 siklus bertahap:

```text
┌─────────────────────────────────────────────────────────────┐
│ 1. SCOPE & ATTACK SURFACE DISCOVERY                         │
│    - Verifikasi aset in-scope, rate limit, & auth boundary  │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. HYPOTHESIS & INPUT MAPPING                               │
│    - Pelacakan Source (Input) ──► Sanitizer ──► Sink (DB/OS)│
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. TRIAGE & 7-QUESTION GATE                                 │
│    - Filter eliminasi cepat sebelum menulis PoC             │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. PROOF-OF-CONCEPT & EVIDENCE DISCIPLINE                   │
│    - OOB Verification, Marker Canary, Body-Diff, Stats N=30 │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 5. PRE-SEVERITY GATE (CVSS / VRT)                           │
│    - Penilaian dampak bisnis nyata tanpa inflasi tingkat P  │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 6. REMEDIATION & DEFENSIVE REPORTING                        │
│    - Analisis akar masalah kode & rekomendasi mitigasi kuat │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛡️ 3. Penjelasan 6 Discipline Gates (Anti-False-Positive Engine)

| Gerbang Disiplin | Prinsip Operasional | Standar Verifikasi |
| :--- | :--- | :--- |
| **1. 7-Question Gate** | Checklist eliminasi awal sebelum membuat asumsi celah. | Menjawab 7 parameter: *Asset Scope, Trust Boundary, Vulnerability Primitive, Minimal Trigger, Real Impact, Reproducibility, Safety Policy*. |
| **2. OOB-Or-It-Didn't-Happen** | Bukti mutlak untuk kerentanan *blind* / asinkron. | Tidak boleh mengandalkan status code / delay. Wajib ada bukti interaksi DNS/HTTP ke listener terkontrol (*interactsh/collaborator*) dengan token unik. |
| **3. Marker Discipline** | Kebersihan dan keterlacakan payload (*canary hygiene*). | Wajib menggunakan token acak terisolasi (`fox_canary_<uuid>`). Dilarang memakai payload generik perusak database produksi. |
| **4. Body-Diff Rule** | Eliminasi false positive dari respons dinamis web. | Perbandingan 3 arah ($R_{base}$ normal vs $R_{mut}$ payload vs $R_{ctrl}$ kontrol acak) untuk memastikan respons unik berasal dari payload. |
| **5. Statistical-Sample Rule** | Verifikasi anomali waktu & race condition. | Wajib mengambil minimal $N \ge 30$ sampel baseline dan payload. Selisih waktu harus melewati $\text{Median} + 3\sigma$ ($p < 0.001$). |
| **6. Pre-Severity Gate** | Anti-inflasi tingkat keparahan (*Zero Severity Inflation*). | Mengacu standar Bugcrowd VRT / HackerOne: Memisahkan P1-P4 (dampak bisnis nyata) dari P5 (informational / best-practice). |

---

## 💻 4. Cakupan Domain Teknis

### A. Web & API Security
* **Auth & Session:** OAuth2 state parameter misconfigurations, JWT algorithm confusion (`none` / asymmetric mismatch), SAML replay / signature wrapping.
* **Logic Flaws:** IDOR/BOLA pada UUID/Integer endpoint, Broken Function Level Authorization (BFLA), race condition pada voucher / checkout.
* **Injection Primitives:** SQLi, NoSQLi, Server-Side Template Injection (SSTI), SSRF (termasuk cloud metadata bypass).

### B. Cloud Security & IAM Privesc
* **AWS:** Analisis rantai `sts:AssumeRole`, eksploitasi `iam:PassRole` ke lambda/EC2, IMDSv1 vs IMDSv2 metadata traversal.
* **Azure / Entra ID:** App registrations secret leakage, excessive Microsoft Graph API permissions, consent grant phishing defense.
* **GCP:** Service account impersonation, Workload Identity federation misconfiguration.

### C. Reverse Engineering & Memory Inspection (Defensif / QA)
* Struktur memori proses, pointer chains & multilevel offset dereferencing.
* Analisis mekanisme injeksi proses (`LoadLibrary`, Manual Mapping, Reflective DLL, APC Queueing) untuk perancangan aturan deteksi EDR, pemantauan AMSI, dan pencegahan hook injection.

---

## 📋 5. Contoh Format Laporan Triage Profesional (Bugcrowd/HackerOne Standard)

```markdown
### [P2 - High] IDOR via User Profile Endpoint Leading to PII Exposure

**Target Asset:** `https://api.example.com/v1/users/{id}/billing`  
**Vulnerability Type:** Insecure Direct Object Reference (CWE-639)  
**CVSS v3.1:** 7.5 (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N)  

#### Summary
Endpoint `/v1/users/{id}/billing` tidak memverifikasi apakah identitas token JWT penyerang (Akun B) berhak mengakses catatan penagihan milik pengguna lain (Akun A).

#### Discipline Gate Evidence
1. **7-Question Gate:** Memenuhi seluruh checklist (In-scope, IDOR primitif teridentifikasi, dampak kebocoran PII nyata).
2. **Marker Discipline:** Payload menggunakan canary test user `uuid: 9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d`.
3. **Body-Diff Rule:** $R_{mut}$ mengembalikan data penagihan lengkap Akun A (status 200), sedangkan $R_{ctrl}$ pada ID acak tidak terdaftar mengembalikan 404.

#### Steps to Reproduce
1. Login sebagai Pengguna A dan dapatkan ID profil `100234`.
2. Login sebagai Pengguna B dan buat request cURL:
   ```bash
   curl -s -X GET "https://api.example.com/v1/users/100234/billing" \
     -H "Authorization: Bearer <TOKEN_USER_B>"
   ```
3. Amati bahwa respons mengembalikan detail penagihan Pengguna A.

#### Remediation Recommendation
Tambahkan verifikasi otorisasi di lapisan service (`BillingService.ts`):
```typescript
if (request.currentUser.id !== requestedUserId && !request.currentUser.isAdmin) {
  throw new ForbiddenException("Access Denied");
}
```
```

---

## 🔒 6. Etika & Batasan Operasional
* Hanya lakukan pengujian pada sistem berizin (*authorized whitehat testing / safe sandbox*).
* Utamakan integritas data: Dilarang mengubah atau menghapus data pengguna asli.
* Patuhi prinsip pengungkapan terkoordinasi (*Coordinated Vulnerability Disclosure*).
