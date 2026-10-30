# 🛡️ SOC Wireshark Extra Project - Week 1 (NetworkWalks)

<p align="center">
  <img src="https://img.shields.io/badge/Author-Angga%20Yoga%20Syahputra-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="Author" />
  <img src="https://img.shields.io/badge/Batch-B084-0052CC?style=for-the-badge&logo=id-badge&logoColor=white" alt="Batch" />
  <img src="https://img.shields.io/badge/Affiliation-Universitas%20Brawijaya-003366?style=for-the-badge&logo=google-classroom&logoColor=white" alt="Affiliation" />
  <img src="https://img.shields.io/badge/Tool-Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white" alt="Wireshark" />
  <img src="https://img.shields.io/badge/Status-Completed%20(100%25%20Score)-success?style=for-the-badge&logo=check-circle&logoColor=white" alt="Status" />
</p>

---

## 📌 1. Project Profile & Executive Summary

### 👤 Project Information
| Attribute | Specification Details |
| :--- | :--- |
| **Full Name** | **Angga Yoga Syahputra** |
| **Participant ID / Batch** | **B084** |
| **Academic Affiliation** | **Universitas Brawijaya** |
| **Program** | **Cybersecurity Internship Program — NetworkWalks Academy** |
| **Project Module** | **SOC Wireshark Extra Project - Week 1** |
| **Completion Date** | **31 Oktober 2026** |
| **Remote Repository** | [GitHub Repository](https://github.com/anggaaygaa/Networkwalks-Angga-Yoga-Syahputra-B084-SOC-Wireshark-Extra-Project-Week-1.git) |

### 🎯 Executive Summary
Repositori ini mendokumentasikan penyelesaian komprehensif **SOC Wireshark Extra Project - Week 1** yang diselenggarakan oleh **NetworkWalks Academy**. Proyek ini dirancang untuk mensimulasikan tugas harian seorang **Security Operations Center (SOC) Analyst** dan **Network Forensics Investigator** dalam menganalisis tangkapan paket data mentah (`.pcapng`).

Fokus investigasi mencakup:
1. **Network Anomaly & Port Scanning Detection**: Mengisolasi pola probing agresif dan memetakan vektor serangan penyerang.
2. **TCP Stream Reconstruction**: Merekonstruksi sesi percakapan jaringan unencrypted untuk mengekstrak kredensial dan informasi sensitif.
3. **Payload Inspection & Data Extraction**: Melacak transmisi file tersembunyi, token rahasia, dan kredensial autentikasi *clear-text*.
4. **Brute Force Attack Identification**: Menganalisis lonjakan percobaan login berulang dan membedakan kegagalan dengan kompromi sukses.
5. **VoIP Telephony Forensics & Audio Replay**: Memisahkan protokol pensinyalan (SIP) dari media suara (RTP) serta merekonstruksi rekaman audio penipuan (*vishing*).
6. **Advanced PCAP Packet Diagnostics**: Mengukur metrik hop TTL, distribusi protokol jaringan, pasangan host volume terbesar (*top talkers*), dan retransmisi TCP akibat kongesti atau paket drop.

---

## 🏆 2. Summary of Achievements

Seluruh modul laboratorium interaktif dan kuis penilaian diselesaikan dengan skor sempurna (**100% / Grade A**):

| No | Module / Lab Name | Focus Area & Key Artifacts | Final Score |
| :-: | :--- | :--- | :-: |
| **01** | **Wireshark SOC Lab 3 · Identify the Attacker's IP** | *Port Scanning Detection, SYN Packet Filtering (`tcp.flags.syn==1`), Reconnaissance Analysis, Open vs. Closed Port Response Evaluation (`RST, ACK`)* | **100%** (100/100 marks) |
| **02** | **Wireshark SOC Lab 4 · Follow TCP Stream** | *Internal Chat Session Reassembly, TCP Stream Reconstruction, IT Helpdesk Credential & Wi-Fi Extraction, Stream Direction & Syntax Coloring Analysis* | **100%** (100/100 marks) |
| **03** | **Wireshark SOC Lab 5 · Follow TCP Stream, part 2** | *Advanced Multi-Stream Payload Analysis, Hidden Data & Secret Code Word Discovery, Backup Server Identification, File Exfiltration Tracking* | **100%** (100/100 marks) |
| **04** | **Wireshark SOC Lab 6 · HTTP plain text credentials** | *Clear-text Protocol Vulnerability Assessment, HTTP POST Request Inspection, Form Data Parameter Extraction (`username` & `password`), TLS/HTTPS Hardening* | **100%** (100/100 marks) |
| **05** | **Wireshark SOC Lab 7 · Brute force catch** | *Authentication Brute Force Investigation, Failed vs. Successful Login Correlation, Anomaly Frequency Analysis, Credential Compromise Identification* | **100%** (100/100 marks) |
| **06** | **Wireshark SOC Lab 8 · VoIP call replay** | *VoIP Protocol Decoupling (SIP Signalling vs. RTP Media Stream), Audio Codec Forensics, Call Player Replay, Financial Fraud / Scammer Intelligence Extraction* | **100%** (100/100 marks) |
| **07** | **Wireshark Quiz (Basic Level)** | *Wireshark UI Layout Mastery (Three-Pane Architecture), OSI/TCP-IP Stack Inspection, Fundamental Protocol Analysis (ARP, ICMP, DNS, TCP, HTTP), Display Filters* | **100%** (10/10 Correct) |
| **08** | **Wireshark Quiz (Advanced Level)** | *Advanced PCAP Forensic Evaluation (TTL Hop Analysis, Obfuscated Token Hunting, Protocol Hierarchy & Conversation Statistics, TCP Retransmission Diagnostics, Plain-text Credential Extraction)* | **100%** (10/10 Correct) |

---

## 🔍 3. Technical Walkthrough & Forensic Breakdown

### 🔬 3.1 Wireshark SOC Labs (Lab 3 – Lab 8)

```
+----------------------------------------------------------------------------------------------------+
|                                    SOC INCIDENT TRIAGE WORKFLOW                                    |
+----------------------------------------------------------------------------------------------------+
|  [Reconnaissance]       [Stream Follow]       [Cleartext Hunt]     [Brute-Force]     [VoIP Replay] |
|   Lab 3: Port Scan  ->   Lab 4 & 5: TCP   ->   Lab 6: HTTP POST  -> Lab 7: Auth   ->  Lab 8: RTP   |
|   SYN Flag Anomaly       Chat & Hidden Data    Plain Credentials    Spray Analysis    Audio Fraud  |
+----------------------------------------------------------------------------------------------------+
```

#### 🛡️ Lab 3 – Identify the Attacker's IP
- **Skenario Investigasi**: Terdeteksi aktivitas pemindaian port yang mencurigakan terhadap aset infrastruktur internal. Tugas analis SOC adalah mengidentifikasi penyerang, target, serta memetakan port yang terbuka.
- **Taktik & Display Filter**:
  - Mengisolasi paket inisiasi koneksi TCP menggunakan filter SYN:
    ```wireshark
    tcp.flags.syn == 1 && tcp.flags.ack == 0
    ```
    *(atau menggunakan tombol simulator `Show only SYN packets`)*.
- **Temuan Forensik Utama**:
  - **IP Attacker**: Teridentifikasi host penyerang yang mengirimkan paket SYN dalam frekuensi tinggi dan jeda waktu sangat singkat.
  - **IP Target**: Host server internal yang menjadi sasaran *reconnaissance*.
  - **Source Port & Probed Ports**: Penyerang menggunakan *source port* acak/efemeral untuk memindai serangkaian *destination port* spesifik pada target.
  - **Open Ports**: Port layanan yang merespons dengan paket `[SYN, ACK]`, mengonfirmasi layanan aktif (seperti port HTTP/Web, FTP, dan SSH).
  - **Closed Ports**: Port non-aktif yang merespons dengan flag `[RST, ACK]` (*Connection Reset*).
  - **Klasifikasi Serangan**: *TCP SYN Stealth Port Scanning (Half-Open Scan)*.
- **Rekomendasi Mitigasi**: Implementasi aturan firewall *stateful*, pembatasan *rate limiting* SYN pada router border, penerapan IDS/IPS rules (Snort/Suricata) untuk mendeteksi SYN scan anomalies, dan penutupan port yang tidak esensial.

---

#### 💬 Lab 4 – Follow TCP Stream
- **Skenario Investigasi**: Rekonstruksi sesi komunikasi *internal chat* antara karyawan dan staf IT Support yang tidak terenkripsi pada jaringan lokal.
- **Taktik & Display Filter**:
  - Menggunakan fitur **Follow > TCP Stream** pada paket TCP pertama:
    ```wireshark
    tcp.stream eq 0
    ```
- **Temuan Forensik Utama**:
  - **Chat Port**: Port aplikasi komunikasi teks internal yang digunakan oleh kedua pihak.
  - **Initiator Komunikasi**: Identitas host/pengguna yang pertama kali mengirimkan salam pembuka dan menginisiasi koneksi.
  - **Stream Index & Ukuran**: Sesi berlangsung pada Stream `0` dengan ukuran payload chat beberapa kilobyte.
  - **Identifikasi Masalah & Artefak**:
    - Pengguna meminta bantuan terkait drive penyimpanan jaringan yang terkunci (*problem drive*).
    - Ditemukan kredensial sensitif yang dibagikan secara ceroboh dalam bentuk teks polos: **Temporary Password** dan **Guest Wi-Fi Password**.
  - **Aturan Pewarnaan Wireshark TCP Stream**: Teks berwarna **Merah** menandakan transmisi data dari inisiator/klien (*Client Request*), sedangkan teks berwarna **Biru** menandakan balasan dari pihak penerima/server (*Server Response*).
- **Rekomendasi Mitigasi**: Wajibkan penggunaan platform *enterprise messaging* yang menerapkan enkripsi *End-to-End* (E2EE/TLS), terapkan kebijakan DLP (*Data Loss Prevention*) untuk mencegah pengiriman kata sandi melalui pesan instan, dan terapkan mekanisme *temporary password reset* otomatis melalui portal IAM resmi.

---

#### 🕵️ Lab 5 – Follow TCP Stream, part 2
- **Skenario Investigasi**: Penyelidikan tingkat lanjut terhadap aliran TCP multi-sesi untuk melacak halaman web yang dikunjungi, mengidentifikasi server cadangan, dan mengekstrak data tersembunyi.
- **Taktik & Display Filter**:
  - Memeriksa urutan stream TCP:
    ```wireshark
    tcp.stream eq 1  # atau tcp.stream eq 2
    ```
- **Temuan Forensik Utama**:
  - **Halaman Web yang Diakses**: Permintaan HTTP GET menuju path dokumen dan laporan internal perusahaan.
  - **Lokasi Data Tersembunyi (*Where is it hidden*)**: Artefak rahasia disisipkan di dalam body payload file dokumen yang ditransfer melalui aliran TCP.
  - **Stream Number**: Ditemukan pada nomor stream tertentu yang memuat transfer file internal.
  - **Secret Code Word**: Berhasil mengekstrak kata kode (*code word*) rahasia yang diselundupkan di dalam payload.
  - **Backup Server & Arah Aliran**: Teridentifikasi alamat IP server cadangan (*backup server*) dan arah trafik data (*inbound/outbound*).
  - **HTTP Status Code**: Terverifikasi status `HTTP/1.1 200 OK` yang menandakan pengunduhan file berhasil seutuhnya.
  - **File & Pembuat Laporan**: Berhasil mengekstrak nama file arsip/laporan yang diunduh beserta metadata nama staf pembuat laporan (*report author*).
- **Rekomendasi Mitigasi**: Terapkan enkripsi transport (HTTPS/TLS) untuk seluruh portal intranet, batasi akses repositori cadangan hanya untuk IP terotorisasi via VPN/IPsec, serta pasang WAF dan *Network File Inspection*.

---

#### 🔓 Lab 6 – HTTP Plain Text Credentials
- **Skenario Investigasi**: Audit keamanan terhadap lalu lintas otentikasi web tanpa enkripsi (HTTP port 80) yang rentan terhadap penyadapan (*packet sniffing* / MITM).
- **Taktik & Display Filter**:
  - Memfilter permintaan pengiriman formulir web login:
    ```wireshark
    http.request.method == "POST"
    ```
- **Temuan Forensik Utama**:
  - **HTTP Method & URL Login**: Metode yang digunakan adalah `POST` dengan endpoint URL formulir login (e.g., `/login.php`).
  - **IP PC Pengguna**: Alamat IP mesin klien internal yang melakukan *sign-in*.
  - **Jumlah Percobaan Login**: Terhitung frekuensi pengiriman paket POST login yang dilakukan oleh pengguna.
  - **Ekstraksi Kredensial**: Pada panel *Packet Details*, di bawah header `Hypertext Transfer Protocol` -> sub-bagian `HTML Form URL Encoded`, kredensial **username** dan **password** langsung terbaca tanpa enkripsi atau hashing.
  - **Akar Masalah**: Protokol HTTP mentransmisikan seluruh data *layer* aplikasi dalam representasi ASCII mentah.
- **Rekomendasi Mitigasi (*The Fix*)**:
  - Migrasi seluruh layanan web ke **HTTPS (TLS 1.3)**.
  - Konfigurasikan header `Strict-Transport-Security` (HSTS) untuk mencegah *SSL Stripping*.
  - Tambahkan atribut `Secure` dan `HttpOnly` pada session cookie, serta terapkan *Multi-Factor Authentication* (MFA).

---

#### 🔨 Lab 7 – Brute Force Catch
- **Skenario Investigasi**: Menyelidiki serangan tebak kata sandi otomatis (*brute force / dictionary attack*) terhadap portal otentikasi.
- **Taktik & Display Filter**:
  - Melacak rentetan paket autentikasi dan menganalisis kode status balasan server:
    ```wireshark
    http.request.method == "POST" || http.response.code == 401 || http.response.code == 200
    ```
- **Temuan Forensik Utama**:
  - **Protokol & Layanan**: Layanan web otentikasi diserang melalui protokol HTTP.
  - **IP Attacker & Target**: Ditemukan alamat IP eksternal penyerang yang mengirimkan ratusan request dalam hitungan detik ke IP server target.
  - **Username Target**: Penyerang menargetkan akun spesifik (e.g., `admin`).
  - **Pola Respons Gagal (*Failure Code*)**: Server berulang kali merespons dengan kode gagal (*Unauthorized*) terhadap rentetan tebakan kata sandi.
  - **Respons Berhasil (*Success Code*)**: Di akhir rentetan kegagalan, muncul satu respons berhasil (*HTTP 200 OK* atau pengalihan sukses).
  - **Password yang Ditembus**: Kredensial kata sandi yang cocok berhasil diisolasi dari paket POST terakhir sebelum sesi login sukses terbentuk.
  - **Klasifikasi Serangan**: *Dictionary-based Credential Brute Forcing*.
- **Rekomendasi Mitigasi**: Terapkan mekanisme *Account Lockout* setelah 3-5 kali gagal, pasang CAPTCHA / Cloudflare Turnstile pada endpoint login, integrasikan *Fail2ban* / WAF rate limiting, dan terapkan autentikasi kunci publik atau MFA.

---

#### 📞 Lab 8 – VoIP Call Replay
- **Skenario Investigasi**: Analisis forensik terhadap rekaman panggilan suara Voice over IP (VoIP) untuk membongkar penipuan telepon (*call scam / vishing*) dan mengekstrak data kartu keuangan yang dibocorkan korban.
- **Taktik & Display Filter**:
  - Memisahkan protokol pensinyalan (SIP) dan protokol transmisi suara (RTP):
    ```wireshark
    sip || rtp
    ```
  - Membuka menu Wireshark: **Telephony > VoIP Calls**, memilih panggilan yang terdeteksi, lalu klik **Play Streams**.
- **Temuan Forensik Utama**:
  - **Pemisahan Protokol**:
    - **SIP (Session Initiation Protocol)**: Digunakan untuk inisiasi, registrasi nomor, negosiasi sesi, dan terminasi panggilan pada port UDP 5060.
    - **RTP (Real-time Transport Protocol)**: Digunakan untuk streaming transmisi audio mentah secara *real-time*.
  - **Caller Extension & Name**: Ekstensi nomor telepon dan identitas penelepon (*Caller ID*) tertera pada header SIP `From:`.
  - **Audio Codec**: Sesi audio menggunakan codec standar VoIP (e.g., G.711 PCMU/PCMA).
  - **Penerima Panggilan**: Ekstensi dan nama pihak yang mengangkat panggilan (*Callee*) pada header SIP `To:`.
  - **Rekonstruksi Percakapan Suara (*Audio Replay*)**: Pemutaran stream RTP mengungkap percakapan penipuan rekayasa sosial (*social engineering*) di mana penipu berhasil mengelabui korban untuk membacakan data finansial sensitif.
  - **Artefak Finansial yang Bocor**: Berhasil mengekstrak nomor kartu kredit (*Card Number*) dan tanggal kedaluwarsa (*Expiry Date*).
- **Rekomendasi Mitigasi (*The Fix*)**:
  - Terapkan **SRTP (Secure Real-time Transport Protocol)** untuk enkripsi payload suara.
  - Gunakan **TLS untuk pensinyalan SIP (SIPS)** guna mencegah *eavesdropping* metadata panggilan.
  - Pisahkan jaringan suara ke dalam **Voice VLAN** khusus yang diisolasi dengan ACL ketat.
  - Edukasi kesadaran keamanan (*security awareness*) bagi staf terhadap taktik *voice phishing (vishing)*.

---

### 📝 3.2 Wireshark Quiz – Basic Level (`100% Score`)
Kuis tingkat dasar menguji pemahaman arsitektur fundamental Wireshark dan analisis lalu lintas jaringan:
- **Arsitektur Tiga Panel Wireshark (*Three-Pane Layout*)**:
  1. **Packet List Pane**: Menampilkan ringkasan seluruh paket (No, Time, Source, Destination, Protocol, Length, Info).
  2. **Packet Details Pane**: Menampilkan dekonstruksi struktur protokol berlapis (*protocol tree*) sesuai model OSI dan TCP/IP.
  3. **Packet Bytes Pane**: Menampilkan representasi heksadesimal (*Hex Dump*) dan ASCII dari payload paket mentah.
- **Inspeksi Protokol Jaringan**: Membedakan karakteristik protokol lapisan Link (Ethernet, ARP), Network (IPv4, IPv6, ICMP), Transport (TCP dengan *three-way handshake*, UDP *connectionless*), dan Application (DNS, HTTP).
- **Sintaks Display Filter Dasar**: Penerapan ekspresi logika boolean (`and`/`&&`, `or`/`||`, `not`/`!`), filtering alamat IP (`ip.addr == ...`, `ip.src == ...`), serta port spesifik (`tcp.port == 80`, `udp.port == 53`).

---

### 🎯 3.3 Wireshark Quiz – Advanced Level (`100% Score` - Rincian Per Soal)
Analisis mendalam terhadap file rekaman paket jaringan (`.pcapng`) pada kuis tingkat lanjut:

#### 🌐 Q3 (`ping_test_q3.pcapng` - How far did the reply travel?)
- **Tujuan**: Menganalisis jarak tempuh paket balasan ICMP berdasarkan nilai *Time to Live* (TTL).
- **Investigasi**: Pada Frame 6, diidentifikasi paket *Echo (ping) reply* yang dikirimkan oleh server Google Public DNS (`8.8.8.8`) menuju host lokal `192.168.0.10`.
- **Hasil Verifikasi**: Header IPv4 menunjukkan nilai **`Time to Live (TTL) = 117`**. Dengan nilai default TTL awal sistem pengirim sebesar 128, paket telah melintasi tepat 11 hop router perantara sebelum mencapai tujuan.

#### 🔑 Q4 (`portal_q4.pcapng` - The hidden code)
- **Tujuan**: Menemukan kode akses rahasia yang tersimpan di dalam berkas web server.
- **Investigasi**: Menggunakan display filter `http.request.uri contains "readme.txt"` untuk memisahkan permintaan `/backup/readme.txt` (Frame 36) dari `/backup/old-readme.txt` (Frame 26).
- **Hasil Verifikasi**: Melakukan *Follow TCP Stream* (`tcp.stream eq 3`) pada sesi komunikasi ke server `portal.company.local` (`192.168.10.80`), mengungkap kode otorisasi rahasia: **`TIGER-4471`**.

#### 📊 Q5 (`office_q5.pcapng` - The office mix)
- **Tujuan**: Menganalisis proporsi komposisi protokol jaringan pada lingkungan perkantoran.
- **Investigasi**: Membuka menu **Statistics > Protocol Hierarchy** untuk mengevaluasi distribusi 50 paket yang terekam.
- **Hasil Verifikasi**: Protokol transport **TCP** mencakup tepat **`48.0%`** (24 paket) dari total keseluruhan lalu lintas jaringan, sementara sisanya didistribusikan ke protokol lain seperti UDP dan ICMP.

#### 💾 Q6 (`transfers_q6.pcapng` - Who moved the most data?)
- **Tujuan**: Mengidentifikasi pasangan host (*top talkers*) yang mempertukarkan volume data terbesar.
- **Investigasi**: Membuka menu **Statistics > Conversations**, memilih tab **IPv4**, dan mengurutkan tabel berdasarkan kolom *Bytes*.
- **Hasil Verifikasi**: Pasangan host **`172.16.5.10` dan `172.16.5.50`** mentransfer volume data tertinggi, yaitu sebesar **`37 kB`** (43 paket).

#### ⏱️ Q7 (`slow_download_q7.pcapng` - A flaky download)
- **Tujuan**: Mendiagnosis penyebab penurunan performa dan kelambatan pengunduhan file `/report.pdf` dari file server (`10.1.1.100`).
- **Investigasi**: Menerapkan display filter diagnostik performa TCP:
  ```wireshark
  tcp.analysis.retransmission
  ```
- **Hasil Verifikasi**: Terdeteksi tepat **`4` paket TCP Retransmission** (tercatat pada Frame 15, Frame 19, Frame 20, dan Frame 23), membuktikan adanya paket drop pada jalur transmisi data.

#### 🔴 Q8 (`colours_q8.pcapng` - Reading the colours)
- **Tujuan**: Mengidentifikasi penolakan koneksi (*connection refused*) melalui aturan pewarnaan default Wireshark.
- **Investigasi**: Mencari paket dengan latar belakang merah tua bertuliskan *TCP RST* pada daftar paket.
- **Hasil Verifikasi**: Ditemukan paket penolakan koneksi dengan flag `[RST, ACK]` pada **Frame 16**, dikirim dari server target `10.5.5.50:8081` ke klien `10.5.5.10:50302`.

#### 📁 Q9 (`ftp_login_q9.pcapng` - Passwords in the clear - FTP)
- **Tujuan**: Melacak kredensial login akun administrator yang dikirim tanpa enkripsi pada layanan FTP server `192.168.20.30` (`vsFTPd 3.0.3`).
- **Investigasi**: Menggunakan filter `ftp.request.arg contains "netadmin"` pada Frame 20, lalu melakukan *Follow TCP Stream* (`tcp.stream eq 1`).
- **Hasil Verifikasi**: Kredensial *plain-text* berhasil diekstrak seutuhnya:
  - **Username**: `USER netadmin`
  - **Password**: `PASS Backup2026`

#### 🌐 Q10 (`intranet_login_q10.pcapng` - Passwords in the clear - Web Login)
- **Tujuan**: Mengisolasi upaya otentikasi intranet dari host `192.168.30.25` ke web server `192.168.30.100`.
- **Investigasi**: Menggunakan display filter `http.request.method == "POST"` untuk memeriksa seluruh pengiriman kredensial.
- **Hasil Verifikasi**:
  - Frame 10: `admin / admin123` (Gagal / respons otentikasi ditolak)
  - Frame 14: `sara.khan / Winter2025` (Gagal / respons otentikasi ditolak)
  - Frame 18: `sara.khan / Winter2026` (Berhasil)
  - Respon server pada Frame 19 mengembalikan payload HTML bertuliskan `"Welcome back"`, mengonfirmasi bahwa kredensial valid adalah **`sara.khan`** dengan password **`Winter2026`**.

---

## 📸 4. Proof of Completion (Documentation Gallery)

Berikut adalah bukti tangkapan layar resmi hasil evaluasi dan penyelesaian tugas dengan perolehan skor sempurna (**100%**):

### 1. Wireshark SOC Labs (Lab 3 – Lab 8)

#### 🔹 Lab 3: Identify the Attacker's IP (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%203%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 3 Result - Angga Yoga Syahputra" width="850" />
</p>

---

#### 🔹 Lab 4: Follow TCP Stream (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%204%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 4 Result - Angga Yoga Syahputra" width="850" />
</p>

---

#### 🔹 Lab 5: Follow TCP Stream, part 2 (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%205%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 5 Result - Angga Yoga Syahputra" width="850" />
</p>

---

#### 🔹 Lab 6: HTTP plain text credentials (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%206%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 6 Result - Angga Yoga Syahputra" width="850" />
</p>

---

#### 🔹 Lab 7: Brute force catch (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%207%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 7 Result - Angga Yoga Syahputra" width="850" />
</p>

---

#### 🔹 Lab 8: VoIP call replay (Score: 100% | 100/100 marks)
<p align="center">
  <img src="./Lab%208%20Result%20-%20Angga%20Yoga%20Syahputra.png" alt="Lab 8 Result - Angga Yoga Syahputra" width="850" />
</p>

---

### 2. Wireshark Quiz (Basic Level)

#### 🔹 Wireshark Quiz - Basic Level (Score: 100% | 10 Correct, 0 Wrong, 0 Skipped)
<p align="center">
  <img src="./wireshark-basic-quiz.png" alt="Wireshark Basic Quiz - Angga Yoga Syahputra" width="850" />
</p>

---

### 3. Wireshark Quiz (Advanced Level)

#### 🔹 Wireshark Quiz - Advanced Level (Score: 100% | 10 Correct, 0 Wrong, 0 Skipped)
<p align="center">
  <img src="./wireshark-advanced-quiz.png" alt="Wireshark Advanced Quiz - Angga Yoga Syahputra" width="850" />
</p>

---

## 🛡️ 5. DevSecOps & Security Hardening Recommendations

Berdasarkan temuan forensik pada proyek ini, berikut adalah rekomendasi pengerasan keamanan (*hardening*) terstandar:

```
+----------------------------------------------------------------------------------------------------+
|                               DEFENSE-IN-DEPTH REMEDIATION MATRIX                                  |
+----------------------+------------------------------------+----------------------------------------+
| Threat Category      | Root Cause                         | DevSecOps / SOC Defense Control        |
+----------------------+------------------------------------+----------------------------------------+
| Clear-text Sniffing  | Unencrypted protocols (HTTP/FTP)   | Mandate TLS 1.3, HSTS Preload, SFTP    |
| Port Scanning        | Exposed services, no rate limits   | Stateful ACLs, Port Knocking, Suricata |
| Brute Force Attacks  | Unlimited login attempts           | Fail2ban, WAF Rate Limit, Mandatory MFA|
| VoIP Eavesdropping   | Unencrypted SIP & RTP streams      | Enforce SIPS (TLS) & SRTP, Voice VLAN  |
| Network Stalling     | Packet loss, MTU mismatch          | TCP Window Auto-tuning, QoS Policies   |
+----------------------+------------------------------------+----------------------------------------+
```

1. **Enforce End-to-End Cryptography**: Seluruh komunikasi web, transfer file, dan pensinyalan suara wajib dienkripsi menggunakan protokol modern (HTTPS/TLS 1.3, SFTP, SIPS, SRTP).
2. **Zero-Trust Network Architecture**: Segregasikan subnet produksi, server cadangan, dan voice telephony ke dalam VLAN terisolasi dengan akses berbasis prinsip hak akses terendah (*Least Privilege*).
3. **Automated SOC Detection Rules**: Terapkan deteksi anomali pada SIEM/EDR untuk mendeteksi lonjakan koneksi TCP SYN mendadak dan lonjakan kode respon HTTP 401 berurutan dalam periode waktu singkat.
4. **Credential Security**: Hindari pertukaran kredensial melalui aplikasi chat internal, implementasikan *Enterprise Password Manager* dan *Multi-Factor Authentication* (MFA) berbasis FIDO2/WebAuthn.

---

## 📜 6. References & Acknowledgments
- **NetworkWalks Academy**: *Cybersecurity Internship Program - Week 1 SOC Module*.
- **Wireshark Foundation**: *Official Wireshark User's Guide & Display Filter Reference Manual*.
- **IETF RFCs**:
  - RFC 793 (Transmission Control Protocol)
  - RFC 3261 (SIP: Session Initiation Protocol)
  - RFC 3550 (RTP: A Transport Protocol for Real-Time Applications)
  - RFC 7230-7235 (Hypertext Transfer Protocol)

---
<p align="center">
  <b>Developed with dedication by Angga Yoga Syahputra (B084)</b><br>
  <i>Cybersecurity Analyst & DevSecOps Enthusiast — Universitas Brawijaya</i>
</p>
