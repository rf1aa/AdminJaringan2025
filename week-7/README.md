<div align="center">
  <h1 style="text-align: center;font-weight: bold">LAPORAN RESMI<br>WORKSHOP ADMINISTRASI JARINGAN</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/id/4/44/Logo_PENS.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh : </h3>
  <p style="text-align: center;">
    <strong>Ale Perdana Putra Darmawan (3123500027) </strong><br>
  </p>
<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

## Daftar Isi
- [Daftar Isi](#daftar-isi)
- [Konfigurasi Mikrotik](#konfigurasi-mikrotik)

## Konfigurasi Mikrotik

Langkah 1:  
Unduh WinBox di `https://mikrotik.com/download`.  
Gambar:  
<img src="assets/wb1.png" width="500" />

Langkah 2:  
Buka file WinBox yang telah diunduh. Isi **Connect To** sesuai dengan IP router Mikrotik yang sedang digunakan. Untuk **Login** dan **Password** gunakan `admin`.  
Gambar:  
<img src="assets/wb2.jpeg" width="500" />

Langkah 3:  
Jika sudah berhasil terhubung, buka terminal dengan memilih menu **New Terminal**. Kemudian tambahkan routing dengan perintah:  
```bash
ip route add dst-address=<tujuan-network> gateway=<IP-router>
```  
Gambar:  
<img src="assets/wb3.jpeg" width="500" />

Langkah 4:  
Setelah selesai menambahkan routing, lakukan pengujian koneksi menggunakan perintah:  
```bash
ping <IP-dalam-network>
```  
Gambar:  
<img src="assets/wb4.jpeg" width="500" />
```

Silakan salin bagian tersebut dan uji kembali di platform Anda. Bila masih belum muncul, kemungkinan besar adalah masalah pada nama file atau penempatan folder. Ingin saya bantu cek file gambar atau struktur foldernya juga?
