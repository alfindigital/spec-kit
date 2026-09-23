# Technical Specification Template

> **Dokumen Terpadu:** Menggabungkan Architecture, Database Schema, API Contracts, Validation, Security, dan Testing dalam satu file — **tanpa duplikasi**.
> **Cara Pakai:** Copy file ini ke repo project baru, lalu isi sesuai konteks project. Hapus komentar `<!-- EXAMPLE -->` setelah diganti.

---

## Part 1: Architecture & Tech Stack

### 1.1 Tech Stack Definition

| Layer | Teknologi | Justifikasi |
| :--- | :--- | :--- |
| **Frontend** | [misal: React 19 / Next.js / Vite] | [alasan pemilihan] |
| **Styling & UI** | [misal: Tailwind CSS + Shadcn UI] | [referensi DESIGN_SPEC.md] |
| **Backend / API** | [misal: Node.js / Bun + Elysia / Hono] | [alasan pemilihan] |
| **Database** | [misal: PostgreSQL / SQLite / Turso] | [alasan pemilihan] |
| **ORM / Query** | [misal: Drizzle ORM / Prisma] | [alasan pemilihan] |
| **Cache & Queue** | [misal: Redis / Upstash / BullMQ] | [alasan pemilihan] |
| **Secrets Engine** | Mozilla SOPS + Age | Kredensial terenkripsi di git |

<!-- EXAMPLE:
| Frontend | React 19 / Next.js | Server Components, SEO optimal |
| Backend | Bun + Elysia | Type-safe end-to-end, async I/O cepat |
| Database | PostgreSQL | ACID, UUIDv7, JSONB, partial indexing |
| ORM | Drizzle ORM | Zero-overhead, type-safety ketat |
-->

### 1.2 Layered Architecture & SoC

Setiap modul wajib mematuhi pemisahan 3 lapisan tanpa kontaminasi silang:

```text
┌─────────────────────────────────────────────────────────────┐
│ 1. PRESENTATION / UI LAYER (/app, /components)              │
│    - Murni render tampilan, hook UI-state binding            │
│    - DILARANG: Query database langsung, raw network fetch    │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. APPLICATION / BUSINESS LOGIC LAYER (/services, /usecases)│
│    - Validasi aturan bisnis, kalkulasi, otorisasi            │
│    - Terisolasi dari dependensi framework antarmuka          │
└─────────────────────────────┬───────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. INFRASTRUCTURE / DATA LAYER (/db, /repositories)          │
│    - Query database, klien API 3rd-party, cache              │
│    - Repository pattern mengisolasi interaksi eksternal      │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 Struktur Folder & Konvensi Kode

```text
src/
├── app/                  # Router & Halaman UI (Presentation)
├── components/           # Komponen UI Reusable
│   ├── ui/               # Atomik (Button, Modal, Input)
│   └── shared/           # Gabungan domain-spesifik
├── modules/              # Feature-based modular structure
│   └── [feature]/
│       ├── [feature].service.ts    # Business logic
│       ├── [feature].schema.ts     # Validasi Zod
│       └── [feature].repository.ts # Data access
├── lib/                  # Utilities singleton (db, redis, logger)
└── types/                # Strict TypeScript definitions
```

### 1.4 Third-Party Integrations

| Service | Fungsi | Koneksi |
| :--- | :--- | :--- |
| [Auth Provider] | [OAuth / Magic Link] | [SDK / REST API] |
| [Payment Gateway] | [Pembayaran] | [Webhook / REST API] |
| [Object Storage] | [Upload file] | [S3 Presigned URL] |

---

## Part 2: Database Schema & Data Dictionary

> **SINGLE SOURCE OF TRUTH** — DDL hanya di section ini, tidak di file lain.

### 2.1 Konvensi Database

- Nama tabel: plural snake_case (`users`, `orders`, `transactions`)
- Primary Key: `UUIDv7` (time-sortable, b-tree friendly)
- **Audit Columns wajib di setiap tabel:**
  - `id`: UUID Primary Key
  - `created_at`: TIMESTAMPTZ DEFAULT NOW()
  - `updated_at`: TIMESTAMPTZ DEFAULT NOW()
  - `deleted_at`: TIMESTAMPTZ NULL (soft delete — dilarang hard delete data finansial)
- Indeks eksplisit untuk semua foreign key dan kolom yang sering difilter
- Migration file berurutan: `001_init.sql`, `002_add_orders.sql`, dst.

### 2.2 Entity Relationship Diagram (ERD)

```text
[Users] (1) ──< (N) [Orders] (1) ──< (N) [OrderItems] >── (1) [Products]
```

### 2.3 DDL / Skema Relasional

```sql
<!-- EXAMPLE: Ganti dengan skema database project Anda -->

-- Tabel Autentikasi Pengguna
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) NOT NULL UNIQUE,
    full_name VARCHAR(100) NOT NULL,
    role VARCHAR(20) NOT NULL DEFAULT 'customer',
    is_verified BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    deleted_at TIMESTAMPTZ NULL
);

