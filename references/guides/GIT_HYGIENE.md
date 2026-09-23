# Git Hygiene & .gitignore Best Practices

> **Sumber:** REVERSE_GITIGNORE_TEMPLATE.md
> Gunakan guide ini saat setup repo baru atau debugging git tracking issues.

---

## Reverse Allowlist Pattern (.gitignore)

### Mengapa Reverse Allowlist?

**Masalah pendekatan tradisional (blacklist):**
- Default: semua file di-track git
- Mudah lupa tambahkan file sensitif baru ke `.gitignore`
- Token bleed — credential ter-commit tanpa sengaja

**Solusi: Reverse/Allowlist Pattern:**
- Default: **SEMUA file di-ignore**
- Hanya file yang eksplisit di-allowlist yang di-track
- Jauh lebih aman untuk project dengan AI Agent

### Template .gitignore (Reverse Allowlist)

```gitignore
# ============================================================
# REVERSE ALLOWLIST .gitignore
# Default: ignore EVERYTHING
# Only explicitly unignored files are tracked
# ============================================================

# 1. Ignore everything by default
*

# 2. Unignore directories (needed to traverse into them)
!src/
!src/**/
!tests/
!tests/**/
!docs/
!docs/**/
!public/
!public/**/
!scripts/
!scripts/**/

# 3. Unignore specific file types
!*.ts
!*.tsx
!*.js
!*.jsx
!*.json
!*.md
!*.sql
!*.yaml
!*.yml
!*.toml
!*.css
!*.html
!Dockerfile
!Makefile
!*.sh

# 4. Re-ignore sensitive files (even if matched above)
# Secrets & credentials
secret.yaml
secret.enc.yaml
*.env
.env
.env.*
*.key
*.pem
*.p12
*.pfx

# Build outputs
node_modules/
dist/
build/
.next/
.cache/
coverage/

# AI/IDE artifacts
.claude/
*.backup
*.tmp
*.log

# OS artifacts
.DS_Store
Thumbs.db
desktop.ini
```

### Cara Implementasi

```bash
# Setup reverse allowlist di repo baru
echo "*" > .gitignore
echo "!src/" >> .gitignore
# ... lanjutkan dengan struktur project

# Verifikasi apa yang akan di-track
git status --short

# Pastikan file sensitif tidak ter-track
git check-ignore -v secret.yaml  # Harus ter-ignore
git check-ignore -v src/index.ts # Harus NOT ter-ignore
```

### Aturan untuk AI Agent

Saat AI agent melakukan `git status`, file yang tidak seharusnya ter-track mudah ketahuan karena defaultnya semua sudah ter-ignore. Ini mencegah:
- `.env` yang berisi credential ter-commit
- File `secret.yaml` yang terdekripsi ter-commit
- Folder `node_modules` yang massive ter-commit
- File log/backup sementara ter-commit
