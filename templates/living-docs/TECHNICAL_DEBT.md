# TECHNICAL_DEBT

> Inventaris utang teknis yang diketahui. Tujuan = sadar risiko, BUKAN langsung
> diperbaiki — jangan refactor debt tanpa permintaan user.
> **Update saat debt baru disengaja/ditemukan/dilunasi.**

**Prioritas:** P0 = risiko outage/data loss · P1 = menghambat dev/perf · P2 = smell/test gap · P3 = kosmetik

## Debt List

| Pri | Debt | Evidence | Arah perbaikan |
| :--- | :--- | :--- | :--- |
| [P0] | [deskripsi] | [file/metrik] | [saran] |

<!-- EXAMPLE: P1 — N+1 di list transaksi; evidence: 40 query/request di
TransactionController@index; arah: eager load + index (user_id, created_at). -->

## Risky Areas (berisiko tinggi jika disentuh)

| Area | Risk | Alasan |
| :--- | :--- | :--- |
| [path/modul] | [HIGH/MED] | [mis. tidak ada test coverage, coupling tinggi] |

## Debt Acceptance

> Utang yang DISENGAJA dan diterima — beserta alasannya, supaya tidak "diperbaiki"
> oleh agent yang tidak tahu konteks.

- [debt] — diterima karena [alasan]
