# Studi Kasus: Vibe Coding Workflow untuk App Berbayar (Revenue-Generating)

> **Sumber:** Curasi SariClean dari diskusi Threads [@keanureksha](https://www.threads.net/@keanureksha/post/Dd-dptOATQV)  
> **Arsip Penuh & Infografis PNG:** [`Documents/AI/Knowledge/Agentic Coding/prd-workflow-vibe-coding-revenue-app/`](../../Knowledge/Agentic%20Coding/prd-workflow-vibe-coding-revenue-app/)

---

## 1. Konsensus Praktisi Lapangan

Para builder yang berhasil meluncurkan aplikasi hingga menghasilkan pengguna berbayar / revenue **tidak lagi membuat PRD formal 20 halaman** (*legacy waterfall workflow*).

Alur lama yang lambat:
$$\text{PRD 20 Hal} \longrightarrow \text{Ticket} \longrightarrow \text{Handover Engineer} \longrightarrow \text{Code} \longrightarrow \text{Rilis Berbulan-bulan}$$

Digantikan oleh alur modern:
$$\text{Goal / Problem} \longrightarrow \text{ERD \& Tech Stack Lock} \longrightarrow \text{In-Workspace Agent Tasks} \longrightarrow \text{Instant Deploy} \longrightarrow \text{Distribution Loop}$$

---

## 2. Poin Kunci untuk Blueprinting

1. **AI Butuh Cetak Biru, Bukan Prompt Buta:**
   Secanggih apa pun AI agent (Claude Code, Cursor, Codex), ERD (*Entity-Relationship Diagram*) dan skema database adalah fondasi mutlak. Tanpa ini, agent akan kehilangan konteks di tengah jalan dan terjebak looping bug.
2. **In-Workspace Spec Execution:**
   Spec dan task breakdown dikelola langsung di dalam editor (mis. Kiro AI, Cursor, Claude Code) agar instruksi selalu sinkron dengan state kode.
3. **Showcase Stack Nyata di Lapangan:**
   * `morphic-cms.com`: Hono + Neon Serverless Postgres + Drizzle ORM + dynamic JSONB schema.
   * `needmcp.com`: Agregator MCP, diluncurkan langsung dari implementation plan teknis ringkas.
   * `pamerin.id`: Dibuat menggunakan spec native di Kiro AI.
4. **Jebakan Fatal Distribusi:**
   Tantangan tersulit bukan koding (koding sudah sangat murah dengan AI, bahkan ada yang modal token \$3). Jebakan termahal adalah malas promosi dan tidak memetakan pasar sejak awal, sehingga aplikasi berakhir mangkrak sebagai private repository di GitHub.
