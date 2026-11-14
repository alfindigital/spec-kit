# MENTAL_MODEL

> Model mental arsitektur — "cara sistem ini berpikir". **Update saat pola
> arsitektur berubah.** Bukan dokumentasi detail — cukup untuk membuat keputusan
> yang konsisten dengan desain asli.

## Prinsip Desain Inti

1. [mis. "Semua state mutation lewat service layer — controller tipis"]
2. [mis. "Idempotency-first: semua endpoint POST terima Idempotency-Key"]

## Alur Utama

```text
[request] → [middleware] → [service] → [repository] → [DB]
                │               │
                ▼               ▼
            [event/queue]   [domain rule]
```

## Invariant (yang TIDAK BOLEH dilanggar)

- [mis. "Saldo user hanya berubah via LedgerService — tidak ada update langsung"]
- [mis. "File upload tidak pernah disimpan di public/"]

## Trade-off yang Disengaja

| Keputusan | Konsekuensi diterima | Alasan |
| :--- | :--- | :--- |
| [pilihan desain] | [cost-nya] | [kenapa tetap dipilih] |
