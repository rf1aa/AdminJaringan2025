 ufadi# Daftar Isi
- [Chapter 4: Prosess Control](#chapter-4-process_control)
- [Chapter 2: Proses dan Thread](#chapter-2-proses-dan-thread)
- [Chapter 3: Manajemen Memori](#chapter-3-manajemen-memori)

# Chapter 4: Process Control

# Komponen Proses

Sebuah proses terdiri dari **address space** dan **struktur data dalam kernel**.

## Address Space
Address space adalah sekumpulan halaman memori yang telah ditandai oleh kernel untuk digunakan oleh proses. (Halaman adalah unit pengelolaan memori, biasanya berukuran 4KB atau 8KB). Halaman ini digunakan untuk menyimpan kode, data, dan stack proses.

## Struktur Data dalam Kernel
Struktur data dalam kernel menyimpan informasi tentang status proses, prioritasnya, parameter penjadwalan, dan lainnya.

Misalnya, proses sebagai wadah yang berisi sumber daya yang dikelola oleh kernel untuk program yang sedang berjalan. Sumber daya ini meliputi:
- Halaman memori yang menyimpan kode dan data program.
- File descriptor yang merujuk ke file yang sedang dibuka.
- Atribut lain yang mendeskripsikan status proses.

Kernel menyimpan berbagai informasi tentang setiap proses, seperti:
- Peta ruang alamat proses.
- Status proses (running, sleeping, etc).
- Prioritas proses.
- Informasi sumber daya yang digunakan (CPU, memori, dll.).
- File dan port jaringan yang dibuka oleh proses.
- Signal mask (sinyal yang sedang diblokir).
- Process owner (UID pengguna yang menjalankan proses).

## Thread
**Thread** adalah konteks eksekusi dalam suatu proses. Sebuah proses dapat memiliki beberapa thread, yang semuanya berbagi address space dan sumber daya lainnya. Thread digunakan untuk menjalankan tugas secara paralel dalam suatu proses dan sering disebut sebagai **proses ringan (lightweight process)** karena lebih murah untuk dibuat dan dihancurkan dibandingkan proses utama (**heavyweight process**).

### Contoh Thread
Server web akan mendengarkan koneksi yang masuk. Untuk setiap request, server membuat thread baru untuk menanganinya. Setiap thread menangani satu permintaan, tetapi server secara keseluruhan bisa menangani banyak permintaan sekaligus karena memiliki banyak thread. Dalam kasus ini:
- **Server web adalah proses.**
- **Setiap thread adalah konteks eksekusi dalam proses tersebut.**

## PID: Process ID
Setiap proses memiliki **Process ID (PID)** unik, yang merupakan angka yang diberikan oleh kernel saat proses dibuat. PID digunakan dalam berbagai pemanggilan sistem, misalnya untuk mengirim sinyal ke proses.

### Namespaces & PID
Namespace proses memungkinkan proses yang berbeda memiliki PID yang sama, terutama dalam lingkungan container. Container adalah lingkungan terisolasi yang memiliki tampilan sistemnya sendiri, memungkinkan beberapa instance aplikasi berjalan dalam lingkungan terpisah pada sistem yang sama.

## PPID: Parent Process ID
Setiap proses memiliki proses induk (**parent process**) yang menciptakannya. **Parent Process ID (PPID)** adalah PID dari proses induk tersebut. PPID digunakan dalam berbagai pemanggilan sistem, misalnya untuk mengirim sinyal ke proses induk.

## UID & EUID: User ID & Effective User ID
- **User ID (UID)** → ID pengguna yang menjalankan proses.
- **Effective User ID (EUID)** → ID yang digunakan proses untuk menentukan hak akses terhadap sumber daya seperti file atau port jaringan.

EUID memungkinkan suatu proses berjalan dengan hak akses yang lebih tinggi (misalnya, program dengan setuid dapat dijalankan oleh pengguna biasa tetapi memiliki izin root).

## Siklus Hidup Proses
Untuk membuat proses baru, sebuah proses menyalin dirinya sendiri menggunakan pemanggilan sistem **fork()**.
- **fork()** membuat salinan dari proses asli yang hampir identik dengan induknya.
- Proses baru memiliki PID yang berbeda dan informasi akuntansi sendiri.

> **Note:** Di Linux, sistem sebenarnya menggunakan **clone()**, yang merupakan versi lebih canggih dari fork() dan mendukung pembuatan thread. Perintah **fork()** masih ada di kernel tetapi dipetakan ke **clone()** untuk kompatibilitas.

## Proses Utama Saat Booting
Saat sistem dinyalakan, kernel secara otomatis membuat beberapa proses, salah satunya adalah **init** atau **systemd**, yang selalu memiliki **PID 1**.
- **init/systemd** menjalankan skrip startup sistem dan menjadi induk dari semua proses lain di sistem.
- Semua proses yang tidak dibuat langsung oleh kernel adalah turunan dari **init/systemd**.

## Sinyal (Signals)
Sinyal adalah cara untuk mengirim notifikasi ke proses tentang suatu kejadian.

Terdapat sekitar **30 jenis sinyal**, yang digunakan dalam berbagai cara:
- **Proses ke proses** → Sebagai bentuk komunikasi antar proses.
- **Dari terminal** → Untuk menghentikan, membunuh, atau menunda proses saat pengguna menekan tombol tertentu (misalnya Ctrl+C).
- **Dari administrator** → Dengan perintah **kill** untuk menghentikan atau mengontrol proses.
- **Dari kernel** → Saat terjadi pelanggaran aturan, misalnya pembagian dengan nol (**SIGFPE**).
- **Dari kernel ke proses** → Untuk memberitahu kejadian seperti kematian proses anak atau tersedianya data pada saluran I/O.

Sinyal adalah metode utama untuk mengontrol dan mengelola proses dalam sistem operasi UNIX/Linux.

gambarsignal

bash
kill [-signal] pid

