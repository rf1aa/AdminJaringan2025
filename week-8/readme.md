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

Berikut adalah versi `README.md` yang diperbarui sesuai permintaan kamu:  
- Gunakan penomoran sederhana (1), 2), 3)...  
- Tambahkan placeholder gambar setelah setiap langkah percobaan.  
- Format markdown sudah siap untuk langsung dicopas.

---

```md
# Laporan Instalasi dan Konfigurasi DNS Server (BIND9) dan Web Server (Apache2)

Percobaan ini bertujuan untuk menginstal dan mengonfigurasi dua layanan utama pada sistem berbasis Linux:

- **BIND9** sebagai DNS server yang mengelola pemetaan nama domain ke alamat IP.
- **Apache2** sebagai web server untuk menyajikan halaman web.

---

## Daftar Isi

1) [Instalasi dan Konfigurasi BIND9 (DNS Server)](#1-instalasi-dan-konfigurasi-bind9-dns-server)  
2) [Instalasi dan Konfigurasi Apache2 (Web Server)](#2-instalasi-dan-konfigurasi-apache2-web-server)

---

## 1) Instalasi dan Konfigurasi BIND9 (DNS Server)

### a) Instalasi BIND9

```bash
apt update
apt -y install bind9 bind9utils
```

!(screenshot)[assets/bind-install.jpg]

---

### b) Tambahkan include zona internal

```bash
nano /etc/bind/named.conf
```

Tambahkan:
```conf
include "/etc/bind/named.conf.internal-zones";
```

!(screenshot)[assets/include-internal.jpg]

---

### c) Tambahkan ACL dan konfigurasi opsi

```bash
nano /etc/bind/named.conf.options
```

Tambahkan:
```conf
acl internal-network {
    192.168.200.0/24;
};
```

!(screenshot)[assets/acl-options.jpg]

---

### d) Buat file zona internal

```bash
nano /etc/bind/named.conf.internal-zones
```

Isi dengan:
```conf
zone "praktikum.local" IN {
    type master;
    file "/etc/bind/praktikum.local.db";
    allow-update { none; };
};

zone "200.168.192.in-addr.arpa" IN {
    type master;
    file "/etc/bind/200.168.192.db";
    allow-update { none; };
};
```

!(screenshot)[assets/internal-zones.jpg]

---

### e) Gunakan hanya IPv4

```bash
nano /etc/default/named
```

Tambahkan:
```conf
OPTIONS="-u bind -4"
```

!(screenshot)[assets/bind-ipv4.jpg]

---

### f) Konfigurasi zona forward (A Record)

```bash
nano /etc/bind/praktikum.local.db
```

Isi:
```dns
$TTL 86400
@   IN  SOA     ns1.praktikum.local. root.praktikum.local. (
        2025041501  ; Serial
        3600        ; Refresh
        1800        ; Retry
        604800      ; Expire
        86400       ; Minimum TTL
)

    IN  NS      ns1.praktikum.local.
    IN  A       192.168.200.1
    IN  MX 10   ns1.praktikum.local.

ns1 IN  A       192.168.200.1
www IN  A       192.168.200.2
```

!(screenshot)[assets/forward-zone.jpg]

---

### g) Konfigurasi zona reverse (PTR Record)

```bash
nano /etc/bind/200.168.192.db
```

Isi:
```dns
$TTL 86400
@   IN  SOA     ns1.praktikum.local. root.praktikum.local. (
        2025041501
        3600
        1800
        604800
        86400
)
    IN  NS      ns1.praktikum.local.

1   IN  PTR     ns1.praktikum.local.
2   IN  PTR     www.praktikum.local.
```

!(screenshot)[assets/reverse-zone.jpg]

---

### h) Jalankan dan aktifkan layanan

```bash
systemctl restart named
systemctl enable named
```

!(screenshot)[assets/named-start.jpg]

---

### i) Pengujian DNS

1. Atur `dns-nameservers` pada VM client:
```bash
nano /etc/network/interfaces
```

Tambahkan:
```conf
dns-nameservers 192.168.200.1
```

!(screenshot)[assets/interfaces-dns.jpg]

2. Uji dengan `dig`:
```bash
dig ns1.praktikum.local
```

!(screenshot)[assets/dig-test.jpg]

---

## 2) Instalasi dan Konfigurasi Apache2 (Web Server)

### a) Instalasi Apache2

```bash
apt -y install apache2
```

!(screenshot)[assets/apache-install.jpg]

---

### b) Ubah informasi header (security.conf)

```bash
nano /etc/apache2/conf-enabled/security.conf
```

Ubah baris:
```conf
ServerTokens Prod
```

!(screenshot)[assets/apache-security.jpg]

---

### c) Atur file index default

```bash
nano /etc/apache2/mods-enabled/dir.conf
```

Ubah baris:
```conf
DirectoryIndex index.html index.htm
```

!(screenshot)[assets/apache-dirconf.jpg]

---

### d) Tentukan nama server

```bash
nano /etc/apache2/apache2.conf
```

Tambahkan:
```conf
ServerName www.srv.world
```

!(screenshot)[assets/apache-conf.jpg]

---

### e) Ubah email admin virtual host

```bash
nano /etc/apache2/sites-enabled/000-default.conf
```

Ubah baris:
```conf
ServerAdmin webmaster@srv.world
```

!(screenshot)[assets/apache-vhost.jpg]

---

### f) Reload Apache

```bash
systemctl reload apache2
```

!(screenshot)[assets/apache-reload.jpg]

---

### g) Pengujian Web Server

1. Akses dari browser:
```
http://192.168.200.2
```

2. Jika DNS aktif:
```
http://www.praktikum.local
```

!(screenshot)[assets/apache-browser-test.jpg]

---

**Selesai.**
```

Kalau kamu mau langsung sekalian aku buatin file `.md` atau isi gambarnya nanti kamu isi sendiri, tinggal bilang aja.