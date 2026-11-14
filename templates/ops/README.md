# Ops Docs — Runbook & Operasional Server

> **Kapan dipakai:** Project punya komponen yang harus di-setup/dijaga di server —
> queue worker, scheduler/cron, backup, monitoring, deploy process.
> **Cara Pakai:** Copy folder ini sebagai `docs/ops/` di repo project. Isi satu
> `RUNBOOK` per service/proses, `TOPOLOGY` sekali per environment,
> `PRODUCTION_READINESS` sebagai gate sebelum go-live.

---

## Wajib Dibuat Saat

Agent WAJIB membuat runbook saat membuat fitur yang butuh proses server:

- Queue worker / background job (Redis, RabbitMQ, SQS)
- Scheduler / cronjob
- Process manager (supervisor/systemd/pm2)
- Backup / restore script
- Health check endpoint, monitoring, alerting
- Webhook/callback third-party yang perlu tunnel/firewall rule
- File storage sync, log rotation, SSL renewal

## Format Wajib Runbook

Setiap runbook WAJIB menjawab, urut:

1. **Apa** — nama service & peran
2. **Kenapa** — kenapa butuh proses server (bukan in-request)
3. **Prerequisites** — apa yang harus ada dulu
4. **Setup** — step-by-step, copy-paste-able
5. **Verifikasi** — cara bukti dia hidup (command + expected output)
6. **Troubleshooting** — gejala → penyebab → solusi
7. **Maintenance** — update / restart / stop / remove

## Mode Setup

| Mode | Arti | Default untuk |
| :--- | :--- | :--- |
| Manual | Agent tulis docs, user jalankan | ✅ production |
| Auto | Agent jalankan setup via SSH | dev/staging, dan hanya jika user setuju |

## Security Boundary (MUTLAK)

File ops masuk git repo. **DILARANG credential/IP aktual/secret.** Gunakan
referensi `.env` (`$DB_PASSWORD`) atau nama vault key. Config sample pakai
placeholder `{project}`, `{app_path}` — jangan nilai produksi.

## File

| File | Isi |
| :--- | :--- |
| `RUNBOOK.md` | Template canonical per service (supervisor/systemd/cron/backup/monitoring — pilih blok config yang relevan) |
| `TOPOLOGY.md` | Peta environment: server, network, port, arah traffic |
| `PRODUCTION_READINESS.md` | Checklist gate sebelum go-live |
