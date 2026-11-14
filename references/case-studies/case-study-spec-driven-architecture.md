# Studi Kasus: Spec-Driven Development (SDD) & PRD-First Architecture

> **Sumber:** Curasi SariClean dari diskusi X seputar OpenSpec, GitHub Spec-Kit, dan BMAD Method  
> **Arsip Penuh & Infografis PNG:** [`Documents/AI/Knowledge/Agentic Coding/spec-driven-development-prd/`](../../Knowledge/Agentic%20Coding/spec-driven-development-prd/)

---

## 1. Mengapa Spec Wajib Sebelum Koding?

1. **Efisiensi Token & Anti-Halusinasi:**
   Menulis batasan requirement secara presisi menghemat konsumsi token LLM secara eksponensial karena AI tidak dipaksa menebak-nebak konteks.
2. **Mencegah "Halfway Rewrite":**
   Penyakit kronis "ngevibe langsung prompt tanpa spec" adalah *context drift*—di tengah jalan developer baru sadar fiturnya salah arah, berujung rombak total dari nol.

---

## 2. Tooling & Framework SDD Populer

* **OpenSpec ([openspec.dev](https://openspec.dev)):** PRD modular berbasis hierarki ketat (*changes, task, subtask*) dipadu bahasa teknis terstruktur (ASD-STE100).
* **GitHub Spec-Kit ([github.com/github/spec-kit](https://github.com/github/spec-kit)):** Toolkit resmi spec-driven development untuk memfasilitasi integrasi antara PRD dan implementasi code.
* **BMAD Method:** Metode Agile AI-driven yang mengadopsi skill *bmad plan* saat eksekusi koding berjenjang.
* **Wawancara Kritis (`/grill-me`):** Protokol interogasi AI terhadap developer untuk mencecar edge cases, asumsi tersembunyi, dan blast radius sebelum spec disetujui.
