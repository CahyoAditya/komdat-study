# Cheatsheet KOM1314 Bab 1–7 menurut kontrak kuliah

Rangkuman ini mengikuti subpokok bahasan **pertemuan 1–7 dan 9** dalam kontrak KOM1314 Semester Ganjil 2026/2027. Pertemuan 8 adalah UTS; **Bab 7 dijadwalkan sesudah UTS**. Ini peta belajar berdasarkan kontrak, bukan jaminan cakupan ujian. Untuk penjelasan dan latihan lebih rinci, buka [catatan tiap bab](README.md).

| Pertemuan | Fokus kontrak | Bagian |
|---|---|---|
| 1 | Sejarah, klasifikasi, perancangan, evolusi jaringan | Bab 1 |
| 2 | Layering, OSI, TCP/IP, Berkeley API, aplikasi/utilitas | Bab 2 |
| 3–4 | Sinyal, representasi, kanal, batas, coding, modulasi, media, galat | Bab 3 |
| 5 | Multiplexing, SONET, WDM, switch, PSTN, pensinyalan, trafik | Bab 4 |
| 6 | Peer-to-peer, ARQ, adaptasi, data link, link sharing | Bab 5 |
| 7 | Multiakses, LAN, IEEE 802.x, bridge, protokol data link | Bab 6 |
| 9 | Topologi paket, datagram/VC, routing, jalur terpendek | Bab 7 |

Diagram teks tetap terlihat dalam satu dokumen yang dibuka dari WhatsApp. Gambar line coding berwarna juga tersedia di bawah.

## Bab 1 — Jaringan komputer dan layanannya (pertemuan 1)

### Overview dan sejarah telekomunikasi/jaringan komputer

Jaringan menghubungkan perangkat agar aplikasi dapat memakai layanan komunikasi. **Aplikasi** adalah kegiatan pengguna, **layanan** adalah kemampuan yang diberikan jaringan, dan **protokol** adalah aturan pertukaran data.

    Telegraf: pesan utuh → stasiun simpan → teruskan → tujuan
    Telepon:  minta sambungan → jalur tersedia → bicara → lepas jalur
    Internet: data → paket A/B/C → jalur bersama → susun di tujuan

- **Message switching:** simpul menerima pesan utuh sebelum meneruskan; pesan panjang dapat menahan pesan lain.
- **Circuit switching:** sambungan disiapkan selama panggilan; delay relatif teratur sesudah setup, tetapi kapasitas dapat menganggur.
- **Packet switching:** pesan dipecah menjadi paket yang berbagi link; antrean dan waktu tiba dapat berubah.
- **Store-and-forward:** simpul menerima satu unit data lengkap sebelum meneruskan. Unit itu bisa pesan atau paket.
- Telegraf memakai kode karakter, telepon memakai sentral dan trunk, terminal awal berbagi host, ARPANET mengembangkan jaringan paket, Ethernet menghubungkan perangkat LAN, dan Internet menghubungkan banyak jaringan berbeda.

### Taksonomi dan klasifikasi

| Dilihat dari | Contoh | Pertanyaan |
|---|---|---|
| Cakupan | PAN, LAN, MAN, WAN | Seberapa luas? |
| Topologi | bus, bintang, ring, mesh | Bagaimana simpul tersambung? |
| Media | tembaga, serat optik, radio | Lewat apa sinyal berjalan? |
| Berbagi sumber daya | sirkuit, paket | Kapasitas dipesan atau dibagi? |
| Arah | simplex, half-duplex, full-duplex | Siapa bisa mengirim bersamaan? |
| Kebutuhan layanan | suara, data, video | Prioritas laju, delay, kehilangan? |

    Bus:     A──B──C──D          Bintang:  A   B
                                        \ /
                                         S
                                        / \
                                       C   D
    Ring:    A──B               Mesh:   A───B
             │  │                       │╲ ╱│
             D──C                       │ ╳ │
                                       │╱ ╲│
                                       D───C

