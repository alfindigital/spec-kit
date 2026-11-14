# Runbook — {nama service/proses}

> Copy file ini per service: `docs/ops/{nama}.md`. Hapus blok config yang
> tidak relevan. Secrets hanya referensi (`$ENV_VAR` / vault) — jangan nilai.

## Apa & Kenapa

- **Service:** [nama — mis. queue worker, nightly backup]
- **Command:** [command yang dijalankan]
- **Kenapa perlu proses server:** [mis. job >30s tidak boleh di request path]
- **Dampak jika mati:** [mis. email tidak terkirim, laporan tidak jalan]

## Prerequisites

- [runtime/tools yang harus ada — versi]
- [env var yang dibutuhkan — nama saja]

## Setup

### Config — pilih blok yang relevan

**Supervisor** `/etc/supervisor/conf.d/{project}-{service}.conf`:

```ini
[program:{project}-{service}]
process_name=%(program_name)s_%(process_num)02d
command={full_command}
autostart=true
autorestart=true
user={user}
numprocs={n}
redirect_stderr=true
stdout_logfile={log_path}
stopwaitsecs=3600
```

**Systemd** `/etc/systemd/system/{project}-{service}.service`:

```ini
[Unit]
Description={desc}
After=network.target

[Service]
Type=simple
User={user}
WorkingDirectory={app_path}
ExecStart={full_command}
Restart=always

[Install]
WantedBy=multi-user.target
```

**Cron**:

```cron
{schedule} {command} >> {log_path} 2>&1
```

### Install

```bash
# supervisor
sudo cp docs/ops/supervisor/{service}.conf /etc/supervisor/conf.d/
sudo supervisorctl reread && sudo supervisorctl update
sudo supervisorctl start {project}-{service}:*

# systemd
sudo cp docs/ops/systemd/{service}.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now {project}-{service}
```

## Verifikasi

| Check | Command | Expected |
| :--- | :--- | :--- |
| Proses hidup | `[systemctl status / supervisorctl status]` | RUNNING |
| Log berputar | `tail -f {log_path}` | baris baru |
| Hasil nyata | [mis. row masuk DB / email terkirim] | [observable] |

## Troubleshooting

| Gejala | Penyebab umum | Solusi |
| :--- | :--- | :--- |
| FATAL/BACKOFF | permission, path salah | `...` |
| Restart terus | error di app, OOM | cek log, stopwaitsecs |
| Log kosong | dir tidak writable | `mkdir -p + chown` |

## Maintenance

```bash
# restart / stop / status
[command]
# update config
[command + reload step]
# remove
[stop + rm config + reload]
```
