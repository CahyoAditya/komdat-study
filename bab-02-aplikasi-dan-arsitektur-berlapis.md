# Bab 2 — Aplikasi dan Arsitektur Berlapis

> Pertanyaan utama: ketika sebuah situs terbuka, bagian jaringan mana yang mengerjakan tiap langkah?

Susunan 2.1–2.5 dan nomor subbagiannya mengikuti daftar isi Bab 2 buku Leon-Garcia dan Widjaja, edisi kedua. Bagian 2.4 dan 2.5 bertanda topik pilihan dalam daftar isi buku; tetap dijelaskan secara ringkas agar setiap subbab punya tempat. Penjelasan memakai bahasa sendiri dan slide kuliah, bukan salinan buku.

## 2.1 Contoh Protokol, Layanan, dan Lapisan (*Examples of Protocols, Services, and Layering*)

| Istilah | Arti sederhana | Contoh |
|---|---|---|
| **Lapisan (*layer*)** | Kelompok pekerjaan yang sejenis | Lapisan transport menangani komunikasi antarproses. |
| **Layanan (*service*)** | Kemampuan yang diberikan sebuah lapisan kepada lapisan di atasnya | TCP menyediakan aliran byte yang andal bagi aplikasi. |
| **Protokol (*protocol*)** | Aturan pertukaran pesan antara pihak setingkat | TCP mengatur nomor urut, pengakuan penerimaan, dan pengiriman ulang. |

Bayangkan kantor pos. Bagian loket menyediakan **layanan** menerima kiriman. Dua kantor pos memakai **aturan** bersama tentang label dan penanganan kiriman: itulah analogi protokol. Loket dan pengangkutan adalah **lapisan tugas** yang berbeda. Analogi ini membantu mengingat pembagian kerja, tetapi paket jaringan tidak persis sama dengan paket pos.

*Protokol* menentukan bentuk pesan, kapan pesan dikirim, serta tindakan saat pesan diterima atau waktunya habis. Contoh: HTTP mengenal permintaan dan respons; TCP mempunyai aturan untuk mendeteksi data yang belum diterima dan mengirimnya kembali.

#### Mengapa tugas dibagi menjadi lapisan?

Masalah besar menjadi beberapa masalah kecil. Pengembang peramban dapat memakai layanan transport tanpa merancang ulang kabel; teknologi Wi-Fi dapat berubah tanpa mengubah arti permintaan HTTP. Pembagian ini membantu desain, pengujian, dan penggantian bagian. Tetap ada biaya tambahan, seperti informasi kendali pada tiap lapisan.

### 2.1.1 HTTP, DNS, dan SMTP

Sensei mengetik nama situs dan menekan Enter. Agar halaman tampil, beberapa pekerjaan berbeda perlu selesai:

1. **DNS** mencari alamat IP yang sesuai dengan nama situs. Ini seperti mencari nomor tujuan dari nama kontak.
2. Peramban menyiapkan komunikasi dengan server. Pada contoh **HTTP melalui TCP**, TCP mengurus aliran data antaraplikasi dan pengiriman ulang ketika diperlukan.
3. **HTTP** menyatakan halaman apa yang diminta dan bagaimana server menjawab. Misalnya, permintaan `GET /` dapat dibalas dengan isi halaman.
4. **IP** membawa paket menuju jaringan tujuan melalui perangkat perantara.
5. Teknologi jaringan setempat, seperti Ethernet atau Wi-Fi, memindahkan data ke tetangga berikutnya; media fisik membawa sinyalnya.

```mermaid
flowchart LR
  A[Ketuk tautan] --> B[DNS mencari alamat IP]
  B --> C[TCP membentuk hubungan]
  C --> D[HTTP menyiapkan permintaan]
  D --> E[TCP dan IP membawa data]
  E --> F[Wi-Fi atau Ethernet mengirim lewat satu jalur]
  F --> G[Server menjawab; proses berjalan balik]
```