Topologi fisik menunjukkan sambungan yang dipasang; topologi logis menunjukkan arah data mengalir. Keduanya dapat berbeda.

### Pendekatan perancangan dan faktor evolusi

Rancang dari kebutuhan aplikasi → ukur laju, delay, kehilangan, jangkauan, biaya, keandalan → pilih media, topologi, cara berbagi → tentukan alamat dan rute → uji beban dan gangguan. **QoS** menyatakan sasaran kinerja. Suara interaktif peka terhadap delay dan jitter; transfer file peka terhadap keutuhan.

Evolusi dipengaruhi **teknologi**, **regulasi**, **pasar**, dan **standar**. Standar memungkinkan perangkat berbeda bekerja bersama. Efek jaringan berarti layanan cenderung makin berguna saat makin banyak pihak dapat saling terhubung.

## Bab 2 — Aplikasi dan arsitektur berlapis (pertemuan 2)

### Konsep dan contoh layering

**Lapisan** membagi tugas; **layanan** diberikan lapisan bawah kepada atas; **protokol** mengatur percakapan logis antarlapisan setingkat. Data sebenarnya turun di pengirim dan naik di penerima.

    Pengirim                                      Penerima
    Aplikasi    data ─── percakapan logis ───►  Aplikasi
        ↓                                             ↑
    Transport   [TCP | data]                     Transport
        ↓                                             ↑
    Internet    [IP | TCP | data]                Internet
        ↓                                             ↑
    Link        [MAC | IP | TCP | data | FCS]     Link
        └────────── sinyal pada media ────────────────┘

Penambahan header disebut **encapsulation**; pelepasan header di penerima disebut **decapsulation**. Router mengganti frame untuk link berikutnya, lalu memakai alamat IP untuk menentukan hop. Contoh membuka situs: DNS mencari alamat → transport menghubungkan bila diperlukan → HTTP meminta halaman → IP dan link mengangkut data.

### OSI dan TCP/IP

| OSI | Tugas | Padanan TCP/IP praktis |
|---|---|---|
| 7 Aplikasi | Fungsi aplikasi | Aplikasi |
| 6 Presentasi | Format data | Umumnya bagian aplikasi |
| 5 Sesi | Pengelolaan dialog | Umumnya bagian aplikasi |
| 4 Transport | Komunikasi antarproses | TCP/UDP |
| 3 Jaringan | Alamat dan rute | IP |
| 2 Data link | Frame satu link | Ethernet/Wi-Fi |
| 1 Fisik | Bit menjadi sinyal | Fisik/media |

OSI adalah **model acuan**; Internet tidak wajib mempunyai tujuh protokol terpisah. Alamat **MAC** menunjuk antarmuka link lokal; **IP** menunjuk tujuan antarnetwork; **port** menunjuk proses. TCP memberi aliran byte berurutan dan mekanisme keandalan; UDP memberi datagram tanpa jaminan keandalan dari UDP sendiri.

### Berkeley API; aplikasi dan utilitas TCP/IP

    Server TCP: socket → bind → listen → accept → read/write → close
    Klien TCP: socket → connect ────────────────→ read/write → close
    UDP:       socket → sendto / recvfrom → close

Socket adalah titik komunikasi program. Bind memilih alamat/port server, listen menunggu, dan accept menerima koneksi klien. Datagram UDP biasa tidak memerlukan listen/accept.

| Nama | Fungsi singkat |
|---|---|
| DNS | Mencari alamat IP dari nama |
| HTTP | Permintaan dan respons web |
| SMTP | Mengirim surel |
| FTP | Transfer file klasik; jalur kontrol dan data terpisah |
| Telnet | Terminal jarak jauh klasik tanpa enkripsi |
| ping | Memakai ICMP Echo; tidak dijawab bukan bukti host mati |
| traceroute | Melihat hop yang merespons di lintasan |

## Bab 3 — Dasar sistem komunikasi data (pertemuan 3–4)

