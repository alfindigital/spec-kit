# HANDOVER — Final System Handover

> Ringkasan kondisi akhir untuk engineer/agent berikutnya. **Update setelah
> milestone selesai atau sebelum ganti tangan.** Ini file pertama yang dibaca
> sesi baru — tulis untuk pembaca yang belum pernah lihat repo ini.
>
> **SECURITY:** DILARANG mengandung credential/IP/secret — hanya referensi
> ke `.env`/vault.

## Identitas

| Versi | Branch | Last commit | Tanggal | Agent terakhir |
| :--- | :--- | :--- | :--- | :--- |
| [ref VERSION.md] | [branch] | [hash] | [YYYY-MM-DD] | [model] |

## Arsitektur Saat Ini (2–4 paragraf)

[Pola arsitektur, komponen utama, alur data penting — cukup untuk orientasi.
Detail per modul → MODULE_MAP.md.]

## Kondisi Per-Area

| Area | State | Catatan |
| :--- | :--- | :--- |
| [auth] | [Production/Beta/WIP] | — |
| [payment] | — | — |

## Yang Sedang Berjalan / Belum Selesai

- [item + status + di mana dilanjutkan — ref TASKS.md]

## Jebakan yang Sudah Diketahui

> Hal yang akan bikin agent baru jatuh ke lubang yang sama — gotchas env,
> urutan deploy yang tidak boleh dibalik, "jangan X sebelum Y".

- [jebakan]

## Cara Menjalankan (ringkas)

```bash
# dev
[command]
# test / build / deploy
[command]
```

## Pointer Lanjutan

| Butuh | Baca |
| :--- | :--- |
| State sekarang | `CURRENT_STATE.md` |
| Issue open | `KNOWN_ISSUES.md` |
| Peta kode | `MODULE_MAP.md` |
| Konteks produk | `../../specs/PRODUCT_SPEC.md` |
