# Rangkuman lengkap Bab 1–7 — Komunikasi Data dan Jaringan Komputer

Rangkuman ini mengikuti urutan subbab *Communication Networks: Fundamental Concepts and Key Architectures*, edisi kedua, sebagaimana dijabarkan dalam [Bab 1](bab-01-jaringan-dan-layanan.md) sampai [Bab 7](bab-07-jaringan-paket.md). Tujuannya mengulang seluruh peta materi, istilah, mekanisme, perbandingan, dan rumus dalam satu bacaan. Untuk penurunan rumus dan latihan, buka bab terkait. Bab 7 ditempatkan setelah UTS dalam kontrak perkuliahan; cakupan ujian sebenarnya tetap mengikuti dosen.

## Peta besar: satu data menempuh jaringan

```text
Aplikasi → transport → IP → frame link → sinyal pada media
                                           ↓ tiap hop
Sinyal → frame diperiksa → paket diteruskan router → ... → penerima
```

Empat pertanyaan pokok: **apa** yang dikirim (aplikasi dan representasi), **bagaimana** bit melewati satu media (fisik dan link), **ke mana** paket diteruskan (jaringan dan routing), dan **bagaimana** data yang hilang/terlambat ditangani (protokol ujung dan pengelolaan trafik).

| Ukuran | Arti | Jangan tertukar dengan |
|---|---|---|
| Bit rate (b/s) | Banyak bit per detik | Baud, banyak simbol per detik |
| Bandwidth (Hz) | Rentang frekuensi kanal/sinyal | Kapasitas data (b/s) |
| Throughput | Laju data yang benar-benar sampai | Laju nominal media |
| Delay transmisi | Waktu memasukkan semua bit ke link | Delay propagasi, waktu rambat sinyal |
| Jitter | Perubahan delay antarbagian data | Delay rata-rata |
| BER | Bagian bit yang salah | Peluang paket hilang |

## Bab 1 — Jaringan komunikasi dan layanannya

### 1.1 Evolusi arsitektur dan layanan

**Aplikasi** ialah kegiatan pengguna (web, surel, panggilan); **layanan** ialah kemampuan komunikasi yang dipakai aplikasi; **jaringan** ialah perangkat dan sambungannya; **arsitektur** ialah pembagian fungsi dan aturan kerja. Switch memilih sambungan/keluaran, multiplexer menggabungkan beberapa aliran pada satu jalur, dan router meneruskan paket antarnetwork. *Routing* menghitung/memilih lintasan; *forwarding* menjalankan keputusan itu untuk satu paket.

