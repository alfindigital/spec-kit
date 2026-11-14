# MODULE_MAP

> Peta modul bisnis → komponen kode. **Update saat modul baru dibuat atau
> struktur file modul berubah.** Mencegah agent menebak-nebak "file-nya di mana".
>
> Anti-monster: file ini indeks. Modul kompleks boleh split ke `modules/{nama}.md`.

## Module → Code

| Modul | Routes | Controllers/Handlers | Models | Services | Views/UI |
| :--- | :--- | :--- | :--- | :--- | :--- |
| [nama] | [path] | [path] | [path] | [path] | [path] |

<!-- EXAMPLE:
| payment | routes/api.php#L40 | PaymentController | Payment, Invoice | MidtransService | pay/checkout.tsx |
-->

## Shared Infrastructure

| Area | Path | Catatan |
| :--- | :--- | :--- |
| Middleware | [path] | — |
| Auth/guard | [path] | — |
| Helpers | [path] | — |
| Config | [path] | — |

## Modul Detail (link split)

| Modul | File detail |
| :--- | :--- |
| [nama] | [modules/nama.md — jika di-split] |
