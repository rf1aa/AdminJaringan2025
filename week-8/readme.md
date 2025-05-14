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

# Laporan Instalasi dan Konfigurasi DNS Server (BIND9)

Percobaan ini bertujuan untuk menginstal dan mengonfigurasi **BIND9** sebagai DNS server yang mengelola pemetaan nama domain ke alamat IP.
---

## Daftar Isi

1) [Definisi BIND9 (DNS Server)](#1-definisi-bind9)  
2) [Instalasi dan Konfigurasi BIND9 (Web Server)](#2-instalasi-dan-konfigurasi-bind9-dns-server)

---

![screenshot](assets/topologi.png)
## 1) Definisi BIND9 (DNS Server)


### **Apa itu BIND9 dan Fungsinya**

**BIND9 (Berkeley Internet Name Domain versi 9)** adalah **perangkat lunak server DNS (Domain Name System)** yang paling umum digunakan di sistem berbasis UNIX/Linux, termasuk Debian. Fungsinya adalah untuk **menerjemahkan nama domain** seperti `example.com` menjadi alamat IP seperti `192.0.2.1`, sehingga komputer dapat menemukan dan berkomunikasi satu sama lain di jaringan.

#### **Fungsi Utama BIND9**

* **DNS Resolver**: Menjawab permintaan DNS dari klien (misalnya browser).
* **Authoritative DNS Server**: Menyediakan informasi DNS untuk domain tertentu.
* **Caching**: Menyimpan hasil pencarian DNS sementara agar lebih cepat diakses berikutnya.
* **Zone Management**: Mengelola zona DNS, termasuk domain utama dan subdomain.

#### **Komponen Penting BIND9**

* **named**: Layanan utama BIND (daemon) yang berjalan di latar belakang.
* **named.conf**: File konfigurasi utama BIND, di mana zona dan opsi DNS diatur.
* **Zona file**: File yang berisi informasi pemetaan nama domain ke IP (dan sebaliknya).

#### **Manfaat Penggunaan BIND9**

* Menyediakan layanan DNS lokal dalam jaringan internal.
* Mempercepat akses internet dengan caching DNS.
* Dapat digunakan untuk membuat DNS server publik.

---

## 2) Instalasi dan Konfigurasi BIND9 (Web Server)


### a) Instalasi BIND9

```bash
apt update
apt -y install bind9 bind9utils
```

---

### b) Tambahkan include zona internal

```bash
nano /etc/bind/named.conf
```

Tambahkan:
```conf
include "/etc/bind/named.conf.internal-zones";
```

![screenshot](assets/bind-1.jpg)

---

### c) Tambahkan ACL dan konfigurasi opsi

```bash
nano /etc/bind/named.conf.options
```

Tambahkan:
```conf
acl internal-network {
    192.168.2.0/24;
};

options {
        directory "/var/cache/bind";
        allow-query { localhost; internal-network; };
        allow-transfer { localhost; };
        recursion yes;
        dnssec-validation auto;
        listen-on-v6 { any; };
};
```

![screenshot](assets/bind-2.jpg)

---

### d) Buat file zona internal

```bash
nano /etc/bind/named.conf.internal-zones
```

Isi dengan:
```conf
zone "kelompok2.home" IN {
    type master;
    file "/etc/bind/kelompok2.home.lan";
    allow-update { none; };
};

zone "2.168.192.in-addr.arpa" IN {
    type master;
    file "/etc/bind/2.168.192.db";
    allow-update { none; };
};
```

![screenshot](assets/bind-3.jpg)

---

### e) Gunakan hanya IPv4

```bash
nano /etc/default/named
```

Tambahkan:
```conf
OPTIONS="-u bind -4"
```

![screenshot](assets/bind-4.jpg)

---

### f) Konfigurasi zona forward (A Record)

```bash
nano /etc/bind/kelompok2.home.lan
```

Isi:
```
$TTL 86400
@   IN  SOA     kelompok2.home. root.kelompok2.home. (
        2025042401  ; Serial
        3600        ; Refresh
        1800        ; Retry
        604800      ; Expire
        86400       ; Minimum TTL
)

    IN  NS      ns1.kelompok2.home.
    IN  A       192.168.2.1
    IN  MX 10   ns1.kelompok2.home.

ns1 IN  A       192.168.2.10
www IN  CNAME ns
```

![screenshot](assets/bind-5.jpg)

---

### g) Konfigurasi zona reverse (PTR Record)

```bash
nano /etc/bind/2.168.192.db
```

Isi:
```dns
$TTL 86400
@   IN  SOA     kelompok2.home. root.kelompok2.home. (
        2025042401  ; Serial
        3600        ; Refresh
        1800        ; Retry
        604800      ; Expire
        86400       ; Minimum TTL
)

    IN  NS      ns.kelompok2.home.

10   IN  PTR     ns.kelompok2.home.
10   IN  PTR     www.kelompok2.home.
```

![screenshot](assets/bind-6.jpg)

---

### h) Jalankan dan aktifkan layanan

```bash
systemctl restart named
systemctl enable named
```

---

### i) Menambah nameserver

1. Atur `nameserver` pada VM client:
```bash
nano /etc/resolv.conf
```

Tambahkan:
```conf
dns-nameserver 192.168.2.1
```

!(screenshot)[assets/bind-7.jpg]

2. Uji dengan `dig`:
```bash
dig ns.kelompok2.home.
```

![screenshot](assets/bind-8.jpg)

---

**Selesai.**
```
