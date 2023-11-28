---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Intro - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Intro

Desdulianto

---

## Web Dinamis vs Web Statis

| Web Statis | Web Dinamis |
|------------|-------------|
| Konten statis, jarang berubah | Konten dinamis, di-generate oleh script/program dari sumber data (mis. database) |
| Interaksi terbatas, link ke halaman web lain | Interaksi lebih kaya, dapat menerima input dari pengguna melalui HTML form |
| Client side script (Javascript) | Client dan Server side script (Javascript dan PHP)

---

## Web Dinamis vs Aplikasi Web

| Web Dinamis | Aplikasi Web |
|-------------|--------------|
| Fungsi utama menampilkan konten | Fungsi utama menjalankan logic bisnis, misalnya untuk transaksi *e-commerce*, *e-banking*, forum, dll |
| Input user minimal: navigasi dan query, dll | Input user lebih kompleks: transfer, posting, membuat transaksi belanja, dll |

Aplikasi web merupakan alternatif untuk deliver aplikasi ke user (selain desktop dan mobile)

---

## Static Site Generator (SSG)

* Hybrid antara web statis dan web dinamis
* Konten web SSG digenerate dari sumber data menjadi konten statis (HTML, CSS, Javascript)
* Proses di sisi server hanya pada saat men-generate konten
* Relatif lebih aman dan cepat karena tidak ada logic yang dijalankan di sisi server
* Dikenal dengan Jamstack [https://jamstack.org/](https://jamstack.org/)

---

## Client Side Scripting

* Logic program yang dijalankan pada browser user (client side)
* Perlu penanganan khusus untuk mendapatkan hasil yang seragam karena runtime yang berbeda-beda (chrome, firefox, safari, edge, dll)
* Bahasa yang populer: Javascript

---

## Server Side Scripting

* Logic program dijalankan pada sisi server (server side)
* Dieksekusi oleh web server melalui [common gateway interface (CGI)](https://en.wikipedia.org/wiki/Common_Gateway_Interface)
* Banyak pilihan bahasa: PHP, Perl, Ruby, Python, Go, Javascript, dll

---

## Kenapa menggunakan PHP

* Dikembangkan untuk pengembangan web
* Relatif mudah untuk di-*hosting*
* Banyak materi pembelajaran dan komunitas
* Rilis terbaru terutama sejak versi 8 memiliki banyak fitur bahasa mengikuti trend teknologi bahasa pemrogramn modern

---

## Sejarah PHP

* Dikembangkan oleh Rasmus Lerdorf, sekitar tahun 1994
* Populer digunakan untuk membuat web dinamis
* Server side scripting, script PHP dieksekusi di sisi server dan outputnya dikirim ke client
* Bahasa *intepreted*
* Website resmi [https://www.php.net/](https://www.php.net/)

---

## Komponen Software Web PHP

* *Interpreter* PHP, untuk menjalankan script PHP
* Web server, untuk melayani request halaman web
* Data store, untuk menyimpan data atau konten yang akan diolah untuk menjadi tampilan web
* Stack sofware yang populer untuk menghosting web PHP: LAMP (Linux, Apache, Mysql, PHP)

---

## Stack LAMP

* Linux, sistem operasi untuk menghosting web server, PHP dan database server
* Web server Apache untuk melayani request HTTP
* *Intepreter* PHP untuk mengeksekusi script PHP
  * Biasanya jika dipasang bersama Apache, script PHP akan dieksekusi melalui module PHP
* Mysql, database server untuk menyimpan data secara relasional

---

## Stack alternative

* Web server Nginx
* php-fpm [https://php-fpm.org/](https://php-fpm.org/), server FastCGI untuk script PHP, biasanya disandingkan dengan Nginx atau menggantikan module PHP di Apache (mod_php)
* Database server Postgresql atau menggunakan library database Sqlite3 (tidak memerlukan server khusus)

---

## XAMPP

* Menyediakan software-software yang dibutuhkan untuk menghosting web PHP
* Memudahkan developer untuk memasang seluruh software dependensi dengan hanya melalui satu program installer
* Download di [https://www.apachefriends.org/](https://www.apachefriends.org/)

---

## Script PHP dari Command Line

* Ketik code PHP dengan text editor, simpan dengan nama file berekstensi .php
* Jalankan script PHP dengan memanggil *interpreter* PHP:

  ```sh
  php <script.php>
  ```

* Output biasanya ditampilkan pada terminal/console

---

## Script PHP sebagai bagian dari halaman web

* Ketik code PHP dengan text editor, simpan dengan nama file berekstensi .php
* File script PHP harus disimpan pada directory tertentu supaya dapat ditemukan dan dieksekusi oleh web server
  * XAMPP di Windows, biasanya di `c:\xampp\htdocs`
  * Di linux biasanya di `/var/www/html`
* Melihat halaman web melalui browser, `http://localhost/index.php` (sesuaikan dengan nama file PHP dan port number jika dibutuhkan)