Ini **contoh alur dari slide**, bukan syarat setiap situs masa kini. HTTPS menambahkan perlindungan TLS, dan HTTP/3 dapat memakai QUIC di atas UDP. Pokok pelajarannya tetap sama: satu tindakan pengguna memerlukan kerja sama beberapa lapisan.

**Bedakan perannya:** DNS membantu menemukan alamat IP dari nama, HTTP mengatur permintaan dan respons sumber daya web, sedangkan SMTP mengatur pengiriman surel. Ketiganya contoh protokol aplikasi; tidak berarti ketiganya memakai transport yang selalu sama.

### 2.1.2 Layanan lapisan transport TCP dan UDP

**TCP** menyediakan aliran byte yang andal dan berurutan antaraplikasi setelah hubungan logis dibentuk. **UDP** mengirim datagram tanpa jaminan keandalan atau urutan dari UDP sendiri. Keduanya menggunakan nomor port untuk membedakan aplikasi. HTTP klasik pada contoh slide memakai TCP; DNS sering memakai UDP untuk pertanyaan singkat, tetapi juga dapat memakai TCP.

| Sifat | Berorientasi koneksi | Tanpa koneksi |
|---|---|---|
| Sebelum data utama | Ada tahap membentuk hubungan logis | Tidak perlu tahap pembentukan hubungan pada protokol itu |
| Contoh | TCP | UDP, IP |
| Hal yang harus diingat | “Koneksi” TCP bukan sirkuit fisik yang dipesan | “Tanpa koneksi” tidak berarti mustahil membangun keandalan pada lapisan lain |

Jenis layanan di dua lapisan tidak harus sama. TCP yang berorientasi koneksi dapat bekerja di atas IP yang tanpa koneksi.

Contoh pilihan: jika berkas harus diterima lengkap, layanan aliran andal memudahkan aplikasi. Jika aplikasi ingin mengatur sendiri cara menangani keterlambatan, layanan datagram dapat lebih sesuai. **UDP bukan sinonim “cepat pasti”;** hasil nyata bergantung pada jaringan dan aplikasinya.

## 2.2 Model Acuan OSI (*The OSI Reference Model*)

### 2.2.1 Tujuh lapisan OSI

Baca tabel **dari atas ke bawah** ketika mengirim, lalu **dari bawah ke atas** ketika menerima.

| No. | Lapisan OSI | Pekerjaan inti | Pertanyaan pengingat |
|---|---|---|---|
| 7 | Application | Menyediakan fungsi komunikasi yang dipakai aplikasi, misalnya web atau DNS | Layanan aplikasi apa yang diminta? |
| 6 | Presentation | Mengatur representasi data agar kedua pihak memahaminya | Bagaimana data diwakili? |
| 5 | Session | Mengelola dialog atau sesi komunikasi | Bagaimana percakapan diatur? |
| 4 | Transport | Mengantar data antaraplikasi di ujung, termasuk port dan, bila dipakai, keandalan | Aplikasi mana di komputer tujuan? |
| 3 | Network | Mengantar paket melewati beberapa jaringan, termasuk alamat dan rute | Komputer tujuan ada di jaringan mana? |
| 2 | Data Link | Mengantar *frame* melalui satu sambungan langsung | Bagaimana mencapai tetangga berikutnya? |
| 1 | Physical | Mengirim bit sebagai sinyal melalui media | Bagaimana 0 dan 1 dikirim? |

```mermaid
flowchart TB
  A[7 Application] --> B[6 Presentation]
  B --> C[5 Session]
  C --> D[4 Transport]
  D --> E[3 Network]
  E --> F[2 Data Link]
  F --> G[1 Physical]
```

**Jebakan ujian:** OSI adalah *model acuan*. Internet sehari-hari terutama menggunakan keluarga protokol TCP/IP, bukan kewajiban menjalankan tujuh protokol OSI secara terpisah. Fungsi presentasi dan sesi sering dikerjakan di tingkat aplikasi atau pustaka yang dipakainya.

