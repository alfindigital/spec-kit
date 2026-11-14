# Projects & Drafts Workspace

Folder ini digunakan sebagai **ruang inkubasi & perancangan spesifikasi (drafting room)** untuk ide aplikasi atau fitur baru.

### Alur Kerja:
1. Saat Anda memiliki ide produk/app baru, buat folder draf di sini:
   `Documents/AI/blueprints/projects/<nama-ide>/`
2. Salin template yang relevan dari `../templates/`:
   * Untuk app cepat/micro-SaaS: Salin `LEAN_PRD.md` dan `IMPLEMENTATION_PLAN.md`.
   * Untuk sistem kompleks: Salin `PRODUCT_SPEC.md` dan `TECH_SPEC.md`.
3. Diskusikan dan matangkan spesifikasi bersama AI (gunakan `/grill-me` untuk menguji edge cases dan logika bisnis).
4. Setelah spec berstatus **APPROVED**:
   * Pindahkan spec final ke folder repository kode sebenarnya (misal di `Documents/Apps/<nama-proyek>/specs/` atau `Documents/IDX/<nama-proyek>/specs/`).
   * Mulai eksekusi koding bersama AI agent.
