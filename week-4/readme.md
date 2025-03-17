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

# Sistem Nama Domain (DNS)

## **Apa itu DNS?**
DNS (Domain Name System) adalah punya prinsip yang sama dengan buku kuning telepon pada zaman dahulu. Bedanya, sekarang perangkat mencari nama domain di DNS untuk mendapatkan alamat IP. Bedanya, DNS bekerja secara otomatis dan sangat cepat, tanpa perlu dicari secara manual seperti buku kuning dulu.Manusia mengakses informasi online menggunakan **nama domain** (contoh: `nytimes.com` atau `espn.com`). Namun, peramban web berkomunikasi menggunakan **alamat IP**. DNS menerjemahkan nama domain menjadi alamat IP agar peramban dapat memuat sumber daya Internet. Setiap perangkat yang terhubung ke Internet memiliki **alamat IP unik**, yang digunakan oleh mesin lain untuk menemukannya. DNS memungkinkan pengguna **tidak perlu menghafal alamat IP**, seperti `192.168.1.1` (IPv4) atau `2400:cb00:2048:1::c629:d7a2` (IPv6).

---

## **Bagaimana DNS Bekerja?**
Proses **resolusi DNS** mengubah **hostname** (contoh: `www.example.com`) menjadi **alamat IP** (`192.168.1.1`). Ini seperti mencocokkan alamat rumah dengan nama pemiliknya. Saat pengguna mengakses sebuah situs web, terjadi proses pencarian alamat IP dari nama domain yang dimasukkan.
Pencarian DNS terjadi "di balik layar" dan melibatkan beberapa **komponen utama** dalam sistem DNS.
Gampangnya, nanti klien akan memasukkan domain yang ingin dikunjungi. DNS akan meemberikan IP dari domain tersebut sehingga klien bisa memulai komunikasi dengan server yang dituju.
---

## **Komponen Utama dalam DNS**

### 1. **DNS Recursor**
- Berperan seperti **pustakawan** yang mencari buku dalam perpustakaan.
- Bertugas menerima permintaan dari klien (misalnya, peramban web).
- Melakukan permintaan tambahan untuk mendapatkan alamat IP yang dicari.

### 2. **Root Nameserver**
- Langkah pertama dalam menerjemahkan nama domain ke alamat IP.
- Berfungsi seperti **indeks perpustakaan**, yang mengarahkan pencarian ke bagian lebih spesifik.

### 3. **TLD Nameserver (Top-Level Domain)**
- Bertanggung jawab atas ekstensi domain tertentu, misalnya:
  - `.com`, `.org`, `.net` (domain umum)
  - `.id`, `.jp`, `.uk` (domain berdasarkan negara)
- Berfungsi seperti **rak buku** yang hanya berisi kategori tertentu.

### 4. **Authoritative Nameserver**
- Langkah terakhir dalam pencarian DNS.
- Bisa dianggap seperti **kamus**, yang memberikan definisi atau informasi final.
- Jika memiliki catatan DNS yang dicari, server ini akan mengembalikan alamat IP ke **DNS Recursor**.

---

# Fitur DNS

DNS memiliki beberapa fitur, di antaranya:

- **Globally distributed**  
  DNS terdiri dari banyak server yang dikelola oleh berbagai operator di seluruh dunia, memastikan ketersediaan dan kecepatan akses.

- **Loosely coherent**  
  Meskipun terdistribusi, semua server tetap menjadi bagian dari satu sistem DNS global yang bekerja secara bersamaan.

- **Scalable**  
  Sistem ini dapat ditingkatkan dengan menambahkan lebih banyak server untuk menangani peningkatan jumlah permintaan pengguna.

- **Handal**  
  DNS adalah bagian penting dari infrastruktur internet, sehingga harus dirancang agar tetap berfungsi meskipun ada kegagalan pada beberapa server.

- **Dinamis**  
  Siapa pun dapat menambahkan domain dan catatan DNS tanpa menyebabkan gangguan pada sistem secara keseluruhan.

  ---
  # DNS sebagai Aplikasi Client-Server

- **DNS adalah Aplikasi Client-Server**  
  Artinya, klien (resolver) harus mengirim permintaan, dan server DNS akan merespons dengan informasi terkait catatan DNS.
- **Permintaan dan respons biasanya dikirim melalui **UDP port 53**.**
- **TCP port 53** digunakan dalam beberapa kasus untuk permintaan berukuran besar, seperti **Zone Transfer**.

  ![dns](assets/dns.png)
  Ini adalah contoh bagaimana DNS bekerja.
  - Klien akan menghubungi DNS rekursif sebagai  perantara.
  - DNS ini akan menanyakan kepada root server dimana alamat ini
  - Root tidak akan langsung memberikan fullnya, melainkan akan mengecek Top Level Domain (TLD)nya. Apakah itu TLD atau ccTLD (Country Code), seperti `.id`, `.au`, `.jp`, dan lainnya.
  - Kalau ccTLD, maka akan diarahkan ke Second Level Domain (SLD). SLD memiliki beberapa keuntungan, seperti:
    a. Organisasi yang lebih rapi → Memudahkan pengelolaan domain di suatu negara.
    b. Kontrol administratif → Setiap SLD bisa memiliki aturan pendaftaran berbeda.
  - Kalau itu Generic TLD, seperti `.com`, `.org`,  `.net`, a