#### Ujung dan perangkat perantara

Pada contoh sederhana, aplikasi dan transport berjalan pada perangkat pengirim serta penerima. *Router* di tengah terutama membaca informasi lapisan jaringan untuk meneruskan paket, lalu memakai data link dan fisik pada tiap sambungannya. Tidak perlu menganggap router biasa membaca isi HTTP untuk dapat meneruskan paket.

### 2.2.2 Tampilan bersama: lapisan, protokol, dan layanan

Bayangkan dua komputer A dan B. Protokol mengatur komunikasi *sejajar* antara lapisan yang sama di A dan B; layanan diberikan *vertikal* dari satu lapisan kepada lapisan di atasnya pada satu komputer. Data sungguhan turun melewati lapisan A, melintasi jaringan, lalu naik pada B.

```mermaid
flowchart LR
  AA[Aplikasi A] -. aturan HTTP .-> AB[Aplikasi B]
  AA -->|memakai layanan| TA[Transport A]
  TA -. aturan TCP .-> TB[Transport B]
  TB -->|memberi layanan| AB
```

Contoh: HTTP adalah protokol aplikasi; TCP adalah protokol transport dan menyediakan layanan aliran byte untuk HTTP. OSI membantu memetakan tugas, tetapi bukan daftar yang mewajibkan satu perangkat menjalankan tujuh protokol terpisah.

## 2.3 Gambaran Arsitektur TCP/IP (*Overview of TCP/IP Architecture*)

### 2.3.1 Arsitektur TCP/IP

Slide kuliah menggambarkan aplikasi di atas **TCP atau UDP**, di atas **IP**, di atas berbagai antarmuka jaringan. Untuk belajar, susunannya dapat ditulis begini:

| Susunan TCP/IP sederhana | Contoh | Padanan fungsi pada OSI |
|---|---|---|
| Aplikasi | HTTP, DNS, SMTP | Terutama lapisan 7; sebagian fungsi 5–6 berada di sini |
| Transport | TCP, UDP | Lapisan 4 |
| Internet | IP | Lapisan 3 |
| Akses jaringan | Ethernet, Wi-Fi, media fisik | Lapisan 2 dan 1 |

Beberapa buku memisahkan akses jaringan menjadi *data link* dan *physical*, sehingga TCP/IP digambar sebagai **lima** lapisan. Empat dan lima sama-sama bisa dipakai sebagai penyederhanaan; sebutkan pembagian yang dipakai sebelum membandingkannya.

```mermaid
flowchart TB
  subgraph UjungPengirim[Perangkat pengirim]
    H[HTTP atau DNS] --> T[TCP atau UDP]
    T --> I[IP]
    I --> L[Wi-Fi atau Ethernet]
  end
  L --> R[Router: IP dan sambungan setempat]
  R --> P[Perangkat penerima: naik melalui lapisan]
```

**TCP** memberi aliran byte andal dan memakai sambungan logis. **UDP** memberi datagram tanpa jaminan pengiriman atau urutan dari UDP itu sendiri. **IP** memberi pengiriman paket *best effort* antardevais. Memakai TCP di atas IP tidak mengubah IP menjadi protokol yang menjamin setiap paket sampai; keandalan TCP ditangani pada lapisan transport.

### 2.3.2 Cara lapisan bekerja bersama

Misalkan aplikasi mengirim teks `HALO`. Tiap lapisan menambahkan informasi yang dibutuhkan untuk pekerjaannya. Informasi depan disebut **header**; data link dapat pula menambah **trailer** untuk pemeriksaan kesalahan.

```text
Aplikasi:    HALO
Transport:  [header TCP][HALO]                         → segmen TCP
Internet:   [header IP][header TCP][HALO]              → paket IP
Data link:  [header link][header IP][header TCP][HALO][trailer] → frame
Fisik:      bit-bit dari frame dikirim sebagai sinyal
```

