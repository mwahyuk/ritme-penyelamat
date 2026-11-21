# Ritme Penyelamat

Alat latihan ritme kompresi dada CPR (Cardiopulmonary Resuscitation) untuk umum. Banyak orang yang menyaksikan serangan jantung di tempat umum ragu melakukan CPR karena tidak tahu ritme yang benar. Alat ini memungkinkan latihan kapan saja, tanpa internet, langsung di browser.

> **Perhatian:** Alat ini hanya untuk melatih ritme, bukan pengganti pelatihan CPR bersertifikat.

---

## Fitur

**3 Mode Latihan**
- **Latihan Terpandu** — metronom berbunyi sepanjang sesi di 110 BPM
- **Uji Ritme** — metronom hanya 10 detik pertama, lalu Anda meneruskan sendiri; hasil menampilkan akurasi fase terpandu vs fase mandiri terpisah
- **Mode 30:2** — 30 kompresi diikuti jeda napas 5 detik, lalu kompresi kembali

**Gauge BPM Real-Time**
- Zona tiga warna + label + ikon: Terlalu lambat (< 100) / Pas! (100–120) / Terlalu cepat (> 120)
- Peringatan "Jangan berhenti!" jika tidak ada tekanan lebih dari 2 detik

**Hasil Sesi**
- Akurasi (% kompresi dalam zona), rata-rata BPM, skor konsistensi
- Waktu tangan berhenti, fraksi kompresi
- Grafik BPM sepanjang waktu (Canvas)

**Riwayat**
- 10 sesi terakhir tersimpan di `localStorage`; skor terbaik ditandai

---

## Cara Menjalankan

**Offline:** unduh atau clone repo, buka `index.html` di browser modern. Tidak perlu server.

**GitHub Pages:** [https://mwahyuk.github.io/ritme-penyelamat/](https://mwahyuk.github.io/ritme-penyelamat/)

---

## Catatan Teknis

- Satu file `index.html` — semua CSS dan JS inline, tanpa library eksternal
- Metronom via **Web Audio API** dengan look-ahead scheduling; tidak menggunakan `setInterval` untuk timing audio
- Tap diukur dengan `performance.now()` pada `pointerdown`; `Space` didukung sebagai input keyboard
- Berjalan sepenuhnya offline, responsif dari 360 px hingga desktop
- Dark mode otomatis via `prefers-color-scheme`; aksesibilitas WCAG AA
