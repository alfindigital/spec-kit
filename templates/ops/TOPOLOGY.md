# TOPOLOGY — Peta Environment

> Satu file per environment (staging / production). Masuk git repo →
> **placeholder untuk IP/host**; nilai aktual di vault/dokumen internal,
> referensikan lokasinya.

## Diagram

```text
                    internet
                        │
                   [ LB / CDN ]
                        │
              ┌─────────┴─────────┐
              │   Private network │
              │   [subnet/{cidr}] │
              │                   │
              │  [app]      [db]  │
              │  nginx      db    │
              └───────────────────┘
```

## Komponen

| Komponen | Peran | Host ref | Port expose | Catatan |
| :--- | :--- | :--- | :--- | :--- |
| [app server] | [app+nginx] | `{APP_HOST}` | [80 via LB] | — |
| [db server] | [engine] | `{DB_HOST}` | [3306 internal only] | — |
| [load balancer] | SSL termination | `{LB_HOST}` | 443 publik | — |

## Port & Arah Traffic

| Port | Service | Bind | Terbuka ke |
| :--- | :--- | :--- | :--- |
| 443 | HTTPS | LB | publik |
| [3306] | [db] | internal | app server saja |
| [6379] | [redis] | localhost | tidak keluar |

## Aturan Boundary

- [mis. "DB tidak punya IP publik — hanya private subnet"]
- [mis. "SSH hanya via bastion/VPN, PermitRootLogin no"]

## Credential & Secret Location

| Rahasia | Lokasi (referensi, bukan nilai) |
| :--- | :--- |
| DB password | `.env` di server — `DB_PASSWORD` |
| Deploy key | GitHub repo → Settings → Deploy keys (read-only) |
| [lainnya] | [vault path / secret manager ref] |