Penerima membukanya dalam urutan sebaliknya. Proses ini disebut **decapsulation**. Di router, frame dari satu sambungan dibuka hingga paket IP, lalu paket itu dibungkus menjadi frame baru untuk sambungan berikutnya. Karena itu, header link dapat berubah sepanjang perjalanan, sedangkan tujuan IP pada contoh biasa tetap menunjuk tujuan akhir.

Jangan hafal nama unit tanpa konteks: istilah umum adalah **PDU** (*protocol data unit*), yakni unit yang dipertukarkan oleh dua pihak pada lapisan yang sama. Data dari lapisan atas yang diserahkan ke lapisan bawah disebut **SDU** (*service data unit*). Bagi lapisan bawah, SDU itu menjadi isi yang dibungkus dalam PDU miliknya.

#### Potong, gabung, dan bagikan

Jika data terlalu besar, lapisan dapat **memecah**nya menjadi beberapa unit dan penerima **menyatukannya kembali**. Jika banyak aplikasi memakai satu komputer, lapisan transport memakai **nomor port** untuk membedakan alirannya. Ini contoh *multiplexing*: banyak pengguna layanan berbagi lapisan di bawahnya. Alamat IP mengidentifikasi antarmuka pada jaringan; port membantu memilih proses atau layanan pada perangkat tersebut.

### 2.3.3 Tinjauan protokol

| Lapisan | Contoh protokol/teknologi | Tugas satu kalimat |
|---|---|---|
| Aplikasi | HTTP, DNS, SMTP | Menyatakan kebutuhan aplikasi. |
| Transport | TCP, UDP | Menghubungkan proses dan membedakan port. |
| Internet | IP, ICMP | Mengantar paket antarnetwork dan melaporkan kondisi tertentu. |
| Akses jaringan | Ethernet, Wi-Fi | Memindahkan frame melalui sambungan setempat. |

**Periksa dengan contoh:** permintaan HTTP masuk ke TCP, dibungkus IP, kemudian dibawa frame jaringan setempat. Router meneruskan paket IP melalui sambungan lain. Nama protokol dan nama lapisan perlu dibedakan.

## 2.4 Berkeley API

*API* adalah cara aplikasi meminta layanan dari sistem operasi. Buku memakai antarmuka *Berkeley sockets* untuk memperlihatkan bagaimana program berkomunikasi melalui jaringan. **Socket** ialah titik komunikasi yang diakses program; nomor port membantu memilih layanan pada sebuah perangkat. Sensei cukup memahami alurnya dahulu, tanpa harus menghafal kode C.

### 2.4.1 Pemanggilan sistem socket (*Socket System Calls*)

Contoh server TCP: `socket()` membuat socket → `bind()` memasang alamat/port lokal → `listen()` menunggu → `accept()` menerima hubungan → `read()`/`write()` bertukar byte → `close()` menutup. Klien TCP memakai `socket()` → `connect()` → `read()`/`write()` → `close()`.

```mermaid
sequenceDiagram
  participant K as Klien
  participant S as Server
  S->>S: socket, bind, listen
  K->>K: socket
  K->>S: connect
  S->>S: accept
  K->>S: write permintaan
  S->>K: write jawaban
```

Pada UDP, program dapat memakai `sendto()` dan `recvfrom()` untuk datagram tanpa `listen()` atau `accept()`. `accept()` menghasilkan socket baru untuk satu hubungan TCP, sedangkan socket pendengar tetap dapat menerima hubungan lain. Panggilan nyata dapat gagal; diagram hanya menunjukkan jalur yang berhasil.

### 2.4.2 Fungsi utilitas jaringan (*Network Utility Functions*)

Program sering perlu menerjemahkan nama host menjadi alamat jaringan serta mengubah bilangan port/alamat ke urutan byte yang dipakai protokol. Di contoh klasik buku dan slide, keluarga fungsi resolusi nama seperti `gethostbyname()` serta konversi seperti `htons()` membantu pekerjaan ini. `htons()` mengubah bilangan 16-bit dari urutan byte komputer ke urutan byte jaringan; fungsi itu **tidak mencari alamat IP**. Dalam program baru, antarmuka seperti `getaddrinfo()` lebih umum untuk resolusi nama, tetapi fokus bagian ini adalah peran fungsi pembantu, bukan hafalan sintaks.

