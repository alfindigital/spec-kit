# Production Readiness Checklist

> Gate sebelum go-live / merge ke main untuk rilis. Semua item harus ✅ atau
> diberi alasan eksplisit kenapa N/A. Kalau ada ❌ → jangan rilis.

## Secrets & Config

- [ ] Tidak ada credential/token/IP aktual di repo (`git log -p | grep` pola secret)
- [ ] `.env` production di server, `.env.example` punya semua key yang dipakai
- [ ] Secret rotation policy terdokumentasi
- [ ] Debug mode OFF di production config

## Aplikasi

- [ ] Semua endpoint mutating punya input validation
- [ ] Security headers aktif (CSP/HSTS/X-Frame-Options)
- [ ] Rate limit di endpoint publik & auth
- [ ] Error tidak leak stack trace ke client
- [ ] Test suite passing + build sukses

## Operasional

- [ ] Semua background process punya runbook (`docs/ops/`)
- [ ] Backup terjadwal + restore pernah ditest (bukan cuma script jalan)
- [ ] Log rotation aktif
- [ ] Health check endpoint ada dan termonitor
- [ ] Alert ke channel yang dibaca manusia

## Deploy & Rollback

- [ ] Deploy via git pull / CI — bukan upload manual
- [ ] Rollback procedure ditulis dan ditest minimal sekali
- [ ] Migration DB punya rollback path atau dinyatakan irreversible eksplisit
- [ ] `TOPOLOGY.md` dan `HANDOVER.md` up-to-date

## Sign-off

| Peran | Nama | Tanggal | Status |
| :--- | :--- | :--- | :--- |
| [owner/reviewer] | — | — | [Approved/Blocked] |
