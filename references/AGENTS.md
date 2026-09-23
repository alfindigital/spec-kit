# UNIVERSAL AI CODING AGENT DIRECTIVES

## 1. Core Engineering Standards
- **Clean & Idiomatic Code:** Write production-grade, readable, and maintainable code adhering strictly to the idiomatic standards of the target language/framework.
- **Single Responsibility Principle (SRP):** Each module, class, and function must have only one reason to change. Functions must not exceed 30–40 lines.
- **Early Returns & Guard Clauses:** Avoid deep nesting (max 2 levels). Handle edge cases, invalid inputs, and error states at the top of the function to eliminate nested `if-else` trees.
- **Explicit Typing:** Enforce strict type definitions across all boundaries. Avoid `any`, `interface{}` (without type assertions), or untyped dictionaries/maps unless strictly required.
- **No Hardcoded Values:** Extract constants, status codes, magic strings, and configuration values into dedicated constant files or enums.

## 2. Architecture & Separation of Concerns (SoC)
- **Layered Architecture:** Maintain a clean boundary between:
  1. *Presentation / UI Layer:* Pure view logic, rendering, and UI-state binding.
  2. *Application / Business Logic Layer:* Services, use-cases, and business validations.
  3. *Infrastructure / Data Layer:* Database queries, API clients, caching, and third-party integrations.
- **No Cross-Contamination:** Never execute raw database queries, network calls, or heavy business logic directly inside UI components or presentation controllers.
- **Component & Module Decomposition:** Split large files once they approach 200–250 lines into smaller, focused, and reusable components/modules.

## 3. Error Handling & Resilience
- **Explicit Error Management:** Never swallow errors silently or use empty `catch`/`except` blocks.
- **Actionable Error Messages:** Provide clear, structured error logs for developers and sanitized, user-friendly messages for the client.
- **Defensive Boundary Checks:** Validate all external inputs (API payloads, query parameters, file uploads) using standard schema validators before passing data to domain logic.

## 4. Code Generation & Refactoring Guardrails
- **Scope Discipline:** Only modify or generate code directly related to the user's prompt. Do not perform unsolicited broad refactoring on untouched files.
- **Minimal Diffs:** Keep edits surgical and clean. Preserve existing formatting, linting rules, and architectural patterns of the current codebase.
- **Self-Documenting Code:** Choose clear, intention-revealing variable and function names. Write comments only to explain complex business decisions (*why*), not trivial execution steps (*how*).
- **Security Baseline:** Always sanitize inputs against SQL injection, XSS, and command injection. Never hardcode secrets, API keys, or credentials.

## 5. Agent Context, Secrets, & Git Hygiene
- **Encrypted Secrets Workflow:** Simpan konfigurasi kredensial proyek dalam `secret.yaml` menggunakan Mozilla SOPS + enkripsi `age`. Biarkan key tetap terbuka sebagai deklarasi konfigurasi, namun enkripsi nilainya. Integrasikan ke CI/CD dengan automatic secret masking (`::add-mask::`). Reference: [`guides/SECURITY_AND_SECRETS.md`](guides/SECURITY_AND_SECRETS.md).
- **Allowlist .gitignore Pattern:** Untuk repositori baru atau yang dikelola AI agent, utamakan pola reverse `.gitignore` (allowlist: default `*`, unignore eksplisit `!path`) guna mencegah token bleed saat `git status` dan mengunci file rahasia/untracked secara otomatis. Reference: [`guides/GIT_HYGIENE.md`](guides/GIT_HYGIENE.md).
- **Session Handover:** Gunakan `SESSION.md` sebagai memori kerja eksternal sebelum menjalankan `/clear` atau berpindah sesi konteks AI. Buat SESSION.md di proyek terkait; SESSION.md pada knowledge base ini adalah handover pemeliharaan, bukan template kosong.

## 6. Specification & Verification Hierarchy (Vibe Coding Stack)
- **Spec First:** Do not execute code without explicit specifications. Follow the hierarchy:
  $$\text{BRD (Business/Why)} \longrightarrow \text{PRD (Product/What)} \longrightarrow \text{FRD/Tech Spec (How/Data Contract)} \longrightarrow \text{Implementation}$$
- **Atomic Execution:** Break work into small, verifiable steps. 1 task = 1 prompt / 1 commit.
- **Test-Driven Acceptance:** Write failing unit/integration tests before implementing complex logic. A feature is done only when automated tests PASS.
- **Fresh Context Review:** For major diffs, review changes via a clean session with `git diff` before final merge.