| Rancangan | Mekanisme | Kekuatan | Masalah |
|---|---|---|---|
| Telegraf/*message switching* | Terima pesan utuh → simpan → baca tujuan → teruskan | Jalur tidak dipesan terus | Pesan panjang menahan pesan lain |
| Telepon klasik/*circuit switching* | Siapkan sirkuit → bawa suara → lepas sirkuit | Kapasitas dan delay lebih teratur sesudah setup | Kapasitas menganggur saat diam; panggilan baru bisa terblokir |
| Internet/*packet switching* | Bagi pesan → tiap paket membawa kendali → antre dan teruskan → susun kembali | Pemakaian bersama luwes untuk trafik yang berubah | Delay berubah, paket bisa hilang/terbalik |

**1.1.1 Telegraf.** Huruf dikodekan menjadi lambang seperti titik dan garis Morse. Stasiun harus mengenali batas pesan (*framing*), alamat tujuan, dan stasiun berikutnya. Dalam *store-and-forward*, stasiun menunggu pesan lengkap sebelum meneruskan. Contoh historis Baudot memakai kode karakter lima bit dan pembagian waktu antarsumber.

**1.1.2 Telepon.** Pelanggan masuk lewat *local loop* ke sentral lokal, lalu sentral terhubung lewat *trunk*. Urutan panggilan: angkat → nada siap → nomor → pilih rute dan sediakan jalur → tujuan berdering/menjawab → suara dua arah → tutup dan lepas jalur. *Signaling* mengatur panggilan; jalur pembawa membawa suara. Tanpa sentral, `N` pelanggan memerlukan `N(N−1)/2` sambungan langsung untuk setiap pasangan. Suara dapat didigitalkan dengan PCM dan digabung melalui TDM; sirkuit telepon tidak berarti semua segmennya analog. Pada seluler, *handoff* mempertahankan hubungan saat berpindah sel.

**1.1.3 Jaringan komputer dan Internet.** Terminal awal berbagi host; *polling* memberi giliran pada terminal dalam jalur bersama. ARPANET memperkenalkan pertukaran paket antarkomputer; pesan dipecah (*segmentation*), paket diteruskan menurut tabel rute, dan data disusun lagi (*reassembly*). LAN menghubungkan perangkat setempat; Ethernet koaksial lama memakai media bersama dan CSMA/CD. Internet menghubungkan jaringan berbeda dengan IP. IP adalah datagram *best effort*: tidak menjamin tiba, urutan, atau batas waktu. TCP dapat memberi aliran byte andal di ujung; UDP menyediakan datagram sederhana. DNS mencari alamat IP dari nama. Alamat MAC berlaku di link lokal, alamat IP untuk pengantaran antarnetwork, dan port memilih proses/aplikasi.

Fungsi yang menyusun arsitektur: transmisi digital; framing/deteksi galat; akses media; pengalamatan; forwarding; routing; kendali kemacetan; penghubung antarjaringan; segmentasi, penyusunan, dan pemulihan ujung; serta aplikasi. **Error control** menangani data rusak/hilang, **flow control** menyesuaikan kapasitas penerima, sedangkan **congestion control** menyesuaikan beban jaringan di tengah.

### 1.2 Arsitektur multiservis

Pesan/berkas mengutamakan kelengkapan; suara interaktif mengutamakan delay dan jitter rendah; video juga butuh laju cukup. Jaringan paket dapat melayani ketiganya, tetapi antrean dan kehilangan perlu dikelola. Satu paket suara mengalami waktu pengumpulan menjadi paket, antrean, transmisi, dan propagasi. **QoS** menyatakan sasaran laju, delay, jitter, atau kehilangan; prioritas dan pengaturan antrean membantu, tetapi tidak menambah kapasitas fisik. ATM adalah contoh historis jaringan paket dengan jalur virtual. Overlay/P2P ialah hubungan logis di atas jaringan yang sudah ada. Keamanan memisahkan autentikasi (identitas), integritas (isi tidak berubah), dan kerahasiaan (isi tidak terbaca pihak lain).

### 1.3 Faktor perkembangan

Teknologi menentukan kemampuan transmisi, penyimpanan, dan pemrosesan; regulasi mengatur penyelenggaraan dan spektrum; pasar menentukan kebutuhan dan biaya yang diterima; standar memungkinkan interoperabilitas. *Efek jaringan*: layanan lebih berguna ketika lebih banyak orang dapat dihubungi. `N(N−1)/2` menghitung pasangan potensial, bukan nilai ekonomi pasti. Skala produksi dapat menurunkan biaya rata-rata; adopsi sering tumbuh pelan, cepat setelah massa kritis, lalu melambat. Standar **de jure** disepakati formal, **de facto** menjadi kebiasaan luas. IETF/RFC, ITU, IEEE 802, dan W3C menangani bidang berbeda. Topologi yang perlu dikenali: bus, bintang, ring, pohon, dan mesh; pilihan dipengaruhi biaya, kapasitas, serta ketahanan.

## Bab 2 — Aplikasi dan arsitektur berlapis

### 2.1 Protokol, layanan, dan lapisan

**Lapisan** membagi pekerjaan; **layanan** adalah kemampuan yang diberikan lapisan bawah kepada atas; **protokol** adalah aturan pertukaran antarentitas setingkat. Ketika membuka situs pada contoh klasik: DNS mencari IP → TCP membentuk hubungan → HTTP meminta halaman → IP mengantar paket → Ethernet/Wi-Fi mengantar frame tiap link → sinyal melintasi media. SMTP adalah aturan pengiriman surel. HTTP/3 modern dapat memakai QUIC di atas UDP; contoh TCP/HTTP di sini ialah alur klasik buku. TCP memberi byte stream berurutan dan andal; UDP tidak memberi jaminan itu sendiri. Koneksi TCP adalah hubungan logis, bukan sirkuit fisik yang dipesan.

### 2.2 Model acuan OSI

| No. | Lapisan | Tugas dan contoh |
|---|---|---|
| 7 | Application | Fungsi aplikasi, misalnya HTTP/DNS |
| 6 | Presentation | Representasi/format data |
| 5 | Session | Pengelolaan dialog |
| 4 | Transport | Antarproses, port, dan bila perlu keandalan |
| 3 | Network | Alamat dan rute antarnetwork |
| 2 | Data Link | Frame pada satu link, pemeriksaan galat |
| 1 | Physical | Bit sebagai sinyal |

OSI ialah **model acuan**, bukan perintah bahwa Internet memakai tujuh protokol terpisah. Protokol bekerja secara logis sejajar antarujung pada lapisan sama; data nyata turun melalui lapisan pengirim dan naik di penerima. Router biasa membaca informasi jaringan untuk forwarding dan memakai link/fisik pada tiap port.

### 2.3 TCP/IP dan pembungkusan

Peta sederhana: **aplikasi** (HTTP/DNS/SMTP), **transport** (TCP/UDP), **Internet** (IP/ICMP), **akses jaringan** (Ethernet/Wi-Fi/fisik). Ada buku yang memisahkan fisik menjadi lapisan kelima. Pembungkusan: `data → [TCP|data] → [IP|TCP|data] → [link|IP|TCP|data|FCS] → sinyal`. Penerima membukanya sebaliknya. Router melepas frame link lama dan membuat frame link baru; paket IP diteruskan. *PDU* adalah unit sebuah lapisan; *SDU* ialah data dari lapisan atas yang menjadi muatannya. Segmentasi membagi data besar; multiplexing port membedakan banyak aplikasi pada satu host.

### 2.4 Berkeley sockets

Server TCP: `socket → bind → listen → accept → read/write → close`. Klien: `socket → connect → read/write → close`. UDP dapat memakai `sendto/recvfrom` tanpa `listen/accept`. `accept` membuat socket hubungan baru. Fungsi pencarian nama seperti `getaddrinfo` mencari alamat; `htons` mengatur urutan byte bilangan port, bukan mencari IP.

### 2.5 Aplikasi dan alat pengamatan

Telnet memberi terminal jarak jauh klasik tanpa enkripsi; FTP klasik memakai hubungan kendali dan data terpisah; HTTP mengatur permintaan/respons web, sedangkan URL mengidentifikasi sumber daya. `ping` memakai ICMP Echo; tidak dibalas tidak otomatis berarti host mati. `traceroute` mencoba melihat hop. `tcpdump` menampilkan lalu lintas yang terlihat pada satu titik, bukan seluruh jaringan.

## Bab 3 — Dasar transmisi digital

### 3.1–3.2 Informasi dan komunikasi digital

Bit memperoleh arti dari formatnya: pola sama dapat berarti karakter atau bilangan. **Blok** punya batas (berkas/pesan); **aliran** dihasilkan terus-menerus (suara/video) dan memerlukan laju cukup serta buffer untuk jitter. Kompresi *lossless* memungkinkan pemulihan persis; *lossy* menghilangkan rincian. Analog berubah kontinu; digital memutuskan simbol dari sinyal terima, lalu regenerator dapat membentuk pulsa lagi. Penguat analog juga memperkuat derau. Digital tetap bisa salah bila derau/distorsi melampaui kemampuan keputusan penerima. Sinkronisasi waktu penting untuk memilih saat membaca simbol.

### 3.3 Suara analog menjadi bit

Alur: filter pembatas frekuensi → **sampling** → **kuantisasi** amplitudo → kode biner (PCM) → transmisi → rekonstruksi perkiraan di tujuan. Untuk sinyal terbatas hingga `B` Hz, laju sampling ideal harus **lebih dari `2B`**; filter antialias diperlukan. Laju PCM mentah `f_s × b` bit/detik bila `b` bit per sampel. Contoh `8.000 × 8 = 64 kb/s`. Kuantisasi membulatkan nilai, sehingga menambah derau kuantisasi; untuk sinus skala penuh dengan kuantisator seragam ideal, `SNR_q ≈ 6,02b + 1,76 dB` (bukan aturan universal).

### 3.4–3.5 Kanal dan batas transmisi

Kanal mengalami **redaman** (sinyal melemah), **distorsi** (komponen frekuensi/timing berubah), **derau** (gangguan tambahan), dan **ISI** (pulsa bertumpuk). `SNR = S/N`, sedangkan `SNR_dB = 10 log10(S/N)` untuk rasio daya. `T_transmisi = L/R` untuk `L` bit dan laju `R` b/s; `T_propagasi = d/v` untuk jarak `d` dan laju rambat `v`. Keduanya harus dibedakan dari antrean dan pemrosesan.

| Batas | Rumus | Asumsi/kegunaan |
|---|---|---|
| Nyquist | `R_b ≤ 2B log2(M)` | Kanal ideal tanpa derau, `M` tingkat simbol |
| Shannon | `C = B log2(1+S/N)` | Batas kapasitas model kanal berderau |

`B` dalam Hz; `S/N` pada Shannon adalah rasio **linear**, bukan angka dB. Baud adalah simbol/detik; bit mentah/s = baud × `log2(M)` bila simbol memuat `M` pilihan setara. Contoh 16-QAM, 1.000 baud → 4.000 bit mentah/detik sebelum overhead. Kedua batas tidak dijumlahkan.

### 3.6–3.8 Membentuk sinyal dan memilih media

**Line coding** memetakan bit ke pulsa: NRZ sederhana tetapi rentetan bit sama menyulitkan sinkronisasi; Manchester memberi transisi di tengah bit tetapi membutuhkan bandwidth lebih; AMI mengganti polaritas bit 1 dan mengurangi komponen DC. *Scrambling* membantu mengatasi rentetan tanpa transisi. **Modulasi** mengubah pembawa: ASK mengubah amplitudo, FSK frekuensi, PSK fase, QAM amplitudo dan fase. BPSK membawa satu bit/simbol; 16-QAM empat bit mentah/simbol dengan kebutuhan SNR lebih tinggi ketika titik makin rapat.

| Media | Sifat yang perlu diingat |
|---|---|
| Twisted pair | Murah; pilinan mengurangi gangguan/crosstalk; redaman naik dengan jarak/frekuensi |
| Koaksial | Konduktor pusat dan pelindung; pernah dipakai pada media bersama |
| Serat optik | Kapasitas besar, redaman rendah; single-mode/multimode berbeda dispersi |
| Radio | Mobilitas, tetapi rentan halangan, interferensi, dan multipath |
| Inframerah | Lazim jarak dekat; halangan/dinding membatasi |

### 3.9 Kesalahan bit

**Parity genap** menambah bit agar jumlah 1 genap; ia mendeteksi satu bit salah tetapi bisa melewatkan dua bit salah. Parity dua dimensi dapat menunjukkan posisi satu bit salah lewat perpotongan baris-kolom. **Checksum** menjumlahkan kata data dengan aturan tertentu; perubahan bisa saling meniadakan. **CRC** menganggap bit sebagai polinomial biner, membagi dengan generator memakai XOR, lalu mengirim sisanya; penerima membagi ulang dan memeriksa sisa. Generator derajat `r` menambah `r` bit dan mendeteksi semua burst sepanjang paling banyak `r` bit pada asumsi biasa. Parameter CRC (polinomial, urutan bit, nilai awal) harus sama. **FEC** menambah redundansi untuk memperbaiki tanpa kirim ulang; kode berjarak minimum `d_min` mendeteksi hingga `d_min−1` dan memperbaiki hingga `floor((d_min−1)/2)` kesalahan per kata kode.

## Bab 4 — Jaringan pensaklaran sirkuit

### 4.1 Berbagi satu jalur: multiplexing

**FDM** memberi pita frekuensi berbeda sekaligus, dengan *guard band*. **TDM** memberi slot waktu berulang; TDM sinkron tetap menyediakan slot saat sumber diam, TDM statistik memberi slot menurut kebutuhan dan memerlukan buffer. **WDM** memberi panjang gelombang optik berbeda pada satu serat. Multiplexing membagi media; switching memilih sambungan/keluaran.

### 4.2–4.3 SONET dan transport

SONET menyamakan frame dan laju transport optik sinkron. STS-1/OC-1 = **51,84 Mb/s**; frame STS-1 `9×90` byte dikirim **8.000 kali/detik**, sehingga `9×90×8×8.000 = 51.840.000 b/s` termasuk overhead. STS-3 nominal 155,52 Mb/s. *Pointer* membantu pergeseran posisi muatan. Jaringan transport menggabungkan aliran berkapasitas tinggi; ring SONET dapat menyediakan proteksi ketika ruas gagal (contoh UPSR/BLSR). Transport optik memakai WDM dan perangkat *add/drop* untuk mengambil/menambah panjang gelombang tertentu.

### 4.4–4.6 Switch, telepon, dan signaling

**Space-division switch** menghubungkan jalur fisik masukan-keluaran; crossbar `N×M` secara konsep punya hingga `N×M` titik silang. **Time-division switch** memindahkan sampel dari slot masuk ke slot keluar lewat memori. *Blocking* berarti panggilan baru tidak mendapat kapasitas/lintasan. PSTN historis: pelanggan → local loop → sentral lokal → trunk → sentral tujuan. Suara bisa digital di tengah; ISDN secara historis menyediakan kanal B 64 kb/s untuk isi dan kanal D untuk kendali. *Signaling* menyiapkan, memantau, dan melepas panggilan; dapat in-band atau out-of-band. Dalam SS7, SSP menangani layanan, STP meneruskan pesan signaling, SCP menyimpan logika/data layanan; jalur pesan kendali tidak harus sama dengan jalur suara.

### 4.7–4.8 Trafik dan seluler

Trafik **erlang** ialah rata-rata jumlah sirkuit yang sedang dipakai: `jumlah panggilan × lama rata-rata / periode pengamatan` dengan satuan waktu konsisten. Contoh 30 panggilan/jam × 2 menit = **1 erlang**, bukan satu panggilan. **Konsentrasi** membagi trunk karena tidak semua pelanggan menelepon bersamaan; rute alternatif membantu jika masih tersedia kapasitas; kendali overload membatasi permintaan baru. Pada jaringan seluler, wilayah dibagi sel, frekuensi dapat dipakai ulang di sel cukup jauh, *handoff* memindahkan hubungan antarsel, sedangkan *roaming* berarti layanan di wilayah/jaringan lain. Banyak jaringan seluler kini memakai suara berbasis paket; pembahasan sirkuit adalah konteks historis buku.

## Bab 5 — Protokol antartitik dan data link

### 5.1 Model layanan

Protokol antartitik mengatur frame, nomor urut, ACK, timer, dan respons pada satu hubungan. Layanan bisa tanpa/berorientasi koneksi dan tanpa/dengan pengakuan. **Per-hop** memperbaiki satu link; **end-to-end** memeriksa hingga penerima akhir. Keberhasilan satu hop belum membuktikan seluruh rute berhasil. TCP memberi aliran byte andal tetapi **tidak mempertahankan batas pesan** aplikasi; datagram mempertahankan batas unitnya tetapi belum tentu andal.

### 5.2 ARQ: kirim ulang otomatis

ARQ memakai deteksi galat + nomor urut + ACK/NAK + timer. Jika ACK hilang, pengirim dapat mengulang frame yang sudah diterima; penerima mengenali nomor urutnya dan menolak penyerahan duplikat. NAK membantu, tetapi timer tetap diperlukan karena ACK/NAK juga bisa hilang.

| Metode | Frame belum diakui | Bila frame 2 hilang tetapi 3 dan 4 tiba | Konsekuensi |
|---|---|---|---|
| Stop-and-Wait | Satu | 2 diulang setelah timer | Sederhana; link jauh banyak menganggur |
| Go-Back-N | Hingga jendela `W` | 3–4 dibuang; ulang 2, 3, 4 | ACK kumulatif, buffer penerima kecil |
| Selective Repeat | Hingga jendela `W` | 3–4 disimpan; ulang hanya 2 | Lebih efisien pada galat, buffer/ACK lebih rumit |

Efisiensi Stop-and-Wait tanpa galat kira-kira `T_frame/(T_frame + 2T_prop + T_ACK)` bila pemrosesan diabaikan. Untuk `m` bit nomor urut, batas jendela umum Go-Back-N `W ≤ 2^m−1`; Selective Repeat dengan jendela pengirim/penerima sama `W ≤ 2^(m−1)`. *Sliding window* juga dapat dipakai untuk flow control, bukan hanya pengiriman ulang.

### 5.3 Kendali lain

**Flow control** mencegah buffer penerima penuh; **error control** memulihkan kerusakan/hilang; **congestion control** mengurangi beban jaringan. Aliran suara perlu pemulihan waktu: data yang tiba lengkap tetapi terlambat dapat tak berguna. Pada TCP, nomor urut menghitung **byte** dan ACK menyatakan byte berurutan berikutnya yang diharapkan; jendela penerima mengatur flow control.

### 5.4–5.6 Framing, PPP, HDLC

Framing menentukan batas frame melalui panjang eksplisit, byte pembatas dan *byte stuffing*, atau pola bit dan *bit stuffing*. HDLC memakai flag `01111110`; setelah lima bit 1 berturut-turut dalam data, pengirim menyisipkan 0 agar flag tidak muncul tanpa sengaja. Penerima membuang 0 sisipan. **PPP** membungkus paket pada hubungan dua ujung; LCP mengatur link, NCP mengatur protokol jaringan seperti IP. PPP bukan jaminan ARQ. **HDLC** memakai `flag | alamat | kendali | data | FCS`; frame I membawa data, frame S pengawasan/ACK, frame U kendali hubungan. FCS/CRC memeriksa galat, sedangkan framing menemukan batas frame.

### 5.7 Multiplexing paket

Pada pembagian statistik, sumber aktif memakai kapasitas ketika sumber lain diam; saat total arus melebihi link, buffer dan antrean bertambah. *Speech interpolation* mengirim suara terutama ketika ada aktivitas bicara, tetapi perlu menangani jitter dan lonjakan saat banyak orang bicara serentak.

## Bab 6 — Kendali akses media dan LAN

### 6.1–6.3 Siapa boleh mengirim?

Media bersama memerlukan MAC (*medium access control*). **Akses acak** membiarkan stasiun mencoba, lalu mengulang setelah jeda acak jika tabrakan. **Akses terjadwal** mengoordinasikan giliran dan membayar overhead kendali. Alamat MAC ialah alamat antarmuka link, berbeda dari nama sublapisan MAC.

| Metode | Mekanisme | Hal yang diuji |
|---|---|---|
| Pure ALOHA | Mulai kapan saja | Periode rawan `2T`; `S=G e^(−2G)`, puncak `1/(2e)≈18,4%` |
| Slotted ALOHA | Mulai di batas slot | Periode rawan `T`; `S=G e^(−G)`, puncak `1/e≈36,8%` |
| CSMA | Dengar kanal sebelum kirim | Tetap bisa tabrakan karena delay rambat |
| CSMA/CD | Dengar juga saat mengirim; hentikan dan backoff saat tabrakan | Ethernet media bersama historis |
| Reservasi | Minta giliran/slot lebih dulu | Perlu tahap reservasi |
| Polling | Pengendali memanggil stasiun satu-satu | Pengendali bisa jadi titik gagal |
| Token passing | Hanya pemegang token boleh kirim | Perlu memulihkan token hilang/ganda |

Puncak ALOHA itu hasil **model ideal**, bukan throughput Wi-Fi/Ethernet. CSMA/CD membutuhkan frame minimum cukup lama untuk mendeteksi tabrakan sepanjang waktu rambat pulang-pergi. Pada Ethernet switch full-duplex, tidak ada media tabrakan bersama. Akses acak sering ringan saat trafik jarang; terjadwal dapat lebih stabil saat trafik tinggi.

### 6.4–6.5 Pembagian kanal dan delay

**FDMA** membagi frekuensi, **TDMA** membagi waktu, **CDMA** membedakan pengguna lewat kode. CDMA tetap mengalami interferensi dan membutuhkan kendali daya untuk masalah *near-far*. Pembagian tetap dapat menyia-nyiakan jatah pengguna diam; pembagian statistik menggunakan jatah kosong tetapi menimbulkan antrean saat banyak pengguna aktif. Delay polling/token memuat waktu menunggu giliran, pesan kendali, dan transmisi; delay akses acak naik karena tabrakan/ulang pada beban tinggi.

### 6.6–6.9 LAN dan Ethernet

LAN mencakup area terbatas; IEEE 802 memisahkan fungsi MAC dan LLC. **Ethernet lama** memakai bus/hub dan CSMA/CD; **Ethernet switch** belajar alamat MAC **sumber** pada port masuk, meneruskan ke port tujuan yang diketahui, dan membanjiri port lain dalam VLAN ketika tujuan belum dikenal. Frame Ethernet memuat preamble, MAC tujuan/sumber (masing-masing 6 byte), tipe/panjang, muatan, dan FCS. Muatan standar umumnya 46–1500 byte; frame klasik minimal 64 byte dari MAC tujuan hingga FCS, tanpa preamble. Fast Ethernet 100 Mb/s, Gigabit 1 Gb/s, 10 Gigabit 10 Gb/s adalah laju nominal, bukan throughput aplikasi. **Token Ring** IEEE 802.5 memberi token logis sebagai giliran; **FDDI** memakai ring serat dan ring cadangan untuk ketahanan. Keduanya terutama penting sebagai rancangan historis.

### 6.10 WLAN dan 6.11 bridge/switch

Wi-Fi IEEE 802.11 memakai **CSMA/CA**: dengar kanal → tunggu/backoff → kirim → tunggu ACK link; RTS/CTS opsional membantu keadaan *hidden terminal* (A dan C tak saling dengar tetapi sama-sama menuju B). Radio sulit mendeteksi tabrakan sambil memancar. Mode **infrastruktur** memakai AP; **ad hoc** antarklien tanpa AP. Frame Wi-Fi dapat memuat beberapa alamat karena pengirim/penerima radio bisa berbeda dari sumber/tujuan akhir. Laju fisik bukan throughput aplikasi.

Bridge/switch bekerja dengan alamat MAC pada link, router dengan alamat IP antarnetwork. **Transparent bridge** belajar sumber dan memakai spanning tree agar topologi aktif tidak membentuk loop; *source-routing bridge* membawa informasi lintasan dari pengirim. **VLAN** memisahkan domain link/broadcast secara logis; komunikasi antar-VLAN membutuhkan routing. Tag 802.1Q membawa identitas VLAN pada link yang sesuai.

## Bab 7 — Jaringan pensaklaran paket

### 7.1–7.2 Layanan dan topologi

Paket diterima router → header diperiksa → keluaran dipilih → paket menunggu bila perlu → dikirim. Delay per hop = **pemrosesan + antrean + transmisi + propagasi**. Antrean berubah menurut beban; buffer penuh menyebabkan *drop*. Topologi bintang, ring, dan mesh berbeda dalam biaya dan jalur cadangan. Link putus dapat dialihkan jika ada rute lain dan tabel routing sudah diperbarui. **Forwarding** memakai tabel yang ada untuk paket saat ini; **routing** membangun/memperbarui tabel.

### 7.3 Datagram, virtual circuit, dan switch

| Sifat | Datagram | Virtual circuit (VC) |
|---|---|---|
| Sebelum kirim | Tidak wajib setup rute | Setup memasang keadaan/label di node |
| Keputusan | Per paket menurut alamat tujuan | Menurut label VC pada link itu |
| Urutan | Paket dapat lewat rute berbeda | Biasanya satu jalur selama VC hidup |
| Bila jalur gagal | Paket berikut dapat dirutekan ulang setelah konvergensi | VC mungkin perlu dibangun/dialihkan |

VC tetap **jaringan paket**, bukan sirkuit telepon fisik. Tahap VC: setup → transfer → teardown. Node dapat menukar `(port masuk, label masuk)` menjadi `(port keluar, label keluar)`. Switch paket punya port masuk, *switching fabric*, port keluar, dan buffer. *Head-of-line blocking* terjadi bila paket depan di antrian input menghalangi paket belakang yang sebenarnya bisa menuju keluaran lain.

### 7.4–7.5 Routing

Rute dapat dinilai menurut hop, delay, biaya, kapasitas, atau kebijakan; “terpendek” memerlukan bobot yang jelas. Routing **statis** jarang berubah, **dinamis** merespons keadaan. Routing hierarkis merangkum wilayah agar tabel tidak terlalu besar. Tabel memetakan tujuan/prefix ke *next hop*; IP memakai pencocokan prefix terpanjang.

**Bellman–Ford/distance vector:** node memperbarui jarak lewat tetangga dengan `D_x(y)=min_v{c(x,v)+D_v(y)}`. Informasi terlambat dan kegagalan dapat menimbulkan *count-to-infinity*. **Dijkstra/link state:** setiap node memperoleh keadaan link, mulai jarak sumber 0, memilih node sementara termurah, lalu merelaksasi tetangga; syarat bobot nonnegatif untuk versi standar. **Source routing** menaruh lintasan dari awal; **hop-by-hop** memilih tetangga berikut di setiap router. *Convergence* ialah proses mencapai tabel stabil setelah perubahan.

### 7.6 ATM

ATM memakai VC dan *cell* tetap **53 byte = 5 byte header + 48 byte muatan**. Jika 48 byte muatan penuh, efisiensi bagian cell `48/53≈90,6%`, belum termasuk overhead lain. Unit kecil membatasi waktu tunggu di belakang data besar, tetapi cell yang tak penuh membuang kapasitas.

### 7.7–7.9 Mengelola trafik

**FIFO** melayani urutan kedatangan; **priority queue** mendahulukan kelas tertentu tetapi dapat membuat kelas rendah kelaparan. **Fair queueing** membagi layanan antararus; **weighted fair queueing** membagi menurut bobot (bobot 2:1 → sasaran bagian sekitar 2:1 saat keduanya aktif). **RED** mulai menandai/membuang sebagian paket sebelum buffer penuh berdasarkan rata-rata antrean, dengan harapan sumber mengurangi laju. Penjadwalan tidak menambah kapasitas link.

Pada tingkat **aliran**, *admission control* menolak aliran baru ketika sumber daya tak cukup. **Open-loop** mengatur tanpa umpan balik sesaat: *leaky bucket* meratakan laju, *token bucket* mengizinkan lonjakan sampai token tersedia sambil membatasi laju rata-rata. **Closed-loop** memakai sinyal kemacetan/kehilangan sebagai umpan balik agar pengirim menyesuaikan laju. **Agregasi** mengelompokkan banyak aliran ke kelas agar router tidak menyimpan keadaan terpisah untuk semuanya; pengawasan di tepi mencegah satu aliran menyalahgunakan kelasnya.

## Rumus dan keputusan cepat

| Kebutuhan | Pakai | Contoh |
|---|---|---|
| Lama mengirim `L` bit | `L/R` | 1.500 byte / 12 Mb/s = 1 ms |
| Lama rambat | `d/v` | 100 km / `2×10^8` m/s = 0,5 ms |
| Laju PCM | sampel/detik × bit/sampel | 8.000 × 8 = 64 kb/s |
| Bit mentah modulasi | baud × `log2(M)` | 2.000 baud, 8 simbol = 6 kb/s |
| Sampel minimum ideal | lebih dari `2B` | sinyal terbatas 4 kHz → lebih dari 8 ksampel/s menurut formulasi ideal |
| Laju ideal Nyquist | `2B log2(M)` | 2 kHz, 4 tingkat → 8 kb/s |
| Kapasitas Shannon | `B log2(1+S/N)` | 2 kHz, S/N=3 → 4 kb/s |
| Trafik erlang | total lama pemakaian / lama pengamatan | 30 × 2 menit / 60 menit = 1 erlang |

**Catatan sampling:** contoh suara PCM 8.000 sampel/detik memakai sinyal suara yang dibatasi **di bawah** 4 kHz dan rancangan filter yang sesuai; jangan menyimpulkan bahwa `8.000 > 2×4.000`.

## Pasangan istilah yang sering tertukar

| Pasangan | Pembeda paling singkat |
|---|---|
| Message / circuit / packet switching | Pesan utuh disimpan; sirkuit disediakan; paket berbagi jalur |
| Circuit switching / virtual circuit | Kapasitas jalur disediakan selama panggilan / jalur logis untuk paket; reservasi VC bergantung teknologi |
| Multiplexing / switching | Menggabungkan aliran pada media / memilih hubungan atau keluaran |
| DNS / HTTP / IP | Cari alamat / minta sumber daya web / antarkan datagram |
| TCP / UDP | Aliran byte andal dan berurutan / datagram tanpa jaminan dari UDP sendiri |
| MAC / IP / port | Tujuan link lokal / antarnetwork / proses pada host |
| CRC / ARQ / FEC | Deteksi galat / kirim ulang / perbaiki dari redundansi |
| Flow control / congestion control | Lindungi penerima / lindungi jaringan tengah |
| FDM / FDMA; TDM / TDMA | Teknik berbagi media / pembagian akses antarpengirim; prinsip frekuensi/waktunya serupa |
| CSMA/CD / CSMA/CA | Deteksi tabrakan pada media kabel bersama lama / kurangi peluang tabrakan di Wi-Fi |
| Router / switch LAN | Pilih next hop IP antarnetwork / teruskan frame menurut MAC dalam LAN |

## Cek pemahaman singkat

1. Mengapa koneksi TCP tidak berarti sirkuit telepon? **Jawab:** hubungan TCP adalah keadaan logis pada ujung di atas paket IP; jalur fisik tidak dipesan end-to-end.
2. ACK sebuah frame hilang. Apa yang mencegah data diserahkan dua kali? **Jawab:** nomor urut membuat penerima mengenali frame ulangan, lalu mengakui lagi tanpa menyerahkannya ulang.
3. Paket 1.000 byte melewati link 1 Mb/s sejauh 100 km dengan `v=2×10^8 m/s`. Berapa dua delay dasarnya? **Jawab:** transmisi 8 ms, propagasi 0,5 ms; tambah antrean/pemrosesan bila diketahui.
4. Mengapa Wi-Fi masih bisa tabrakan walau mendengar kanal? **Jawab:** stasiun tersembunyi mungkin tidak saling mendengar, dan dua stasiun bisa memulai pada waktu hampir sama.
5. Mengapa Dijkstra tidak sekadar menghitung jumlah hop? **Jawab:** ia menjumlahkan bobot link yang ditetapkan; bobot bisa berupa biaya atau delay.

## Sumber dan batas cakupan

Rangkuman ini disusun dari tujuh catatan bab dalam repositori ini, yang merujuk buku Leon-Garcia dan Widjaja edisi kedua, kontrak KOM1314, dan slide kuliah yang tersedia. Bab 1, 2, 3, 5, dan 6 memiliki rujukan slide dalam paket materi; Bab 4 dan 7 tidak memiliki slide khusus di paket tersebut. Istilah dan teknologi historis diberi konteks sesuai masa buku. Ini catatan belajar, bukan kisi-kisi resmi atau pengganti pembahasan rinci pada setiap bab.
