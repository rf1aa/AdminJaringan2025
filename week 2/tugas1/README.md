<div align="center">
  <h1 style="text-align: center;font-weight: bold">Laporan Resmi<br>Workshop Admnistrasi Jaringan</h1>
  <h4 style="text-align: center;">Dosen Pengampu : Dr. Ferry Astika Saputra, S.T., M.Sc.</h4>
</div>
<br />
<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/id/4/44/Logo_PENS.png" alt="Logo PENS">
  <h3 style="text-align: center;">Disusun Oleh : </h3>
  <p style="text-align: center;">
    <strong>Muhammad Rafi Dhiyaulhaq (3123500004) </strong><br>
  </p>
<h3 style="text-align: center;line-height: 1.5">Politeknik Elektronika Negeri Surabaya<br>Departemen Teknik Informatika Dan Komputer<br>Program Studi Teknik Informatika<br>2024/2025</h3>
  <hr><hr>
</div>

## Instalasi Virtual Box Menggunakan Lab Komputer
1. Mengunduh VirtualBox package dari laman `https://www.virtualbox.org/wiki/Linux_Downloads`. Lalu pilih distro linux yang sedang digunakan. Dalam lab computer, distro yang digunakan adalah debian 12.
<br><div style=width:500;>![ss](assets/1.jpeg)</div>

2. Setelah mengunduh, buka terminal dan masuk ke dalam directory `Downloads` menggunakan `cd Downloads`. Dalam directory, jalankan command `sudo dpkg -i [nama-paket-VirtualBox]` untuk memulai instalasi VirtualBox dengan keterangan `sudo` untuk mendapatkan hak akses administrator, `dpgk` untuk instalasi paket berjenis `.deb`, dan `-i` untuk opsi instalasi.
<br><div style=width:500;>![ss](assets/2.jpeg)</div>

3. Pada saat instalasi, komputer yang saya gunakan mengalami kendala dikarenakan terdapat dependency yang belum terinstall. Solusi yang saya gunakan agar dependency yang diperlukan ada, saya menggunakan command `sudo apt --fix-broken install`. Setelah itu, jalankan ulang command `sudo dpkg -i [nama-paket-VirtualBox]` untuk melakukan instalasi kembali.
<br><div style=width:500;>![ss](assets/3.jpeg)</div>

4. Setelah instalasi berhasil dan dapat membuka VirtualBox, unduhlah image distro yang akan digunakan dalam VirtualBo. Saya menggunakan distro debian yang dapat diakses pada laman `http://www.debian.org/download`.
<br><div style=width:500;>![ss](assets/4.jpeg)</div>

5. Jika image telah terunduh, tahap selanjutnya mulai pembuatan virtual machine dalam VirtualBox. Namun, saya mendapatkan eror saat ingin menekan tombol `new` dengan keterangan user belum masuk dalam grup vboxusers. Langkah yang saya ambil untuk menyelesaikan masalah tersebut adalah menggunakan command `sudo usermod -a -G vboxusers $USER` dengan keterangan `sudo` untuk mendapatkan hak akses administrator, `usermod` untuk modifikasi akun pengguna, `-a` untuk menambahkan pengguna ke grup tanpa mengeluarkan keanggotaan dalam grup lain, `-G vboxusers` untuk menentukan grup, dan `$USER` untuk menentukan akun pengguna yang sedang digunakan.
<br><div style=width:500;>![ss](assets/5.jpeg)</div>

6. Setelah menyelesaikan eror grup vboxusers, saya mendapatkan eror `VirtualBox Kernel driver not installed (rc=1908)` yang menandakan kernel driver antara tidak dimuat atau tidak diatur dengan benar. Solusi untuk memperbaiki eror ini menggunakan command `sudo /sbin/vboxconfig` untuk menjalankan konfigurasi vbox sebagai administrator.
<br><div style=width:500;>![ss](assets/6.jpeg)</div>

7. Saat menjalankan command `sudo /sbin/vboxconfig`, muncul eror yang menjelaskan bahwa linux headers tidak ada. Untuk memperbaiki eror, command yang digunakan adalah `sudo apt --fix-broken install` yang berfungsi untuk memperbaiki paket yang rusak dengan cara menginstal dependency yang hilang atau rusak.
<br><div style=width:500;>![ss](assets/7.jpeg)</div>

8. Jika sudah menjalankan command `sudo apt --fix-broken install`, maka command `sudo /sbin/vboxconfig` akan dapat dijalankan dan contoh hasil berhasil sebagai berikut:
<br><div style=width:500;>![ss](assets/8.jpeg)</div>

9. Langkah selanjutnya, membuka kembali Virtual Box untuk pembuatan virtual machine dengan image yang telah diunduh dengan ketentuan spesifikasi 2048 MB untuk memory, 2 core untuk cpu, dan 10 GB untuk penyimpanan. Setelah selesai melakukan konfigurasi, tekan next untuk memulai pembuatan virtual machine. Berikut contoh tampilan saat proses pembuatan virtual machine:
<br><div style=width:500;>![ss](assets/9.jpeg)</div>

10. Proses pembuatan virtual machine ini akan menggunakan waktu kurang lebih 30 sampai 60 menit. Berikut contoh tampilan saat proses telah selesai:
<br><div style=width:500;>![ss](assets/10.jpeg)</div>


## Kesimpulan
Dapat disimpulkan bahwa instalasi virtualbox dalam linux menggunakan bash shell berbeda dengan melakukan instalasi pada windows dikarenakan adanya beberapa kendala yang dialami oleh pengguna yang memerlukan melakukan beberapa langkah tambahan untuk memperbaiki kendala-kendala yang muncul.
