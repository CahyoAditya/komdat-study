# Belajar Komunikasi Data dan Jaringan Komputer

Catatan belajar sederhana untuk mata kuliah **KOM1314 — Komunikasi Data dan Jaringan Komputer**. Bab 1–7 mengikuti urutan setiap subbab buku, lalu menjelaskan konsep, mekanisme, contoh hitungan, dan latihan dengan bahasa sederhana. Bab 1 telah diperluas berdasarkan isi buku dan seluruh slide kuliah Bab 1.

## Mulai dari sini

Untuk mengulang hitungan pada poin kuliah dalam kontrak, buka [cheatsheet rumus Bab 1–7](cheatsheet-bab-01-07.md). Isinya hanya rumus yang dikelompokkan menurut bab agar mudah disalin ke kertas. [Gambar line coding](diagram-line-coding.svg) tetap tersedia terpisah. Bab 7 dijadwalkan setelah UTS; catatan tiap bab tersedia untuk mekanisme, contoh, dan latihan lebih rinci.

1. Baca [Bab 1 — Jaringan Komputer dan Layanannya](bab-01-jaringan-dan-layanan.md).
2. Lanjutkan [Bab 2 — Aplikasi dan Arsitektur Berlapis](bab-02-aplikasi-dan-arsitektur-berlapis.md).
3. Lanjutkan [Bab 3 — Dasar Transmisi Digital](bab-03-dasar-transmisi-digital.md), [Bab 4 — Jaringan Sirkuit](bab-04-jaringan-sirkuit.md), [Bab 5 — Protokol Antartitik dan Data Link](bab-05-protokol-antartitik-dan-data-link.md), [Bab 6 — Akses Media dan LAN](bab-06-akses-media-dan-lan.md), lalu [Bab 7 — Jaringan Paket](bab-07-jaringan-paket.md).
4. Coba latihan di akhir tiap bab tanpa melihat pembahasan. [Latihan bergaya UTS Bab 1](bab-01-jaringan-dan-layanan.md#latihan-bergaya-uts-20182019) dan [Bab 2](bab-02-aplikasi-dan-arsitektur-berlapis.md#latihan-bergaya-uts-20182019) memakai pola dari contoh UTS 2018/2019.

## Peta materi menurut kontrak perkuliahan

| Pertemuan | Pokok bahasan | Bacaan dalam repo |
|---|---|---|
| 1 | Jaringan komputer dan layanannya; sejarah, klasifikasi, perancangan, faktor evolusi | [Bab 1](bab-01-jaringan-dan-layanan.md) |
| 2 | Aplikasi, arsitektur berlapis, OSI dan TCP/IP | [Bab 2](bab-02-aplikasi-dan-arsitektur-berlapis.md) |
| 3–4 | Sinyal, kanal, transmisi, pengkodean dan media | [Bab 3](bab-03-dasar-transmisi-digital.md) |
| 5 | Multiplexing, switching dan jaringan telepon | [Bab 4](bab-04-jaringan-sirkuit.md) |
| 6 | Protokol antartitik dan ARQ | [Bab 5](bab-05-protokol-antartitik-dan-data-link.md) |
| 7 | LAN, Ethernet dan kendali akses media | [Bab 6](bab-06-akses-media-dan-lan.md) |
| 8 | Ujian tengah semester | — |
| 9 | Jaringan packet switching | [Bab 7](bab-07-jaringan-paket.md) |

Urutan ini mengikuti **pertemuan 1–9** dalam `kom1314_KDJK-kontrak-perkuliahan_Sem1-202627-signed.pdf`. Bab 7 berada *sesudah* UTS pada kontrak. Ujian tetap mengikuti pengumuman dosen; tabel ini hanya peta belajar, bukan kisi-kisi resmi.

## Susunan menurut buku

| Bab | Subbab buku | Letak penjelasan |
|---|---|---|
| 1 | 1.1 Evolusi arsitektur; 1.2 arsitektur masa depan; 1.3 faktor evolusi | [Bab 1](bab-01-jaringan-dan-layanan.md) |
| 2 | 2.1 Contoh protokol; 2.2 OSI; 2.3 TCP/IP; 2.4 Berkeley API; 2.5 protokol aplikasi dan utilitas | [Bab 2](bab-02-aplikasi-dan-arsitektur-berlapis.md) |
| 3 | 3.1 Representasi informasi; 3.2 komunikasi digital; 3.3 analog menjadi digital; 3.4 kanal; 3.5 batas; 3.6 line coding; 3.7 modem; 3.8 media; 3.9 galat | [Bab 3](bab-03-dasar-transmisi-digital.md) |
| 4 | 4.1 Multiplexing; 4.2 SONET; 4.3 transport; 4.4 switch; 4.5 telepon; 4.6 pensinyalan; 4.7 trafik; 4.8 seluler | [Bab 4](bab-04-jaringan-sirkuit.md) |
| 5 | 5.1 Model layanan; 5.2 ARQ; 5.3 protokol lain; 5.4 framing; 5.5 PPP; 5.6 HDLC; 5.7 berbagi link | [Bab 5](bab-05-protokol-antartitik-dan-data-link.md) |
| 6 | 6.1–6.5 akses media; 6.6–6.11 standar LAN, Wi-Fi, dan bridge | [Bab 6](bab-06-akses-media-dan-lan.md) |
| 7 | 7.1–7.5 layanan dan routing; 7.6 ATM; 7.7–7.9 pengelolaan trafik | [Bab 7](bab-07-jaringan-paket.md) |

Nomor subbagian seperti 1.1.1 dan 7.8.2 juga mengikuti daftar isi buku. Materi pelengkap kontrak kuliah diberi bagian tersendiri. Bagian pilihan dalam daftar isi tetap diperkenalkan dan diberi tanda; cakupan ujian mengikuti arahan dosen.

## Fokus Bab 1 dalam 60 menit

- **10 menit:** pahami *aplikasi*, *layanan*, *jaringan*, arsitektur, *switch*, dan *multiplexer*.
- **20 menit:** ikuti mekanisme telegraf, telepon, terminal, ARPANET, Ethernet, dan Internet.
- **15 menit:** bedakan pesan utuh, sirkuit, paket, serta keterlambatan dan mutu layanan.
- **15 menit:** baca faktor teknologi, regulasi, pasar, dan standar; lalu kerjakan latihan.

## Fokus Bab 2 dalam 35 menit

- **10 menit:** pahami contoh membuka situs pada 2.1: DNS → TCP → HTTP.
- **10 menit:** bedakan lapisan, layanan, dan protokol; pahami pembungkusan data.
- **10 menit:** bandingkan tujuh lapisan OSI (2.2) dengan TCP/IP (2.3).
- **5 menit:** baca peta 2.4–2.5, lalu jawab latihan dan periksa kunci.

## Sumber

- Kontrak perkuliahan KOM1314 Semester 1 2026/2027, khususnya tabel topik pertemuan 1–2.
- Alberto Leon-Garcia dan Indra Widjaja, *Communication Networks: Fundamental Concepts and Key Architectures*, edisi ke-2, Bab 1–2. Nama salinan menyebut “2003”, sedangkan halaman hak cipta edisi kedua mencantumkan 2004. **Koreksi:** salinan PDF yang tersedia memuat isi Bab 1 lengkap, bukan hanya halaman awal dan daftar isi. Penjelasan Bab 1 merujuk langsung ke bab tersebut dan slide kuliah.
- Slide kuliah `Garcia-2003_ch1-Communication Networks and Services` dalam paket materi Sensei.
- Slide kuliah `Garcia-2003_ch2-Applications and Layered Architectures.pdf` dalam paket materi Sensei.
- Slide kuliah Bab 3, Bab 5, dan Bab 6 dalam paket materi Sensei. Nama file slide Bab 5 dan 6 menyebut *Digital Transmission Fundamentals*, tetapi halaman pembukanya menunjukkan bab yang sesuai. Paket tersebut tidak memuat slide khusus Bab 4 dan 7; dua bab itu dijelaskan dari buku dan kontrak kuliah.
- `UTS KDJK 2019.pdf` dari paket contoh soal Sensei, yaitu UTS KDJK semester genap 2018/2019. Format pilihan ganda, menjodohkan, dan uraian singkat dijadikan patokan untuk latihan baru Bab 1–2. Soal asli juga memuat materi pertemuan 3–7 yang belum dicakup dua bab ini.

Catatan ini merupakan penjelasan ulang, bukan salinan buku atau slide. Materi asli tidak disertakan karena hak cipta.
