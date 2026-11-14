# Delivery Roadmap (Satellite Doc)

> **Kapan dipakai:** Bila delivery bertahap (>1 fase/milestone). Untuk project 1-shot
> kecil, PLAN.md saja cukup — jangan buat file ini.
> **Relasi:** ROADMAP = fase high-level (WHAT kapan) → PLAN.md = backlog atomik
> (HOW, 1 task = 1 commit) yang di-breakdown per fase.
> **Cara Pakai:** Copy ke repo project, isi fase sesuai konteks.

---

## 1. Delivery Strategy

- **Pendekatan:** [Vertical slice / Horizontal layer / Hybrid]
- **Prinsip:** [mis. setiap milestone harus deployable & demo-able]
- **Target rilis pertama:** [scope minimum yang memberi nilai]

<!-- EXAMPLE: Vertical slice — tiap milestone memotong UI→API→DB untuk 1 fitur utuh,
bukan "bulan 1 semua backend, bulan 2 semua frontend". -->

## 2. Milestones

### `M0` — Fondasi & Skeleton
- **Goal:** [mis. repo jalan, auth works, deploy pipeline live]
- **Scope backlog:** [BL-001, BL-002, ... — ref PRODUCT_SPEC 3.2]
- **Depends on:** — (start)
- **Exit criteria:** [kondisi verifiable — mis. user bisa login & logout di staging]
- **Risiko utama:** [ref PRODUCT_SPEC 1.6]

### `M1` — [Nama Milestone]
- **Goal:** —
- **Scope backlog:** —
- **Depends on:** M0
- **Exit criteria:** —
- **Risiko utama:** —

### `M2` — [Nama Milestone]
- **Goal:** —
- **Scope backlog:** —
- **Depends on:** M1
- **Exit criteria:** —
- **Risiko utama:** —

## 3. Milestone Map

```text
M0 (Fondasi) ──> M1 (Core loop) ──> M2 (Monetisasi) ──> M3 (Hardening)
   │                  │                  │                    │
 auth, skeleton    fitur utama 1     payment, admin      scale, audit, SLA
```

## 4. Explicitly Deferred

> Fitur yang SENGAJA ditunda dan alasannya — mencegah "sekalian aja" scope creep.

| Item | Defer ke | Alasan |
| :--- | :--- | :--- |
| [Fitur/fase] | [M-x / post-launch] | [ROI / blocker / risiko] |

## 5. Rollback & Kill Criteria

- **Rollback per milestone:** [cara mundur bila M-x gagal — feature flag, revert, restore]
- **Kill criteria:** [kondisi yang membuat project/milestone harus dihentikan & direview ulang]

---

> **Handoff:** Setelah roadmap disetujui, pecah milestone aktif menjadi `PLAN.md`
> (backlog atomik, 1 task = 1 commit, TDD per task) — lihat SKILL.md Step 5.