## 7. UI/UX & Design System Standards (`DESIGN_SPEC.md`)
- **Visual Source of Truth:** Never hardcode ad-hoc CSS styles or arbitrary magic colors. All UI code must strictly adhere to `DESIGN_SPEC.md` (color palette tokens, typographic scale, spacing, border radius, and elevation).
- **Component State Completeness:** Every interactive component must handle all essential states (default, hover, focus-visible, active, disabled, loading skeletons, and empty/error states).
- **Worktree Isolation for UI Refactors:** Major UI redesigns or design system extractions must begin in a dedicated `git worktree` and be verified end-to-end (keyboard navigation `Tab`/`Enter`/`Esc`, contrast ratios, and responsive mobile/tablet/desktop viewports).
- **Reference:** [`guides/UI_UX_DESIGN_SYSTEM.md`](guides/UI_UX_DESIGN_SYSTEM.md).

## 8. Anti-Overengineering & Action Boundaries (Sol-Class Trap Guardrails)
- **The Core Rule:** *"Sol-class models don't write wrong code. They write too much code."* Do not add frameworks when asked for a config change. Do not add adapter layers when fixing a localized bug. Green CI and passing tests do not justify unnecessary architectural layers.
- **Recognize & Eliminate Failure Modes:**
  1. *Surface Patching:* Do not pile on patches, compatibility layers, or dual implementations when a clean 1–2 line root-cause fix suffices.
  2. *Over-Designing for Rare Cases:* Never build premature abstractions for hypothetical future cases that make everyday maintenance expensive.
  3. *Wrong Premise Trap:* Validate the starting assumption first. No amount of correct reasoning fixes an incorrect premise. Read the target code directly instead of guessing or performing excessive blind searches.
  4. *Test-Covered Scope Creep:* Never use "adding tests" as an excuse to introduce new abstractions, expand scope, or backfill unrelated modules.
- **Action Boundaries (Stop Signals):**
  - Before writing code, explicitly define: (1) what the user actually wants, (2) the exact task scope, (3) what is explicitly out of scope, and (4) the definition of done.
  - STOP immediately if you catch yourself:
    - Adding abstractions, config layers, or wrappers not explicitly requested.
    - Modifying unrequested or unrelated files.
    - Stacking artificial constraints to satisfy prior constraints.
    - Creating a parallel implementation to preserve legacy code that should simply be edited in-place.
- **Testing Boundaries:**
  - Tests exist strictly to verify the current change's acceptance criteria—nothing else.
  - Prefer existing test suites. If existing tests pass and prove the fix, do NOT write new tests.
  - New tests are permitted ONLY if: (1) behavior changed in a way existing tests cannot cover, OR (2) user explicitly requested tests.
  - Scope limit: Maximum 1 main path test + 1 critical failure path test per atomic task. No snapshot matrices, no parameterized grids, no educational test suites.
  - Answer before writing any test: *"Which accepted requirement does this test verify?"*
- **Reference:** [`guides/ANTI_OVERENGINEERING.md`](guides/ANTI_OVERENGINEERING.md).

## 9. Agent Orchestration & Subagent Delegation
- **Orchestrator Pattern (Single Source of Truth):** In multi-agent tasks, the **Main Agent** is the sole authoritative entity responsible for reconciling worker findings, writing final artifacts, editing project files, creating git commits, and pushing code.
- **Strictly Read-Only Worker Subagents:** Subagents spawned for code exploration, dependency tracing, or massive data/log audits must operate in **Strictly Read-Only** mode. They must never mutate files, modify configuration, or pollute the repository working tree.
- **Hybrid Cost & Token Strategy:**
  - *Web / Chat Interface (High-Level Planning):* Utilize chat web subscriptions (e.g. ChatGPT Plus / Claude Web) for high-level specification writing, architectural planning, and GitHub PR reviews to conserve API token quotas.
  - *Local CLI Agent (Execution & High Reasoning):* Utilize local agent CLI tools (Codex / Antigravity / Claude Code) with high reasoning effort strictly for localized file edits, executing commands, running tests, and git operations.
- **Reference:** [`guides/AGENT_ARCHITECTURE_AND_WORKFLOW.md`](guides/AGENT_ARCHITECTURE_AND_WORKFLOW.md).
