---
title: "Menyalin File Dari Komputer Remote Dengan ssh"
date: 2015-12-12T00:00:00+07:00
description: "Menyalin file dari komputer remote menggunakan ssh"
tags: ['shell', 'unix', 'linux', 'sysadmin']
---
Menyalin file dari komputer remote membutuhkan waktu yang lama, terutama apabila jumlah file yang disalin banyak dan berukuran kecil. Untuk meng-efisienkan proses penyalinan file, dapat digunakan perintah `tar` yang dijalankan melalui `ssh`.

`tar` menggabungkan kumpulan file menjadi sebuah file arsip besar. Menyalin satu file besar akan lebih cepat dibandingkan dengan menyalin banyak file berukuran kecil. Hal ini dikarenakan banyak waktu yang habis digunakan pada proses *filesystem* untuk memproses file-file yang berbeda.

    ssh user@komputer-remote.com "cd /home/user; tar cjf - directory-yang-akan-disalin" | tar xjvf - -C /directory-tujuan

Perintah di atas terdiri dari dua bagian, yaitu:

1. Membuka koneksi `ssh` (secure shell) ke komputer remote, dan kemudian melakukan `archiving` terhadap file yang akan disalin menggunakan perintah `tar`.
`tar cjf - directory-yang-akan-disalin` membuat `archive` file yang akan disalin.
    * Opsi `c` untuk membentuk `archive`  baru.
    * Opsi `j` untuk mengkompress `archive` dengan format `bzip2` (gunakan `z` untuk format `gzip` atau hilangkan opsi ini untuk tidak melakukan kompresi).
    * Opsi `f -` meminta `tar` menuliskan hasil `archive` ke *standard output* untuk dikirim melalui *pipe* (`|`).
2. Meng-*extract* hasil `archive` pada perintah pertama ke `directory-tujuan`. Input perintah ini adalah `archive` dari perintah pertama yang dikirim melalui *pipe* (`|`).
    * Opsi `x` melakukan *extract*.
    * Opsi `v` menampilkan proses *extract*.
    * Opsi `j` melakukan dekompresi terhadap `archive` dengan format `bzip2`.
    * Opsi `f -` file input dibaca dari *standard input*.
    * Opsi `-C /directory-tujuan` lokasi tujuan untuk melakukan *extract* `archive`.

Happy hacking!
