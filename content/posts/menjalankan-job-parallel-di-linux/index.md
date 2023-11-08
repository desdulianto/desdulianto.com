---
title: "Menjalankan banyak job secara parallel di Linux"
date: 2016-01-26T00:00:00+07:00
description: "Menjalankan banyak job secara parallel di linux"
tags: ['shell', 'sysadmin', 'linux', 'unix']
---
Memproses banyak file menggunakan `for` memiliki kelemahan yaitu job akan dieksekusi secara serial. Tentunya potensi komputer tidak terpakai secara maksimal, karena job akan dieksekusi satu persatu bahkan pada komputer dengan processor multicore. Untung di Linux ada satu *tool* yang dapat digunakan untuk menjalankan banyak job secara paralel.

GNU Parallel adalah sebuah *tool command line* yang dapat mengeksekusi banyak job sekaligus secara paralel. Dengan demikian tentunya potensi komputer akan terpakai dengan maksimal (dan saya bisa menyelesaikan pekerjaan dengan lebih cepat).

Pada bahasan kali ini saya menggunakan 177 file gambar produk yang diambil dari kamera digital yang akan dipasangkan pada aplikasi program stock. Karena file gambar kamera digital berukuran besar, maka file gambar ini akan di-*resize* menjadi ukuran 800x600.

Pertama saya akan mengukur berapa lama proses resize berjalan dengan menggunakan perintah `for` biasa.

    time for i in *.JPG; do convert -resize 800x600 $i hasil/$i; done
    real	1m45.806s
    user	4m35.264s
    sys	    0m10.168s

Hasilnya adalah 1 menit 45 detik untuk me-*resize* 177 file gambar dari ukuran 4752x3168 menjadi 800x600. Beban system selama proses:

    load average: 2.51, 2.41, 1.71

Proses yang sama dengan menggunakan `parallel`

    time parallel convert -resize 800x600 {} hasil/{} ::: *.JPG

    real	1m25.298s
    user	5m5.924s
    sys	    0m12.464s

Beban system selama proses:

    load average: 8.09, 3.59, 1.81

Proses yang dijalankan dengan `parallel` lebih cepat 20.507 detik dibandingkan dengan yang jalankan secara serial. Tetapi beban system juga otomatis meningkat dari 2.51 menjadi 8.09.
