---
title: "PHP MVC Web - PHP Entry Point"
date: 2024-02-21T15:50:00+07:00
description: "Membuat web MVC menggunakan PHP. Bagian pertama, meng-_forward_ request ke script _entry point_ PHP."
tags: ["code", "web", "php"]
series: ["PHP Web MVC"]
series_order: 1
---

Pada artikel ini kita akan membahas mengenai pengembangan
web dengan pattern MVC menggunakan PHP. Kita tidak akan
menggunakan framework tapi akan menulis sendiri seluruh
komponen yang dibutuhkan. _Code_ akan kita tulis di-inspirasi
oleh [framework Laravel](https://laravel.com/).

Contoh _code_ dari artikel ini bisa dilihat di [todo-project-php](https://github.com/des-learning/todo-project-php).

Kita akan mengeksplorasi beberapa konsep yang umum ditemukan pada
web framework seperti _router_, _controller_, _view_, _controller_ dan _middleware_.

> _Disclaimer_, _code_ yang akan kita tulis belum production ready. Contoh
> _code_ hanya digunakan untuk tujuan pembelajaran konsep MVC pada pengembangan
> web.

Pada seri pertama ini kita akan membahas mengenai _entry point_. Oke, mari kita mulai!

## PHP Entry Point

Hal pertama yang akan kita kerjakan adalah mem-_forward_ seluruh request
ke script PHP ke satu script yang menjadi _entry point_.

Secara tradisional, model eksekusi web PHP adalah sebagai berikut:

1. user mengirimkan request ke web server
2. web server memproses request dan memilih script PHP yang akan dijalankan
   sesuai dengan URL request. Biasanya URL request berisikan nama script PHP yang
   akan dijalankan misalnya: `http://localhost/index.php` atau `http://localhost/hello.php`
3. web server mengeksekusi script PHP dengan mengirimkan request ke script
4. Script PHP memproses request, menghasilkan response (atau error) dan dikirimkan
   kembali ke web server
5. Web server mengirimkan response ke user

Berdasarkan model eksekusi di atas, user dapat mengakses sembarang file script
PHP yang di-_hosting_. Implikasinya kita perlu meng-_require_ seluruh code pada
seluruh file script PHP. Selain tidak _scalable_, hal ini akan mempolusi code yang
akan dikembangkan.

Untuk mengatasi masalah ini, kita akan membuat sebuah script PHP yang menjadi
_entry point_. Seluruh request akan diarahkan ke script _entry point_ ini dan script
ini yang akan meng-_require_ script PHP lain sesuai dengan path logic yang dilalui
pada pemrosesan request.

User tidak lagi bisa mengakses file script PHP secara _random_. Code yang kita
kembangkan menjadi lebih terstruktur dan rapi.

Cara meng-_forward_ request ke script PHP ini tergantung pada web server yang digunakan.

Untuk contoh code yang akan kita kerjakan, kita akan letakkan _source code_ di `/todo/`.
Untuk web server Apache, kita akan menggunakan `RewriteEngine` untuk mem-_forward_
request ke satu file script PHP.

`.htaccess`

```sh
RewriteEngine On
RewriteBase /todo/
RewriteRule ^index\\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /todo/index.php [L]
```

File `.htaccess` in diletakkan di _directory_ yang menghosting file script PHP.
Web server Apache akan membaca konfigurasi dari `.htaccess` dan
meng-_apply rule_ `RewriteEngine` untuk meng-_forward_ seluruh request
ke script `/todo/index.php`.

File `index.php` nantinya akan menerima seluruh request dari user dan
memanggil code PHP yang lain sesuai dengan kebutuhan. `index.php` ini
adalah _entry point_ kita.

Contoh, untuk saat sekarang kita cuma tampilkan `Hello World`.

`index.php`

```php
<?php

echo "Hello World";
```

Sekarang, ketika user mengakses `http://localhost/todo` semua request akan
diproses oleh `index.php` sebagai script _entry point_.

Berikut ini isi directory di project kita pada seri pertama ini:

```sh
.
├── .htaccess
└── index.php
```
