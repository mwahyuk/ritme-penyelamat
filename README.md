# Ritme Penyelamat

**Alat latihan ritme kompresi dada CPR — 100–120 kali per menit**

> **Perhatian:** Alat ini hanya untuk melatih ritme, bukan pengganti pelatihan CPR bersertifikat.

---

## Deskripsi Proyek

Ritme Penyelamat adalah aplikasi web satu halaman yang membantu siapa saja berlatih ritme kompresi dada yang benar untuk CPR (Cardiopulmonary Resuscitation / Resusitasi Jantung Paru). Masalah nyata yang ingin diselesaikan: banyak orang yang menyaksikan serangan jantung di luar rumah sakit ragu untuk melakukan CPR karena tidak tahu ritme yang tepat. Alat ini memungkinkan latihan kapan saja, di mana saja, tanpa koneksi internet.

Proyek ini dibuat untuk **IBM AI Builders Challenge** (jalur IT), nama proyek: *Ritme Penyelamat*.

---

## Fitur Utama

### Mode Latihan

| Mode | Deskripsi |
|---|---|
| **Latihan Terpandu** | Metronom berbunyi sepanjang sesi di 110 BPM. Ikuti irama bunyi. Cocok untuk pemula. |
| **Uji Ritme** | Metronom berbunyi 10 detik pertama sebagai panduan, lalu berhenti. Pengguna harus mempertahankan ritme sendiri. Hasil menampilkan akurasi fase terpandu vs fase mandiri secara terpisah. |
| **Mode 30:2** | Setiap 30 kompresi, muncul jeda napas buatan selama 5 detik, lalu kompresi dilanjutkan. Metronom aktif selama fase kompresi dan berhenti selama jeda napas. Pola CPR standar. |

### Umpan Balik Real-Time

- BPM bergulir dihitung dari 5 interval terakhir, ditampilkan pada indikator tiga zona.
- Setiap zona diberi label teks dan ikon: **🐢 Terlalu lambat** (< 100), **✅ Pas!** (100–120), **🐇 Terlalu cepat** (> 120).
- Warna tidak digunakan sebagai satu-satunya penanda zona (label + posisi + ikon selalu hadir).
- Jika tidak ada tekanan lebih dari 2 detik, muncul peringatan **"Jangan berhenti!"**.

### Sesi dan Timer

- Durasi sesi: **2 menit** dengan hitungan mundur dan penghitung kompresi.
- Hitungan mundur **3-2-1** sebelum sesi dimulai, disertai bunyi metronom; tekanan selama hitungan mundur diabaikan.
- Tombol **Jeda** (atau tekan Esc), **Bisukan**, dan **Hentikan** tersedia selama sesi.
- Tab tersembunyi secara otomatis menjeda sesi.

### Layar Hasil

- Persentase kompresi dalam zona target.
- Rata-rata BPM.
- Skor konsistensi (berdasarkan koefisien variasi interval).
- Waktu tangan berhenti (jumlah jeda antar kompresi lebih dari 2 detik, tidak termasuk jeda napas dan layar jeda).
- Fraksi kompresi (persentase waktu aktif yang digunakan untuk kompresi).
- Grafik BPM sepanjang waktu digambar dengan Canvas (skala devicePixelRatio, warna dari CSS).
- Khusus Mode Uji Ritme: akurasi fase terpandu (0-10 dtk) dan fase mandiri (10 dtk+) ditampilkan terpisah.
- Sesi dengan kurang dari 10 kompresi tidak disimpan; muncul pesan "Sesi terlalu singkat untuk dinilai".

### Riwayat Sesi

- **10 sesi terakhir** tersimpan di `localStorage`.
- Skor terbaik ditandai dengan label **Terbaik**.

### Layar Informasi CPR

Langkah-langkah rantai keselamatan CPR:
1. Periksa kesadaran korban.
2. Hubungi layanan darurat (112/119); minta satu orang menelepon dan satu orang lagi mencari AED.
3. Tekan keras dan cepat di tengah dada, 5–6 cm, 100–120 per menit. CPR hanya kompresi (tanpa napas buatan) dianjurkan untuk penolong awam.
4. Biarkan dada kembali penuh setelah setiap tekanan.
5. Gunakan AED jika tersedia; lanjutkan CPR sampai AED tiba.

---

## Cara Menjalankan

### Offline (Lokal)

Tidak diperlukan server, build, atau instalasi. Cukup:

```
1. Unduh atau clone repository ini.
2. Buka file index.html di browser modern (Chrome, Edge, Firefox, Safari).
```

Semua fitur berjalan sepenuhnya offline.

### GitHub Pages