-- Tabel Transaksi Pesanan
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE RESTRICT,
    total_amount NUMERIC(15, 2) NOT NULL CHECK (total_amount >= 0),
    status VARCHAR(30) NOT NULL DEFAULT 'pending',
    idempotency_key UUID UNIQUE NOT NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Indeks Performa
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
```

---

## Part 3: API Contracts & Endpoint Specifications

### 3.1 Standard Response Envelope

```typescript
// Respon Sukses
interface ApiResponse<T> {
  success: true;
  data: T;
  meta?: { timestamp: string; requestId: string };
}

// Respon Kesalahan
interface ApiErrorResponse {
  success: false;
  error: {
    code: string;
    message: string;
    details?: Array<{ field: string; message: string }>;
  };
}
```

### 3.2 Endpoint Contracts

<!-- EXAMPLE: Ganti dengan endpoint project Anda -->

#### `POST /api/v1/orders` — Membuat pesanan baru
* **Auth:** Bearer Token / HTTP-Only Cookie
* **Headers:** `Idempotency-Key: <uuid-v4>`
* **Error States:**

| HTTP Status | Error Code | Skenario |
| :--- | :--- | :--- |
| `400` | `VALIDATION_ERROR` | Payload tidak valid (ditolak Zod) |
| `401` | `INVALID_SESSION` | Token kadaluarsa / tidak ada |
| `409` | `DUPLICATE_IDEMPOTENCY` | Idempotency key sudah diproses |
| `422` | `INSUFFICIENT_STOCK` | Stok tidak mencukupi |
| `500` | `SERVER_FAILURE` | Kesalahan server/database |

### 3.3 State Machine & Status Transitions

Definisikan siklus hidup entitas agar AI tidak mengubah status secara ilegal:

```text
<!-- EXAMPLE: Ganti dengan state machine project Anda -->
[DRAFT] ──(submit)──> [PENDING_PAYMENT] ──(webhook: paid)──> [PROCESSING]
                             │                                     │
                      (timeout: 24h)                          (ship items)
                             │                                     │
                             ▼                                     ▼
                        [CANCELLED]                           [COMPLETED]
```

---

## Part 4: Validation Schemas (Zod / Pydantic)

> **SINGLE SOURCE** — Schema validasi hanya di section ini.

```typescript
<!-- EXAMPLE: Ganti dengan schema project Anda -->
import { z } from "zod";

export const CreateOrderItemSchema = z.object({
  productId: z.string().uuid("Invalid product ID format"),
  quantity: z.number().int().positive().max(50, "Max 50 items per order"),
  unitPrice: z.number().positive(),
});

export const CreateOrderSchema = z.object({
  items: z.array(CreateOrderItemSchema).min(1, "Minimal 1 item pesanan"),
  shippingAddressId: z.string().uuid("Invalid shipping address ID"),
  paymentMethod: z.enum(["qris", "bank_transfer", "e_wallet"]),
  customerNotes: z.string().max(250).optional(),
});
```

---

## Part 5: Security & Error Handling

### 5.1 Input & Data Security
1. **SQL Injection & XSS Guard:** Semua query via parameterized ORM. Dilarang string interpolation SQL.
2. **Defensive Boundary:** Validasi payload di middleware/controller dengan Zod sebelum ke service.
3. **Input Sanitization:** Semua teks melewati pembersihan HTML/XSS.

### 5.2 Transaction & Idempotency
1. **Idempotency Locking:** Operasi pembayaran/mutasi stok wajib header `Idempotency-Key` + transaksi DB (`SELECT ... FOR UPDATE`).
2. **Rate Limiting:** 60 req/menit endpoint publik, 10 req/menit auth/payment.

### 5.3 Secret Management
Dilarang hardcode kredensial. Semua secret via Mozilla SOPS (`secret.yaml`). Detail: lihat [`guides/SECURITY_AND_SECRETS.md`](../guides/SECURITY_AND_SECRETS.md).

### 5.4 Calculation Formulas & Business Edge Cases

<!-- EXAMPLE: Ganti dengan formula bisnis project Anda -->
$$\text{Total} = (\text{Subtotal Items}) - \text{Diskon Promo} + \text{Ongkos Kirim} + \text{PPN (11\%)}$$

**Guardrails:**
- Diskon > subtotal → total = 0 (tidak boleh negatif)
- Pembulatan: Bankers Rounding (round half to even) pada 2 desimal

---

## Part 6: Testing Strategy

1. **Unit Test:** 100% pure function coverage — kalkulasi harga, diskon, pajak, validasi Zod.
2. **Integration Test:** End-to-end database (create → read → update → soft delete).
3. **Regression Guard:** CI/CD memblokir merge jika test gagal (`bun test` / `npm test`).

| Lapisan | Alat | Target |
| :--- | :--- | :--- |
| Unit | Vitest / Bun Test | ≥80% coverage |
| Integration | Supertest + Test Container | Alur kritis |
| E2E | Playwright | User journey utama |
