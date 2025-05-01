
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

# Laporan Konfigurasi NAT dan Firewall pada VM1
Percobaan ini mengonfigurasi VM1 sebagai **gateway NAT** dan penerapan **firewall** menggunakan `iptables`, agar komputer dalam jaringan privat dapat mengakses internet melalui VM1 dengan aman. 
Selain itu, dilakukan instalasi layanan tambahan **NTP** untuk sinkronisasi waktu, serta **Samba** untuk berbagi file di jaringan lokal.
Tujuannya, yaitu
- Mengaktifkan NAT (Network Address Translation) pada VM1.
- Menerapkan dan menyimpan aturan firewall secara permanen.
- Membatasi akses ke VM1 hanya untuk koneksi tertentu (misalnya SSH).
- Menyediakan layanan waktu (NTP) agar sistem selalu sinkron.
- Menyediakan layanan file sharing lokal (Samba) antar perangkat.

---

## Daftar Isi

1. [Konfigurasi Gateway & Firewall pada VM1](#1-konfigurasi-gateway--firewall-pada-vm1)
2. [Instalasi dan Konfigurasi NTP](#2-instalasi-dan-konfigurasi-ntp)
3. [Instalasi dan Konfigurasi Samba](#3-instalasi-dan-konfigurasi-samba)
4. [Instalasi dan Konfigurasi BIND9](#4-instalasi-dan-konfigurasi-bind9)

## 1. Konfigurasi Gateway & Firewall pada VM1

### Buka dan sesuaikan file interface jaringan:

```bash
nano -l -w /etc/network/interfaces
```
![Screenshot](assets/1.png)

Buat IP statis untuk jaringan lokal, misalnya
Contoh konfigurasi:
```bash
iface enp0s8 inet static
    address 192.168.200.1
    network 192.168.200.0
    netmask 255.255.255.0
    broadcast 192.168.200.255
    dns-nameservers 1.1.1.1
```

![Screenshot](assets/1-ss.png)

---

### Aktifkan IP Forwarding

Edit file konfigurasi sistem:

```bash
nano -l -w /etc/sysctl.conf
```
![Screenshot](assets/2.png)

Uncomment bagian
```conf
net.ipv4.ip_forward=1
```

![Screenshot](assets/2-ss.png)
Simpan dan jalankan:

```bash
sysctl -p
```

![Screenshot IP Forwarding](assets/2-sys.png)

---

### Atur NAT dengan iptables

Tambahkan aturan NAT dengan:
```bash
nano -l -w /etc/iptables/rules.v4
```
![Screenshot IP Forwarding](assets/3.png)

```bash
sudo nano -l -w /etc/iptables/rules.v4
```

![Screenshot NAT](assets/3.png)

Ubah isinya menjadi seperti berikut:

![Screenshot NAT](assets/3-ss.png)


---

### Instal iptables-persistent

Untuk menyimpan aturan iptables secara permanen:

```bash

apt-get install iptables-persistent
apt-get install iptables iptables-persistent
```
![Screenshot NAT](assets/7.png)

![Screenshot NAT](assets/8.png)
Jawab `Yes` saat diminta menyimpan aturan saat ini.
Untuk saya, karena sudah saya kerjakan maka outputnya sudah terinstal

![Screenshot Instalasi iptables-persistent](assets/4.png)

---

### Restore iptables dari File

Untuk menerapkan kembali aturan dari file:

```bash
iptables-restore < /etc/iptables/rules.v4
```

![Screenshot Restore iptables](assets/4.png)

---

### Reboot dan Verifikasi

Reboot sistem agar semua pengaturan aktif saat boot:

```bash
reboot
```
![Screenshot NAT](assets/8-1.png)

Setelah reboot, verifikasi bahwa iptables masih aktif:

```bash
iptables -L
```

![Screenshot Reboot](assets/-L.png)

---

Pengaturan di VM 2

Di VM 2, set '/etc/network/interfaces' menjadi:

![Screenshot Reboot](assets/9.png)

---

Ping dari masing-masing VM

![Screenshot NAT](assets/10-1.png)

![Screenshot NAT](assets/10-2.png)

Tambahkan konfigurasi di bawah di dalam VM 2  untuk menghubungkan internet dari VM 1

![Screenshot NAT](assets/11.png)

---

## 2. Instalasi dan Konfigurasi NTP

### Untuk sinkronisasi waktu otomatis, install `ntp`:

```bash
sudo apt update
sudo apt install ntp -y
```
![Screenshot NAT](assets/21.png)

### Cek status layanan:
```bash
sudo systemctl status ntp
```

![Screenshot NAT](assets/22.png)

### Verifikasi sinkronisasi:
```bash
ntpq -p
```

![Screenshot NAT](assets/23.png)

---

## 3. Instalasi dan Konfigurasi Samba

### Instalasi Samba di VM1 (Server)

```bash
apt update
apt install samba samba-client smbclient -y
```

![Screenshot NAT](assets/31.png)

###  Membuat Folder Share dan File Baru

```bash
mkdir /home/share
chmod 777 /home/share
```

![Screenshot NAT](assets/32.png)


###  Edit Konfigurasi Samba

Edit file konfigurasi utama:

```bash
nano /etc/samba/smb.conf
```

Tambahkan/ubah bagian-bagian berikut:

```conf

unix charset = UTF-8
dos charset = CP932

map to guest = Bad User

# Di akhir, buat direktori baru, yaitu:
[Share]
    path = /home/share
    writable = yes
    guest ok = yes
    guest only = yes
    create mode = 0777
    directory mode = 0777
```
![Screenshot NAT](assets/34.png)


### Jalankan dan Aktifkan Samba

```bash
systemctl restart smbd nmbd
systemctl enable smbd nmbd
```

![Screenshot NAT](assets/35.png)

---

###  Pengujian Akses dari VM2 (Client)

#### Instalasi Tools 

```bash
apt install smbclient cifs-utils -y
```

![Screenshot NAT](assets/36.png)

---

#### Cek direktori Share

```bash
smbclient -L //192.168.200.1 -N
```
![Screenshot NAT](assets/37.png)


#### Akses Folder Share

```bash
smbclient //192.168.200.1/Share -N
```

![Screenshot NAT](assets/38.png)

Masukkan sebuah file, misalnya:

![Screenshot NAT](assets/39.png)


#### Akses File dari VM 1

Cek file `Dhiya.txt` yang sebelumnya sudah di-`put` dari VM 2

![Screenshot NAT](assets/40.png)


## 4. Instalasi dan Konfigurasi BIND9 (DNS Server)

### Instalasi BIND di VM1 (Server DNS)

```bash
apt update
apt -y install bind9 bind9utils
```

![Screenshot NAT](assets/51.png)

### Konfigurasi Dasar BIND

#### Buat file

```bash
nano /etc/bind/named.conf

```
Tambahkan:
```conf
include "/etc/bind/named.conf.internal-zones";
```

![Screenshot NAT](assets/52.png)


#### Edit opsi konfigurasi:
```bash
nano /etc/bind/named.conf.options
```
Tambahkan ACL dan pengaturan:
```conf
acl internal-network {
    192.168.200.0/24;
};
```

![Screenshot NAT](assets/53.png)

#### Buat file zona internal:
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

![Screenshot NAT](assets/54.png)

---

#### Gunakan hanya IPv4:
```bash
nano /etc/default/named
```
Tambahkan:
```conf
OPTIONS="-u bind -4"
```

![Screenshot NAT](assets/55.png)


---

### Konfigurasi Zona DNS

#### File zona langsung (A record):
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


![Screenshot NAT](assets/56.png)


#### File zona reverse (PTR record):
```bash
nano /etc/bind/200.168.192.db
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

1   IN  PTR     ns1.praktikum.local.
2   IN  PTR     www.praktikum.local.
```


![Screenshot NAT](assets/57.png)

---

### Jalankan dan Verifikasi

```bash
systemctl restart named
systemctl enable named
```


![Screenshot NAT](assets/58.png)


### Pengujian DNS

#### Atur VM2 agar menggunakan DNS dari VM1
```bash
nano /etc/network/interfaces
```
Edit bagian:

![Screenshot NAT](assets/59.png)

#### Cek Resolusi Nama
```bash
dig ns1.praktikum.local
```
![Screenshot NAT](assets/60.png)

---

## Kesimpulan

Dengan langkah-langkah di atas:
- VM1 digunakan untuk gateway antara VM2 dengan internat melalui Internal Network, tanpa VM 2 menggunakan bridged adapter ataupun NAT
- Koneksi keluar dari jaringan privat diizinkan melalui `MASQUERADE`.
- Aturan iptables berhasil diamankan dengan `iptables-persistent`.
- Firewall membatasi koneksi hanya pada port yang dibutuhkan (SSH), dan melindungi dari akses luar yang tidak sah.

---