### Deret/transformasi; karakteristik analog dan digital

Sinyal analog berubah kontinu; data digital menggunakan simbol diskret, tetapi tetap melewati gelombang fisik. **Deret Fourier** mengurai sinyal periodik menjadi komponen sinus; **transformasi Fourier** menggambarkan isi frekuensi sinyal. Pulsa dengan tepi tajam memerlukan komponen frekuensi tinggi. Kanal sempit membulatkan tepinya.

    Pulsa kirim:   ┌───┐   ┌───┐
               ────┘   └───┘   └───
    Kanal sempit → tepi membulat → simbol tetangga bisa bercampur

Amplitudo adalah besar sinyal; periode T ialah lama satu siklus; frekuensi f = 1/T; fase adalah posisi dalam siklus. **Bandwidth (Hz)** adalah rentang frekuensi, **baud** adalah simbol/detik, dan **bit rate** adalah bit/detik.

### Representasi informasi digital

Pola bit memperoleh makna dari aturan representasi: 01000001 dapat berarti huruf A dalam ASCII atau angka 65. Analog menjadi digital melalui PCM:

    suara → filter antialias → sampling → kuantisasi → kode biner
                                      (nilai dibulatkan)

Untuk sinyal yang frekuensi tertingginya di bawah B Hz, laju sampling ideal lebih dari 2B sampel/detik. **Bit rate PCM mentah = sampel/detik × bit/sampel.** Contoh 8.000 sampel/s × 8 bit = 64 kb/s; frekuensi suara harus dibatasi di bawah 4 kHz dalam contoh tersebut. Kuantisasi menimbulkan galat pembulatan. Kompresi lossless pulih persis; lossy tidak.

### Karakteristik kanal dan batas transmisi

    pulsa ──► [redaman + derau + distorsi + ISI] ──► pulsa berubah

Redaman melemahkan sinyal; derau menambah gangguan; distorsi mengubah bentuk/waktu; **ISI** membuat simbol bertetangga saling mengganggu. SNR = daya sinyal/daya derau; SNR dalam dB = 10 log10(SNR linear).

| Soal | Rumus | Catatan |
|---|---|---|
| Waktu memasukkan L bit ke link | L/R | R dalam bit/s |
| Waktu rambat jarak d | d/v | v dalam m/s |
| Laju kanal ideal Nyquist | 2B log₂ M | B Hz; M tingkat simbol; tanpa derau |
| Kapasitas Shannon | B log₂(1+S/N) | S/N rasio linear, bukan dB |

Antrean dan pemrosesan menambah delay. Nyquist dan Shannon ialah batas dari model berbeda; jangan dijumlahkan.

### Pengkodean saluran: visual line coding

Contoh bit **1 0 1 1 0 0 1**. Konvensi contoh: NRZ-L memakai 1=H dan 0=L; Manchester memakai 1=L→H dan 0=H→L; AMI memakai 0=0 V dan bit 1 bergantian +V/−V. Buku atau standar lain dapat membalik polaritas; prinsipnya tetap sama.

| Bit ke- | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| Bit | 1 | 0 | 1 | 1 | 0 | 0 | 1 |
| NRZ-L per bit | H | L | H | H | L | L | H |
| Manchester setengah awal→akhir | L→H | H→L | L→H | L→H | H→L | H→L | L→H |
| AMI per bit | + | 0 | − | + | 0 | 0 | − |

    batas bit:   | 1  | 0  | 1  | 1  | 0  | 0  | 1  |
    NRZ-L:       | HH | LL | HH | HH | LL | LL | HH |
    Manchester:  | LH | HL | LH | LH | HL | HL | LH |
    AMI:         | ++ | 00 | -- | ++ | 00 | 00 | -- |

