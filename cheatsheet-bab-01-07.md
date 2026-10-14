# Cheatsheet Rumus Komunikasi Data — Bab 1–7

Rumus dari [materi Bab 1–7](README.md), disusun menurut bab dan topik kontrak. Bab 7 dijadwalkan sesudah UTS. Laju mentah belum dikurangi overhead, kecuali disebut lain.

## Bab 1 — Jaringan dan layanan

- Pasangan sambungan langsung untuk N pengguna: E = N(N−1)/2.
- PCM suara: R_PCM = f_s × b bit/s. Contoh 8.000 sampel/s × 8 bit/sampel = 64 kb/s.
- Contoh T1: 24 × 64 kb/s = 1,536 Mb/s muatan; tambahan 8 kb/s pembingkaian → 1,544 Mb/s.

## Bab 2 — Aplikasi dan arsitektur berlapis

- Pembungkusan data: L_kirim = L_data + Σ L_header + Σ L_trailer. Hitung hanya pembungkus yang benar-benar ditambahkan. Bab ini tidak memiliki rumus hitungan khusus lain dalam materi.

## Bab 3 — Sinyal, kanal, pengkodean, dan galat

- Periode dan frekuensi: T = 1/f; f = 1/T.
- Simbol M tingkat: bit/simbol = log₂ M; laju bit mentah R_b = R_s log₂ M. R_s dalam simbol/s atau baud.
- Kuantisasi b bit: M = 2^b tingkat.
- Sampling ideal: f_s > 2 f_maks sampel/s untuk sinyal yang dibatasi pita sebelum sampling.
- Laju PCM: R_PCM = f_s b bit/s.
- Perkiraan SNR kuantisasi untuk sinus skala penuh dan kuantisasi seragam: SNR_q,dB ≈ 6,02b + 1,76 dB.
- Rasio sinyal/derau daya: SNR = S/N; SNR_dB = 10 log₁₀(S/N); S/N = 10^(SNR_dB/10).
- Waktu memasukkan L bit ke link: t_tx = L/R_b. Waktu rambat: t_prop = d/v.
- Delay satu hop: t = t_proses + t_antre + t_tx + t_prop.
- Nyquist, kanal ideal tanpa derau: R_s,maks = 2B simbol/s; R_b,maks = 2B log₂ M bit/s.
- Shannon, kanal berderau: C = B log₂(1 + S/N) bit/s.
- BER terukur = jumlah bit salah / jumlah bit terkirim.
- CRC dengan generator G(x) berderajat r: tambah r bit; R(x) = [x^r D(x)] mod G(x); C(x) = x^r D(x) + R(x), dengan penjumlahan XOR.
- Kode berjarak minimum d_min: deteksi hingga d_min−1 bit salah; koreksi hingga ⌊(d_min−1)/2⌋ bit salah per kata kode.

B = bandwidth kanal dalam Hz; v = kecepatan rambat. Masukkan S/N sebagai rasio biasa ke rumus Shannon, bukan dB. Jangan jumlahkan batas Nyquist dan Shannon.

## Bab 4 — Multiplexing, SONET, dan jaringan sirkuit

- N aliran TDM berkecepatan sama R: R_muatan = NR; laju fisik menampung overhead tambahan.
- Laju frame sinkron = (byte/frame) × 8 × (frame/s).
- SONET STS-1: 9 × 90 × 8 × 8.000 = 51,84 Mb/s, termasuk overhead.
- SONET STS-3 nominal: 3 × 51,84 = 155,52 Mb/s.
- Trafik ditawarkan: A = λh = (jumlah panggilan × durasi rata-rata) / waktu pengamatan, dalam erlang. λ dan h harus memakai satuan waktu yang cocok. Contoh 30 panggilan/jam × (2/60) jam = 1 erlang.
- Crossbar dengan N masukan dan M keluaran: N × M titik silang secara konseptual.

## Bab 5 — Protokol antartitik dan ARQ

- Lama pengiriman frame: T_f = L_f/R.
- Efisiensi Stop-and-Wait tanpa galat: U ≈ T_f/(T_f + 2T_p + T_ACK), abaikan waktu proses. Jika ACK sangat singkat, a = T_p/T_f dan U ≈ 1/(1+2a).
- Jendela maksimum Go-Back-N dengan nomor urut m bit: W ≤ 2^m−1.
- Jendela maksimum Selective Repeat, ukuran jendela kirim/terima sama: W ≤ 2^(m−1).
- Perkiraan jendela agar link terus terisi: W ≥ ⌈(T_f + 2T_p + T_ACK)/T_f⌉, bila tanpa galat dan penerima mampu menampungnya.

T_p = waktu rambat satu arah. Efisiensi di atas belum memasukkan pengiriman ulang atau overhead lain.

## Bab 6 — Akses media dan LAN

- Lama frame: T_f = L_f/R.
- Pure ALOHA: periode rawan 2T_f; S = G e^(−2G); S_maks = 1/(2e) ≈ 18,4% pada G = 0,5.
- Slotted ALOHA: periode rawan T_f; S = G e^(−G); S_maks = 1/e ≈ 36,8% pada G = 1.
- CSMA/CD pada media bersama, syarat ideal mendeteksi tabrakan: T_frame,min ≥ 2T_p; atau L_min ≥ 2T_p R bit.

G = rata-rata percobaan kirim per waktu frame/slot, termasuk pengiriman ulang. S = throughput ternormalisasi. Rumus ALOHA memakai model ideal; bukan rumus throughput Wi-Fi atau Ethernet modern.

## Bab 7 — Jaringan paket dan perutean

- Delay satu hop: d_hop = d_proses + d_antre + L/R + d/v.
- Delay H hop: d_total = Σᵢ₌₁ᴴ (d_proses,i + d_antre,i + L_i/R_i + d_i/v_i).
- H hop store-and-forward, paket L bit dan semua link berlaju R, tanpa antrean/proses: d_total = H(L/R) + Σᵢ₌₁ᴴ d_i/v_i.
- Biaya rute P: C(P) = Σ_(e∈P) w_e; rute terpendek memiliki C(P) minimum.
- Relaksasi jarak: D(v) ← min[D(v), D(u) + w(u,v)].
- Distance vector/Bellman–Ford: D_x(y) = min_(v tetangga x) {c(x,v) + D_v(y)}.
- Sel ATM 53 byte dengan muatan 48 byte: efisiensi muatan = 48/53 ≈ 90,6%; overhead header = 5/53 ≈ 9,4%.

Gunakan penjumlahan per hop bila laju, ukuran paket, atau antrean tidak sama. Rumus H hop mengasumsikan router menerima paket lengkap sebelum meneruskannya.
