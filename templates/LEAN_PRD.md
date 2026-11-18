# Lean PRD

## Tujuan

- Pengguna dan masalah: [siapa, kendala sekarang]
- Hasil yang bisa diamati: [apa yang berubah setelah pekerjaan selesai]

## Cakupan

- Masuk: [perilaku inti yang diminta]
- Di luar cakupan: [batas yang perlu dicatat]
- Kendala dan keputusan terbuka: [keputusan, asumsi, atau bagian yang menunggu jawaban]
- Kontrak teknis yang diperlukan: [perilaku API atau skema data bila terdampak]

## Kriteria selesai

- [ ] Alur utama: [input atau pemicu, tindakan pengguna, hasil yang diharapkan]
- [ ] Skenario gagal yang relevan: [kondisi, perilaku yang diharapkan]
- [ ] Target verifikasi: [pilih sesuai tujuan dan tentukan bukti]

| Tujuan | Bukti |
|---|---|
| Prototipe atau lokal | Jalankan alur utama di lingkungan lokal dengan contoh input dan hasil. |
| Alat internal | Jalankan alur utama lewat akses dan lingkungan yang dipakai tim. |
| Produk publik | Setelah rilis diotorisasi, jalankan alur utama dari akses publik. Catat kanal distribusi dan target yang sesuai proyek, bila peluncuran masuk scope. |

Catat tugas dan hasil verifikasi di satu `PLAN.md` dengan
[template implementation plan](IMPLEMENTATION_PLAN.md).
