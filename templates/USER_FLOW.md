# User Flow Specification (Satellite Doc)

> **Kapan dipakai:** Hanya bila flow multi-aktor / multi-state cukup kompleks sehingga
> user stories di PRODUCT_SPEC tidak cukup menjelaskannya. Centang di BLUEPRINT_REVIEW §7.
> **Cara Pakai:** Copy ke repo project, isi flow sesuai konteks. Hapus `<!-- EXAMPLE -->`.

---

## 1. Flow Map Overview

```text
<!-- EXAMPLE: Gambar hubungan antar-flow, bukan wireframe -->
[Landing] → [Sign Up / Login] → [Onboarding] → [Dashboard]
                                                ├→ [Flow A: Core Action]
                                                ├→ [Flow B: Management]
                                                └→ [Flow C: Settings/Billing]
```

## 2. Actor Quick Reference

| Actor ID | Nama | Titik Masuk Utama |
| :--- | :--- | :--- |
| `ACT-01` | [Aktor] | [Flow awal aktor ini] |

---

## 3. Definisi Flow

> Format baku per flow. Aktor melakukan AKSI, sistem merespons STATE — bedakan tegas
> agar agent tidak mengaburkan tanggung jawab.

### `FLOW-01`: [Nama Flow — mis. Checkout & Pembayaran]

- **Aktor utama:** [ACT-XX]
- **Preconditions:** [state yang harus benar sebelum flow mulai]
- **Trigger:** [apa yang memulai flow]

**Happy Path:**

| # | Aktor | Sistem |
| :--- | :--- | :--- |
| 1 | [aksi user] | — |
| 2 | — | [respon/validasi/state change] |
| 3 | [aksi user] | — |
| 4 | — | [kondisi sukses akhir] |

**Alternate Paths:**

| Dari Step | Kondisi | Jalur |
| :--- | :--- | :--- |
| 2 | [kondisi alternatif sah] | [alur alternatif → kembali ke step N / exit] |

**Error Paths:**

| Dari Step | Kegagalan | Perilaku Sistem Wajib |
| :--- | :--- | :--- |
| 2 | [mis. timeout gateway] | [retry sekali → tampilkan error terstruktur, jangan charge] |
| 3 | [mis. input invalid] | [pesan field-level, state tidak berubah] |

**Postconditions:** [state akhir yang dijamin — mis. order tersimpan status PENDING, stok ter-reserve]

<!-- EXAMPLE FLOW-01 Checkout:
1. User klik "Bayar" → —
2. — → validasi cart + reserve stok (5 mnt TTL)
3. User pilih QRIS → —
4. — → buat charge ke Midtrans, tampilkan QR
5. — → webhook paid → status PROCESSING, kirim receipt
Error step 4: Midtrans timeout → release stok, order CANCELLED, notif user.
-->

### `FLOW-02`: [Nama Flow]
*(ulangi format FLOW-01)*

---

## 4. Cross-Flow Rules

- **Entry gates:** [flow mana yang butuh auth/role tertentu — ref PRODUCT_SPEC 3.1]
- **Shared states:** [entity yang disentuh >1 flow — ref state machine TECH_SPEC 3.3]
- **Dead-end check:** setiap error path WAJIB punya jalan keluar (retry, cancel, atau contact support) — tidak boleh user "stuck".

## 5. Out-of-Flow (dilarang)

- [Aksi yang TIDAK boleh terjadi di flow mana pun — mis. guest checkout, refund mandiri oleh customer]
