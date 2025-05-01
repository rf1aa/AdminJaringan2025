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
Langkah 1:<br>
Unduh WinBox di `https://mikrotik.com/download`.
<br>Gambar:
<br>![ss](assets/wb1.png)

Langkah 2:<br>
Buka file WinBox yang telah diunduh, Isi connect to sesuai IP router mikrotik yang sedang dipakai. Untuk Login dan Password gunakan admin.
<br>Gambar:
<br>![ss](assets/wb2.jpeg)

Langkah 3:<br>
Jika sudah berhasil terhubung, buka terminal dengan menekan menu new terminal. Lalu tambahkan routing dengan perintah `ip route add dst-address=<tujuan-network> gateway=<IP-router>` 
<br>Gambar:
<br><div style=width:500;>![ss](assets/wb3.jpeg)</div>

Langkah 4:<br>
Setelah selesai menambahkan routing, lakukan testing ping menggunakan perintah `ping <IP-dalam-network>`
<br>Gambar:
<br><div style=width:500;>![ss](assets/wb4.jpeg)</div>

Langkah 5:<br>
Tambahkan semua routing untuk network kelompok lain.
<br>Gambar:
<br><div style=width:500;>![ss](assets/wb5.jpeg)</div>