## 2.5 Protokol Lapisan Aplikasi dan Utilitas TCP/IP (*Application Layer Protocols and TCP/IP Utilities*)

| Nama | Fungsi utama | Hubungan yang perlu diingat |
|---|---|---|
| DNS | Menerjemahkan nama domain menjadi informasi alamat, dan jenis catatan lain | Pertanyaan singkat lazimnya lewat UDP; TCP juga dapat digunakan. |
| HTTP | Meminta dan mengirim sumber daya web | Contoh di slide memakai TCP. |
| SMTP | Mengirim surel antar sistem surat | Protokol aplikasi. |
| FTP | Memindahkan file dalam contoh klasik buku | Contoh klasik memakai koneksi kendali dan data terpisah. |

Contoh yang sering tertukar: **DNS dan HTTP sama-sama protokol aplikasi**, walau DNS mencari alamat dan HTTP meminta isi web. **TCP dan UDP sama-sama protokol transport**. **IP adalah protokol lapisan Internet/jaringan**.

### 2.5.1 Telnet

Telnet menyediakan terminal jarak jauh berbasis teks: pengguna mengetik perintah pada komputer lain melalui jaringan. Contoh ini membantu melihat hubungan aplikasi, socket, dan transport. Telnet klasik tidak mengenkripsi isi komunikasi, sehingga untuk akses jarak jauh yang membutuhkan kerahasiaan, SSH lazim dipakai sekarang.

### 2.5.2 File Transfer Protocol (FTP)

FTP mengirim dan mengambil file. Contoh klasik memakai hubungan kendali untuk perintah dan hubungan data untuk isi file. Jadi “mengirim file” adalah pekerjaan aplikasi, sedangkan TCP mengurus aliran byte yang dipakainya. Variasi aktif/pasif menentukan cara hubungan data dibuat; itu lebih lanjut daripada gagasan inti bab ini.

### 2.5.3 HTTP dan World Wide Web

Web adalah kumpulan sumber daya yang dapat dirujuk lewat URL; HTTP adalah protokol permintaan dan respons untuk mengambil atau mengirim sumber daya tersebut. Pada contoh buku/slide, klien mengirim `GET`, lalu server membalas status dan isi. **Web bukan HTTP saja:** DNS, transport, IP, dan media jaringan ikut bekerja. HTTPS menambahkan perlindungan TLS; itu contoh masa kini di luar alur HTTP klasik yang dipakai untuk belajar.

### 2.5.4 Utilitas IP

`ping` mengirim ICMP Echo dan mengukur waktu pulang-pergi jika ada balasan. `traceroute` atau `tracert` berusaha menampilkan beberapa titik perantara dengan membatasi umur paket. Tidak ada balasan `ping` **tidak cukup** untuk menyimpulkan tujuan mati, sebab balasan dapat dibatasi atau diblokir. `ipconfig` di Windows atau `ip addr` di Linux membantu melihat alamat lokal; `netstat` atau `ss` membantu melihat sambungan. Ini alat pengamatan, bukan lapisan OSI baru.

### 2.5.5 Tcpdump dan penganalisis protokol jaringan

Penganalisis paket seperti `tcpdump` menampilkan paket yang terlihat pada antarmuka yang diamati. Dari catatan waktu, alamat, protokol, dan port, kita dapat menelusuri contoh DNS atau TCP. Tangkapan pada satu titik **bukan** pandangan seluruh jaringan; lalu lintas lain mungkin tak melewati titik itu, dan isi yang terenkripsi tidak terbaca sebagai teks biasa.

## Ringkasan menjelang ujian