Akses langsung di:
**[https://mwahyuk.github.io/ritme-penyelamat/](https://mwahyuk.github.io/ritme-penyelamat/)**

---

## Cara Menggunakan

1. Pilih mode latihan di layar utama.
2. Ikuti hitungan mundur 3-2-1.
3. Tekan lingkaran merah (atau tekan **Spasi**) sesuai ritme, seolah sedang menekan dada korban.
4. Pantau indikator BPM dan pertahankan jarum di zona hijau.
5. Lihat hasil setelah 2 menit.

---

## Catatan Teknis

| Aspek | Detail |
|---|---|
| **Metronom** | Web Audio API dengan look-ahead scheduling pada AudioContext clock. Tidak menggunakan `setInterval` untuk timing audio agar tetap akurat. |
| **Pengukuran tap** | `performance.now()` pada event `pointerdown`. Event keyboard `Space` juga didukung; `event.repeat` diabaikan. Pointer non-primer dan interval < 250 ms diabaikan. |
| **Getaran** | `navigator.vibrate(30)` pada setiap tekanan; gagal secara diam-diam jika tidak didukung. |
| **Penyimpanan** | `localStorage` untuk riwayat 10 sesi terakhir. Tidak ada data yang dikirim ke server. |
| **Satu file** | Seluruh CSS dan JS inline dalam `index.html`. Tidak ada library eksternal, tidak ada framework, tidak ada koneksi jaringan saat digunakan. |
| **Responsif** | Berjalan di layar mulai 360 px (ponsel) hingga desktop. |
| **Aksesibilitas** | Kontras WCAG AA pada semua teks (terang dan gelap), fokus keyboard terlihat, live region tunggal untuk pembaca layar, semua overlay memiliki focus trap. |
| **Dark mode** | Otomatis via `@media (prefers-color-scheme: dark)`. |
| **Reduced motion** | Animasi diturunkan sesuai preferensi sistem via `@media (prefers-reduced-motion)`. |

---

## Changelog

### v2.0
- **Fix 1** Skala gauge proporsional (60-100=40%, 100-120=20%, 120-160=40%); posisi label absolut, semua nilai dari CFG.
- **Fix 2** Timer tidak "mengejar" setelah resume; auto-jeda saat tab disembunyikan.
- **Fix 3** Timeout metronom mode Uji Ritme tersimpan di state; sadar-jeda (sisa waktu panduan dilanjutkan setelah resume).
- **Fix 4** Saat mode bisukan: ring visual tetap berdenyut; hanya audio yang dimatikan.
- **Fix 5** Tap: abaikan pointer non-primer, debounce < 250 ms, avgBPM = 60000/mean(interval).
- **Fix 6** "Waktu tangan berhenti": hanya jeda > 2 detik di luar napas/layar jeda. Tambah "Fraksi kompresi".
- **Fix 7** Kontras WCAG AA: `--muted` dinaikkan di mode terang; aksen merah digelapkan di mode gelap.
- **Fix 8** Hapus `aria-live` dari angka BPM/counter/timer; satu live region tersembunyi mengumumkan perubahan zona (debounce 3 dtk), waktu tiap 30 dtk, dan peringatan inaktivitas.
- **Impr 9** Mode 30:2: metronom aktif selama fase kompresi, berhenti selama jeda napas, aktif kembali sesudahnya.
- **Impr 10** Mode Uji Ritme: akurasi fase terpandu (0-10 dtk) dan fase mandiri (10 dtk+) ditampilkan terpisah di layar hasil.
- **Impr 11** Hitungan mundur 3-2-1 dengan bunyi metronom sebelum sesi; tekanan selama hitungan diabaikan.
- **Impr 12** Sesi dengan < 10 kompresi tidak disimpan; muncul pesan "Sesi terlalu singkat untuk dinilai".
- **Impr 13** Hitungan mundur napas berhenti saat sesi dijeda; dilanjutkan dari angka yang sama setelah resume.
- **Impr 14** Semua overlay: fokus pindah ke kontrol utama saat terbuka, terjebak di dalam overlay, kembali ke tombol dada saat ditutup. Satu landmark `<main>` menggantikan `role="main"` pada setiap section.
- **Impr 15** Grafik canvas: diskalakan oleh `devicePixelRatio` untuk tampilan tajam; warna dibaca dari CSS custom properties; digambar ulang saat resize dan perubahan tema.
- **Impr 16** Layar informasi: catatan CPR hanya kompresi (hands-only) untuk penolong awam; langkah darurat menyertakan instruksi mencari AED.

### v1.0
- Rilis awal: tiga mode, gauge BPM, Web Audio metronom, grafik Canvas, riwayat localStorage.

---

## Lisensi

Proyek ini dibuat untuk keperluan edukasi dalam IBM AI Builders Challenge.
