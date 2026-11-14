# Living Docs — Dokumentasi Hidup Per-Project

> **Kapan dipakai:** Project yang akan dikerjakan lintas sesi/agent — bukan one-shot.
> Berbeda dengan specs/ (kontrak pre-kode, statis setelah approved): living docs
> adalah **state tracker yang di-update terus selama development**.
> **Cara Pakai:** Copy folder ini sebagai `docs/living/` di repo project. AI update
> file yang terdampak di setiap akhir task — bukan buat ulang dari nol.

---

## Kenapa

Spec menjawab "apa yang akan dibangun". Living docs menjawab "kondisi sekarang apa" —
supaya sesi/agent berikutnya tidak re-derive dari nol: modul mana sudah production,
issue apa yang masih open, debt apa yang disengaja, versi berapa sekarang.

## File & Trigger Update

| File | Kapan WAJIB di-update |
| :--- | :--- |
| `CURRENT_STATE.md` | Setiap akhir task — snapshot branch/commit/modul/test |
| `TASKS.md` | Task baru mulai / selesai / blocked |
| `MODULE_MAP.md` | Modul baru dibuat, atau struktur file modul berubah |
| `KNOWN_ISSUES.md` | Issue baru ditemukan / terselesaikan |
| `TECHNICAL_DEBT.md` | Debt baru disengaja / ditemukan / dilunasi |
| `COMMIT_LOG.md` | Setiap commit+push (indeks; detail per hari) |
| `VERSION.md` | `[Unreleased]` terisi signifikan; sebelum rilis |
| `HANDOVER.md` | Setelah milestone / sebelum ganti agent/engineer |
| `MENTAL_MODEL.md` | Pola arsitektur berubah |

**Jika tidak ada yang perlu di-update:** sebutkan alasannya eksplisit di output akhir
task ("living docs tidak terdampak karena ...") — jangan diam.

## Anti-Monster Rule

File dokumentasi DILARANG membengkak — agent hanya boleh membaca yang relevan.

| File | Aturan split |
| :--- | :--- |
| `TASKS.md` | Hanya task aktif. Done >1 minggu → `TASKS-ARCHIVE.md` |
| `KNOWN_ISSUES.md` | Hanya issue OPEN. Resolved >2 minggu → `RESOLVED.md` |
| `COMMIT_LOG.md` | Hanya indeks. Detail per hari → `commit-log/YYYY-MM-DD.md` |
| `MODULE_MAP.md` | README-indeks; detail per modul boleh split `modules/{nama}.md` |

## Security Boundary (MUTLAK)

Living docs masuk git repo dan akan ter-push. **DILARANG menulis nilai secret
apapun** — password, token, API key, SSH key, connection string, credential.
Gunakan referensi: `See .env: DB_PASSWORD`, `Stored in vault`, `{value_from_env}`.

Yang BOLEH: nama service, port, struktur, keputusan, status.

> Catatan: aturan ini berlaku untuk SEMUA markdown di repo, termasuk docs ops dan
> handover. Sumber terinspirasi docs-ai yang membolehkan credential di folder docs —
> di sini tidak: kalau butuh referensi rahasia, catat *lokasi* vault-nya, bukan nilainya.

## Urutan Baca untuk Agent Baru

1. `HANDOVER.md` — gambaran kondisi akhir (start here)
2. `CURRENT_STATE.md` — snapshot sekarang
3. `TASKS.md` + `KNOWN_ISSUES.md` — apa yang sedang dikerjakan/diketahui rusak
4. `MODULE_MAP.md` / `TECHNICAL_DEBT.md` — sebelum menyentuh area tertentu