```text
Nama situs → DNS → alamat IP
Permintaan halaman → HTTP → memakai layanan transport
Transport → TCP/UDP → membedakan aplikasi dengan port
Internet → IP → mengantar paket antarnetwork
Data link → frame → mencapai tetangga pada satu sambungan
Fisik → bit sebagai sinyal

Lapisan = pembagian pekerjaan
Layanan = kemampuan bagi lapisan di atas
Protokol = aturan antar pihak setingkat
```

## Latihan bertahap

Jawab dahulu tanpa membuka kunci.

1. Sensei membuka `contoh.id`. Mengapa DNS diperlukan sebelum peramban menghubungi server memakai nama itu?
2. Bedakan lapisan, layanan, dan protokol dengan satu contoh TCP.
3. Urutkan: IP, HTTP, Ethernet, TCP, saat data dikirim dari aplikasi ke kabel.
4. Pada OSI, lapisan mana yang menangani rute antarbeberapa jaringan? Lapisan mana yang membawa frame ke tetangga langsung?
5. Mengapa dua aplikasi pada satu komputer dapat memakai jaringan bersamaan tanpa setiap balasan tertukar?
6. Apa beda model OSI dan susunan TCP/IP yang dipakai Internet?
7. Jika TCP bekerja di atas IP, apakah IP otomatis menjamin semua paket diterima? Jelaskan.
8. Apa yang terjadi pada frame ketika paket melewati router menuju sambungan lain?
9. Apakah tidak ada balasan `ping` membuktikan server mati? Jelaskan.
10. Sebutkan satu alasan mengapa UDP masih berguna meskipun tidak memberi jaminan pengiriman sendiri.

<details>
<summary>Kunci jawaban dan alasan</summary>

1. Nama mudah diingat manusia, sedangkan komunikasi jaringan membutuhkan alamat IP tujuan. DNS membantu menemukan alamat itu.
2. Transport adalah lapisan; aliran byte andal adalah layanan yang TCP berikan kepada aplikasi; TCP adalah protokol, yaitu aturan antarujung untuk menjalankan layanan tersebut.
3. HTTP → TCP → IP → Ethernet. Di bawah Ethernet, sinyal dikirim melalui media fisik.
4. Network/lapisan 3 mengurus perjalanan antarnetwork. Data Link/lapisan 2 mengurus frame pada satu sambungan.
5. Nomor port pada lapisan transport membantu membedakan proses atau layanan tujuan.
6. OSI adalah model acuan tujuh lapisan. TCP/IP ialah keluarga protokol nyata yang lazim disusun menjadi empat atau lima lapisan untuk penjelasan.
7. Tidak. IP memberi layanan *best effort*; TCP menambahkan mekanisme keandalan pada lapisan transport.
8. Router menerima frame, mengambil paket IP, memilih sambungan berikutnya, lalu membungkus paket dalam frame yang sesuai dengan sambungan keluar.
9. Tidak. ICMP bisa diblokir, dibatasi, atau balasannya hilang.
10. UDP sederhana dan tidak mewajibkan pembentukan koneksi pada UDP. Aplikasi dapat memilihnya ketika karakter itu cocok dan menangani kebutuhan tambahan sendiri.

</details>

## Rujukan dan batasan

- Kontrak perkuliahan KOM1314 Semester 1 2026/2027, tabel topik **pertemuan 2**.
- Alberto Leon-Garcia dan Indra Widjaja, *Communication Networks: Fundamental Concepts and Key Architectures*, edisi ke-2, Bab 2: “Applications and Layered Architectures”.
- Slide kuliah `Garcia-2003_ch2-Applications and Layered Architectures.pdf`, bagian *Protocols, Services & Layering*, *OSI Reference Model*, *TCP/IP Architecture*, dan *Application Layer Protocols & Utilities*.

Materi ini ditulis ulang untuk belajar. Contoh teknologi modern diberi penanda bila berbeda dari alur klasik pada slide; contoh alamat, perintah, dan hasil jaringan bukan hasil pengukuran nyata.
