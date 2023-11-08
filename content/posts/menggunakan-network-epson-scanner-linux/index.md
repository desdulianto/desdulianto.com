---
title: "Menggunakan Network Scanner Epson di Linux (Debian)"
date: 2016-11-28T00:00:00+07:00
description: "Tip untuk mengkonfigurasi network scanner Epson pada Linux"
tags: ['shell', 'linux', 'sysadmin']
---
Kita akan memasang paket `iscan` yang tersedia dari Epson. Ada 2 paket yang akan dipasang oleh script instalasi ini, yaitu driver `epkowa` dan program Image Scanner untuk melakukan scanning.

Berikut ini langkah-langkah untuk menginstall dan menggunakan Network Scanner Epson pada Linux.

1. Download paket iscan dari [driver epson untuk linux](http://support.epson.net/linux/en/iscan_c.html). Pilih paket yang sesuai dengan distribusi Linux yang terpasang.
2. Extract paket yang telah selesai di-download, misalnya: `tar zxf iscan-bundle-1.0.0.x64.deb.tar.gz`
3. Masuk ke directory hasil extract pada langkah 2, `cd iscan-bundle-1.0.0.x64.deb/`
4. Di dalam directory tersebut ada file `README.rst` yang berisikan petunjuk instalasi. Dan apabila sudah siap, jalankan script install `./install.sh`. Proses instalasi akan meminta password `root`.

Setelah paket terpasang dengan baik, berikutnya adalah lakukan konfigurasi `sane`. Driver yang dipasang oleh paket ini adalah `epkowa`. Langkah-langkah konfigurasi:

1. Masuk ke directory `/etc/sane.d`.
2. Edit file `epkowa.conf` dan tambahkan baris `net <ip-scanner>` misalnya `net 192.168.1.100` dan kemudian simpan.
3. Edit file `dll.conf`, tambahkan `epkowa` untuk mengaktifkan driver scanner.

Setelah selesai, silahkan coba scan menggunakan program Image Scanning yang juga telah dipasang pada proses instalasi di atas.

Happy hacking!