![Perbandingan bentuk gelombang NRZ-L, Manchester, dan AMI](https://raw.githubusercontent.com/CahyoAditya/komdat-study/main/diagram-line-coding.svg)

**NRZ** mudah tetapi rentetan bit sama menyulitkan sinkronisasi. **Manchester** punya transisi tengah setiap bit sehingga mudah disinkronkan, dengan lebih banyak perubahan sinyal. **AMI** membuat 1 bergantian polaritas dan mengurangi komponen DC; rentetan 0 tetap sulit. Scrambling dapat membantu memecah rentetan.

### Modem, media, dan galat

Modem memetakan bit ke simbol pembawa dan memutuskan kembali simbol yang diterima. **ASK** mengubah amplitudo, **FSK** frekuensi, **PSK** fase, dan **QAM** amplitudo serta fase. Jika ada M simbol, tiap simbol ideal membawa log₂ M bit. Contoh 16-QAM pada 1.000 baud = 4.000 bit/s mentah sebelum overhead.

| Media | Ciri |
|---|---|
| Twisted pair | Murah; pilinan membantu mengurangi gangguan |
| Koaksial | Konduktor terlindung; pernah dipakai pada LAN bersama |
| Serat optik | Kapasitas besar dan tahan interferensi elektromagnetik |
| Radio | Mobilitas, tetapi dipengaruhi interferensi/pantulan/halangan |

**Parity** dapat mendeteksi satu bit salah, tetapi dua kesalahan bisa lolos. **CRC** mengirim sisa pembagian polinomial biner dengan operasi XOR; penerima memeriksa sisa ulang. **ARQ** mengirim ulang data yang rusak/hilang; **FEC** menambah redundansi untuk memperbaiki sejumlah galat tanpa kirim ulang.

## Bab 4 — Sistem transmisi dan jaringan telekomunikasi (pertemuan 5)

### Multiplexing, SONET, WDM

    FDM: frekuensi  | A |jeda| B |jeda| C |  bersamaan
    TDM: waktu     | A | B | C | A | B | C |  bergiliran
    WDM: serat     λA + λB + λC          warna cahaya berbeda

FDM membagi pita frekuensi; TDM sinkron membagi slot tetap walau sumber diam; TDM statistik memberi slot menurut aktivitas dan membutuhkan antrean; WDM membagi panjang gelombang optik. **Multiplexing** menggabungkan aliran; **switching** memilih sambungan/keluaran.

SONET mengirim frame optik sinkron. STS-1: 9 × 90 byte/frame × 8 bit/byte × 8.000 frame/s = **51,84 Mb/s**, termasuk overhead. STS-3 nominal 155,52 Mb/s. WDM membawa beberapa aliran optik pada serat yang sama; add/drop mengambil atau menambah aliran tertentu.

### Circuit switches, PSTN, pensinyalan

    pelanggan A → local loop → sentral A == trunk == sentral B → pelanggan B
    panggilan: permintaan → siapkan jalur → komunikasi → lepas jalur

Space-division switch memilih jalur fisik; time-division switch memetakan slot masuk ke slot keluar. **Blocking** berarti panggilan baru tidak mendapat jalur/kapasitas. PSTN adalah jaringan telepon publik; sentral mengumpulkan pelanggan dan trunk menghubungkan sentral. Pensinyalan mengurus setup, pengawasan, dan pelepasan panggilan. SS7 adalah contoh jaringan pensinyalan telepon, yang jalur kendalinya dapat terpisah dari jalur suara.

### Trafik dan kontrol overload

**1 erlang** = rata-rata satu sirkuit sibuk selama seluruh periode pengamatan. Contoh 30 panggilan/jam × 2 menit/panggilan ÷ 60 menit = 1 erlang. Ini ukuran beban rata-rata, bukan jumlah panggilan. Ketika permintaan melampaui kapasitas, panggilan dapat terblokir; rute alternatif, pembatasan permintaan baru, dan penambahan kapasitas membantu mengatasi overload.

## Bab 5 — Protokol peer-to-peer (pertemuan 6)

### Model layanan dan ARQ

Peer-to-peer di sini berarti dua **entitas protokol setingkat** yang saling bertukar data, bukan aplikasi berbagi file P2P. Data link menentukan batas frame, memeriksa galat, dan bila diperlukan memberi ACK/nomor urut serta kendali aliran. Keberhasilan pada satu link belum menjamin pengiriman ujung ke ujung.

    Pengirim       frame #0 ─────────► Penerima
                  ◄───────── ACK #0
                  frame #1 ──X         hilang
                  ... timer habis
                  frame #1 ─────────►  ulang

**ARQ = deteksi galat + nomor urut + ACK/NAK + timer + retransmisi.** Nomor urut mencegah data duplikat diserahkan dua kali bila ACK hilang.

| Metode | Frame belum di-ACK | Bila frame 2 hilang tetapi 3–4 tiba |
|---|---|---|
| Stop-and-Wait | Satu | Tunggu/ulang 2; sederhana, tetapi link jauh menganggur |
| Go-Back-N | Hingga W | Buang 3–4, lalu ulang 2–4 |
| Selective Repeat | Hingga W | Simpan 3–4, lalu ulang hanya 2 |

    GBN:  1 2 3 4 → 2 hilang → ulang [2 3 4]
    SR:   1 2 3 4 → 2 hilang → ulang [2] saja

Efisiensi Stop-and-Wait tanpa galat kira-kira T_frame/(T_frame + 2T_prop + T_ACK), jika pemrosesan diabaikan. Untuk nomor urut m bit, batas umum jendela GBN W ≤ 2^m−1; SR dengan jendela kirim/terima sama W ≤ 2^(m−1).

### Adaptasi, pengontrolan data link, link sharing

**Error control** menangani data rusak/hilang; **flow control** mencegah penerima kewalahan; **congestion control** mengurangi beban jaringan. **Framing** menemukan batas frame melalui panjang eksplisit, byte pembatas plus byte stuffing, atau pola bit plus bit stuffing. HDLC memakai flag 01111110; setelah lima bit 1 berurutan pada data, bit 0 disisipkan agar pola flag tidak muncul. PPP membungkus data pada link dua ujung; LCP mengatur link dan NCP mengatur protokol jaringan. PPP sendiri tidak menjamin ARQ.

**Link sharing** memberi kapasitas kosong kepada aliran yang aktif. Jika banyak aliran tiba serentak melebihi laju link, antrean bertambah dan frame dapat dibuang.

## Bab 6 — Protokol LAN (pertemuan 7)

### Multiakses dan protokol data link

Pada media bersama, dua stasiun dapat mengirim serentak dan bertabrakan. **Random access** membolehkan mencoba lalu mengulang; **scheduling** membagi giliran.

| Metode | Mekanisme | Ingatan |
|---|---|---|
| Pure ALOHA | Kirim kapan saja | Periode rawan 2 waktu frame |
| Slotted ALOHA | Kirim pada batas slot | Periode rawan 1 waktu frame |
| CSMA | Dengarkan kanal dulu | Masih bisa tabrakan karena waktu rambat |
| CSMA/CD | Deteksi tabrakan sambil mengirim | Ethernet bus/hub historis |
| CSMA/CA | Kurangi peluang tabrakan; tunggu ACK | Wi-Fi |
| Polling | Pengendali memberi giliran | Ada overhead dan titik kendali pusat |
| Token passing | Pemegang token mendapat giliran | Token perlu dijaga |

Dalam model ideal, puncak Pure ALOHA 1/(2e) ≈ 18,4% dan Slotted ALOHA 1/e ≈ 36,8%; ini bukan throughput Ethernet atau Wi-Fi. FDMA/TDMA/CDMA membagi pengguna menurut frekuensi/waktu/kode.

### Standar LAN dan bridges

IEEE 802 adalah keluarga standar LAN. Ethernet 802.3 memakai alamat MAC dan FCS. Ethernet **switch full-duplex** modern tidak memakai CSMA/CD untuk tabrakan pada link tersebut. Wi-Fi 802.11 memakai CSMA/CA karena radio sulit mendeteksi tabrakan sambil memancar. Token Ring dan FDDI adalah contoh historis akses token.

    A ─ port 1 ┐
    B ─ port 2 ├── switch/bridge ── port 3 ─ C
               └── tabel: MAC sumber → port masuk

Switch/bridge belajar **alamat sumber** pada port masuk. Tujuan yang dikenal dikirim ke port terkait; tujuan yang belum dikenal dibanjiri ke port lain. Spanning tree mencegah loop aktif pada bridge. Router memilih hop menurut IP antarnetwork; bridge/switch meneruskan frame menurut MAC di LAN. **Hidden terminal** pada Wi-Fi terjadi saat dua pengirim tak saling mendengar tetapi sama-sama menuju penerima; RTS/CTS dapat membantu dalam kondisi tertentu.

## Bab 7 — Jaringan packet switching (pertemuan 9, setelah UTS)

### Topologi paket, datagram, dan virtual circuit

    Host → router A → router B → tujuan
                  ╲            ↑
                   → router C ─┘   jalur alternatif

Router memeriksa tujuan → memilih keluaran → mengantre bila perlu → mengirim paket. Delay satu hop = pemrosesan + antrean + transmisi + propagasi. Jika buffer penuh, paket dapat dibuang.

| | Datagram | Virtual circuit (VC) |
|---|---|---|
| Sebelum transfer | Tanpa setup jalur wajib | Setup membuat keadaan/label |
| Forwarding | Menurut alamat tujuan per paket | Menurut label VC pada link |
| Jalur | Bisa berbeda antarpaket | Umumnya sama selama VC hidup |
| Jika jalur gagal | Rute baru setelah pembaruan | VC perlu dialihkan/dibangun ulang |

VC tetap mengirim **paket**, bukan sirkuit fisik telepon. **Forwarding** memakai tabel sekarang; **routing** membuat/memperbarui tabel tersebut.

### Perutean dan jalur terpendek

Bobot link bisa berupa hop, biaya, atau delay. “Terpendek” berarti **jumlah bobot minimum**, bukan selalu jarak fisik.

    A ──1── B ──1── D
     ╲              ↑
      4             3
       ╲            │
        C ──────────┘

Dari A ke D: A→B→D bernilai 1+1=2; A→C→D bernilai 4+3=7. **Dijkstra/link state** memilih simpul sementara termurah dan memperbarui jarak tetangganya; versi standar memerlukan bobot nonnegatif. **Bellman–Ford/distance vector** memperbarui jarak melalui tetangga: D_x(y) = min_v {c(x,v) + D_v(y)}; pembaruan kegagalan yang lambat dapat menimbulkan count-to-infinity. Routing statis relatif tetap; routing dinamis menyesuaikan perubahan. Konvergensi adalah proses hingga tabel kembali stabil.

## Cek cepat sebelum latihan

| Petunjuk soal | Langkah pertama |
|---|---|
| Bit versus simbol | baud × bit/simbol = bit/s mentah |
| Delay paket | Pisahkan transmisi, propagasi, antrean, pemrosesan |
| Gambar line coding | Nyatakan polaritas; bagi tiap bit; periksa transisi tengah/AMI |
| Telepon | Setup → komunikasi → lepas; periksa blocking |
| ACK hilang | Lihat nomor urut dan timer |
| Paket 2 hilang, 3–4 tiba | GBN buang 3–4; SR simpan 3–4 |
| Ethernet lama versus Wi-Fi | CSMA/CD versus CSMA/CA |
| Pilihan rute | Jumlahkan bobot tiap link |

**Rujukan:** kontrak perkuliahan KOM1314 Semester Ganjil 2026/2027 (tabel Topik Perkuliahan pertemuan 1–9), buku Leon-Garcia dan Widjaja edisi kedua, serta slide yang tersedia dalam paket materi Sensei. Bab 7 dimasukkan karena Sensei meminta Bab 1–7 walau jadwal kontrak menempatkannya setelah UTS.
