# Laporan Umum
## Workshop Administrasi Jaringan
![Logo PENS](images/pens.png)

**Dosen Pengampu:**  
Dr. Ferry Astika Saputra, S.T., M.Sc.

**Oleh:**  
Muhammad  
NRP: [Nomor Induk Anda]  
4 D3 Teknik Informatika A

**PROGRAM STUDI D3 TEKNIK INFORMATIKA**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  
2024/2025

---

## Tugas
1. **Analisa file http.cap dengan Wireshark**:
   - Versi HTTP yang digunakan
   - IP address dari client maupun server
   - Waktu dari client mengirimkan HTTP request
   - Waktu dari server mengirimkan respon dan durasinya
2. **Deskripsi gambar pada slide**
3. **Rangkuman tahapan komunikasi menggunakan TCP**

## Jawaban
### 1. Analisa http.cap dengan Wireshark
**Versi HTTP yang digunakan:** HTTP/1.1

![Versi HTTP](images/http_version.jpg)

**IP Address:**
- Client: 145.254.160.237  
  ![IP Client](images/ip_client.jpg)
- Server: 65.208.228.223  
  ![IP Server](images/ip_server.jpg)
- Server iklan: 216.239.59.99  
  ![IP Iklan](images/ip_advertisement.jpg)

**Waktu komunikasi:**
- Client mengirimkan HTTP request pada 0.91131 detik setelah mulai di-capture.  
  ![Waktu Request](images/request_time.jpg)
- Server mengirimkan respon pada 4.846969 detik setelah capture dimulai.  
  ![Waktu Respon](images/response_time.jpg)
- Durasi total: 4.846969 - 0.91131 = **3.935659 detik**

### 2. Deskripsi Gambar
![Struktur Paket](images/packet_structure.jpg)
- Data Link Layer menggunakan **MAC Address** untuk berpindah dalam jaringan lokal.
- Network Layer menangani **IP Address** sebagai alamat tujuan antar jaringan.
- Transport Layer mengatur **port number** untuk memastikan data dikirim ke aplikasi yang benar.
- **Dynamic Port** digunakan untuk membedakan request yang datang dari tab berbeda dalam browser.

### 3. Rangkuman Tahapan Komunikasi TCP
#### 1) **Three-Way Handshake** (Membangun Koneksi)
![Three-Way Handshake](images/three_way_handshake.jpg)
   a. **SYN** → Klien meminta koneksi ke server.
   b. **SYN-ACK** → Server merespons dengan acknowledge.
   c. **ACK** → Klien mengonfirmasi koneksi.

#### 2) **Data Transfer** (Pengiriman Data)
![Data Transfer](images/data_transfer.jpg)
- Data dikirim dalam segmen dengan **sequence number** agar tetap urut.
- TCP memastikan data diterima tanpa kehilangan atau perubahan urutan.

#### 3) **Connection Termination** (Menutup Koneksi)
![Connection Termination](images/connection_termination.jpg)
   a. **FIN** → Klien meminta menutup koneksi.
   b. **ACK** → Server mengakui permintaan.
   c. **FIN** → Server meminta menutup koneksi.
   d. **ACK** → Klien mengonfirmasi.

Jika tab ditutup paksa, **RST (Reset) dikirim oleh klien**, dan server bisa tetap mengirim data sebelum mengetahui koneksi telah diputus.

---

**Hak Cipta © 2024 Muhammad. Semua hak dilindungi.**


