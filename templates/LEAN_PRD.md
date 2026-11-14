# Lean PRD & Implementation Blueprint (1-Page)

> **Untuk:** Vibe coding berorientasi rilis & revenue cepat (MVP / Micro-SaaS / Tools).
> **Prinsip:** Gantikan dokumen formal 20 halaman dengan kontrak teknis, arsitektur database, dan batas masalah yang tajam.
> **Alur:** $\text{Goal} \longrightarrow \text{ERD \& Tech Stack Lock} \longrightarrow \text{In-Workspace Tasks} \longrightarrow \text{Deploy} \longrightarrow \text{Distribution}$

---

## 1. The Core Bet (Masalah & Target)

* **Problem Statement:** Masalah nyata apa yang paling menyakitkan yang ingin diselesaikan? (Maksimal 2 kalimat, tanpa jargon).
* **Target Audience:** Siapa pengguna spesifik pertama yang rela menggunakan/membayar solusi ini?
* **Value Proposition:** Mengapa solusi ini 10x lebih cepat / murah / simpel dibanding cara lama?
* **TAM/SAM Reality Check:** Di mana calon pengguna berkumpul saat ini (grup Telegram, forum, niche komunitas, keyword search)?

---

## 2. Boundaries & Anti-Scope (Pagar Pembatas)

* **Must-Have (MVP Inti):**
  * [ ] Fitur 1: ...
  * [ ] Fitur 2: ...
  * [ ] Fitur 3: ...
* **Anti-Scope (DILARANG dibuat di v1):**
  * ❌ *Contoh: Auth custom rumit (pakai magic link / Clerk / anonymous session dulu)*
  * ❌ *Contoh: Sistem billing multi-tier (pakai single payment / manual transfer dulu)*
  * ❌ *Contoh: Dark mode toggle, complex dashboard analytics, social sharing*
* **Hard Constraints:** (Misal: Budget token AI < \$5, load time < 1s, zero-server / serverless only).

---

## 3. Architecture & Data Gate (Fondasi Mutlak)

> ⚠️ *JANGAN PERNAH menyuruh AI coding sebelum bagian ini terkunci (Locked).*

### 3.1 Tech Stack Lock
* **Frontend:** (Misal: Next.js / Vite React / Vanilla HTML+Tailwind)
* **Backend / Runtime:** (Misal: Cloudflare Workers / Hono / Node.js)
* **Database:** (Misal: Neon Serverless Postgres / Supabase / Cloudflare D1)
* **ORM / Query:** (Misal: Drizzle ORM / Prisma / Raw SQL)
* **Deploy Target:** (Misal: Cloudflare Pages / Vercel / VPS1)

### 3.2 Schema / ERD & Data Contract
Tuliskan entitas tabel utama dan relasinya:

```sql
-- DDL Ringkas / Skema Database Inti
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    email TEXT UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE resources (
    id TEXT PRIMARY KEY,
    user_id TEXT REFERENCES users(id),
    title TEXT NOT NULL,
    payload JSONB,
    status TEXT DEFAULT 'active'
);
```

### 3.3 Core API / Function Contract
* `POST /api/action` $\rightarrow$ Input: `{ ... }` $\rightarrow$ Output: `{ success: true, data: { ... } }`
* `GET /api/status` $\rightarrow$ Output: `{ status: "ok" }`

---

## 4. Definition of Done (DoD) & Verification

MVP dianggap **SELESAI & SIAP RILIS** jika dan hanya jika:
1. [ ] Alur utama (*happy path*) berjalan end-to-end tanpa error: Input $\rightarrow$ Process $\rightarrow$ Output.
2. [ ] Zero secret leaked: Tidak ada token/key yang hardcoded di source code.
3. [ ] Mobile responsive: Tampilan tidak berantakan di layar smartphone (360–412px).
4. [ ] Build & deploy exit 0: Aplikasi bisa diakses via URL publik.

---

## 5. In-Workspace Agent Task Breakdown (Atomik)

> *Bagi pekerjaan menjadi langkah kecil. 1 langkah = 1 eksekusi agent + 1 verifikasi.*

- [ ] **Task 1: Project Scaffolding & Stack Setup**
  * Jalankan init repo, pasang dependencies minimal, pastikan build exit 0.
  * *Verifikasi:* `npm run build` atau tes dev server berjalan.
- [ ] **Task 2: Database Migration & Schema Creation**
  * Terapkan DDL skema di atas ke database lokal/dev.
  * *Verifikasi:* Query uji insert/select berhasil.
- [ ] **Task 3: Core Business Logic & API Endpoint**
  * Buat endpoint pengolah data sesuai contract di Bagian 3.3.
  * *Verifikasi:* Test curl/fetch mengembalikan respons yang diharapkan.
- [ ] **Task 4: Minimal UI / Interface**
  * Buat antarmuka fungsional sederhana untuk memicu API di Task 3.
  * *Verifikasi:* Coba input form langsung di browser.
- [ ] **Task 5: Production Deployment & Verification**
  * Deploy ke target server/hosting publik.
  * *Verifikasi:* Buka URL live dari incognito window.

---

## 6. Distribution & Launch Gate (Anti-Mangkrak)

> 🚨 **Pencegah Jebakan Klasik:** *"Aplikasi selesai dikoding berbulan-bulan, tapi tidak ada yang tahu dan berakhir jadi private repo di GitHub."*

* **Kanal Distribusi Hari-1:** Di mana postingan pertama akan dipublikasikan? (Misal: Thread X, Threads, Grup WA/Telegram, Show HN).
* **Hook / Angle Promosi:** *"Saya bikin tool X untuk menyelesaikan masalah Y dalam Z detik."*
* **Feedback Mechanism:** Di mana user bisa lapor bug / minta fitur? (Link Telegram / form feedback sederhana).
* **Target Hari Pertama:** Minimal mendapatkan 5–10 pengguna nyata mencoba aplikasi.
