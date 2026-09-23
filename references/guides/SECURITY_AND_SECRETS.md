# Security & Secrets Management

> **Sumber:** Distilasi dari PANDUAN_KEAMANAN_AGENT_SKILL_DAN_INJECTION + SOPS_SECRETS_MANAGEMENT.
> Baca sebelum menyentuh credentials, environment variables, atau deployment pipeline.

---

## Part 1: Keamanan Agent, Skill & Prompt Injection

### 1.1 Ancaman Utama AI Agent

| Ancaman | Deskripsi | Mitigasi |
|---------|-----------|----------|
| **Prompt Injection** | Input user/eksternal memanipulasi instruksi agent | Sanitasi input, validasi boundary ketat |
| **Tool Abuse** | Agent dimanipulasi untuk eksekusi tool berbahaya | Whitelist tools, minimal-permission principle |
| **Data Exfiltration** | Agent bocorkan data sensitif via output | Output filtering, no-secret-in-logs rule |
| **Supply Chain Attack** | Skill/plugin dari sumber tidak terpercaya | Hanya install skill dari sumber terverifikasi |

### 1.2 Prinsip Minimal Permission

- Agent hanya boleh akses tool yang diperlukan untuk task saat ini
- **Read-only by default** — write permission hanya jika eksplisit dibutuhkan
- Subagent worker **tidak boleh** write ke file system atau eksekusi command berbahaya
- Log semua tool calls agent untuk audit

### 1.3 Input Validation & Sanitization

Semua input dari user atau sumber eksternal wajib:
1. **Type validation** — via Zod schema atau equivalent sebelum masuk domain logic
2. **Sanitization** — strip HTML/script tags untuk input teks
3. **Length limiting** — batas karakter untuk mencegah DoS via context overflow
4. **Allowlist approach** — definisikan apa yang diizinkan, bukan apa yang dilarang

---

## Part 2: SOPS Secrets Management

### 2.1 Mengapa SOPS?

Masalah dengan pendekatan tradisional:
- `.env` files → sering ter-commit ke git secara tidak sengaja
- Environment variables → tidak ter-audit, tidak ter-versioned
- Secret manager eksternal → dependency tambahan, latency

**Solusi:** Mozilla SOPS + Age encryption → secret tetap di git, tapi terenkripsi.

### 2.2 Setup SOPS + Age

```bash
# Install Age (key generator)
# Windows: winget install FiloSottile.age
# macOS: brew install age

# Generate Age keypair
age-keygen -o ~/.age/key.txt

# Public key ada di output, simpan ke .sops.yaml
```

**`.sops.yaml` di root project:**
```yaml
creation_rules:
  - path_regex: secret\.yaml$
    age: >-
      age1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 2.3 Workflow Secret

```bash
# Enkripsi secret baru
sops --encrypt secret.yaml > secret.enc.yaml

# Edit secret terenkripsi
sops secret.enc.yaml

# Dekripsi untuk dibaca (JANGAN commit hasil dekripsi)
sops --decrypt secret.enc.yaml
```

**`secret.yaml` (sebelum enkripsi — struktur):**
```yaml
database:
  url: postgresql://user:password@host:5432/dbname
  pool_size: 10
api_keys:
  stripe: sk_live_xxxxxxxxxxxxx
  sendgrid: SG.xxxxxxxxxxxxx
```

### 2.4 CI/CD Integration

```yaml
# GitHub Actions — inject secret dari SOPS
- name: Decrypt secrets
  env:
    SOPS_AGE_KEY: ${{ secrets.AGE_PRIVATE_KEY }}
  run: |
    sops --decrypt secret.enc.yaml > secret.yaml
    # Mask values
    cat secret.yaml | grep -E "^\s+\w+:" | while read line; do
      value=$(echo $line | cut -d':' -f2 | tr -d ' ')
      echo "::add-mask::$value"
    done
```

### 2.5 Aturan Mutlak

- **JANGAN** hardcode API key, DB password, atau token di source code
- **JANGAN** commit `.env` (tambahkan ke `.gitignore`)
- **JANGAN** izinkan agent membaca file kredensial mentah (`.env`, `key.txt`, `secret.yaml` terdekripsi, private key) — agent cukup tahu nama variabel/placeholder, bukan nilainya
- **SELALU** enkripsi `secret.yaml` sebelum commit
- **SELALU** masukkan private key Age ke secret manager CI/CD (bukan commit ke git)
- **JANGAN** log nilai secret di console/stdout
