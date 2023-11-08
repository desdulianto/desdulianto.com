---
title: "Arti real, user dan sys dari output perintah time di Linux"
date: 2015-10-01T00:00:00+07:00
description: "Penjelasan mengenai tampilan real, user dan sys pada output perintah time di linux"
tags: ['shell', 'linux', 'unix', 'sysadmin', 'text']
---
Perintah `time` di Linux digunakan untuk mengukur berapa lama waktu eksekusi sebuah perintah. Hasil dari perintah time adalah waktu `real`, `user` dan `sys`. Artikel ini membahas arti dari masing-masing output `time` tersebut.

Contoh eksekusi time:

    time for i in `seq 1 1000000`; do echo $i > /dev/null; done

    real	0m10.178s
    user	0m7.868s
    sys	 0m2.272s

Perintah di atas menghitung berapa lama waktu eksekusi ``for i in `seq 1 1000000`; do echo $i > /dev/null; done`` (cetak 1 sampai 1000000 ke device *null*).

* `real`, lama waktu dari job dijalankan sampai proses selesai (user kembali ke *shell prompt*). Waktu ini adalah waktu `user` + `sys` + block (antrian proses, *i/o wait*).

    Waktu `real` - (`user` + `sys`) adalah waktu blocking. Apabila hasil waktu ini cukup tinggi berarti banyak waktu yang terbuang menunggu proses input dan output (*i/o wait*).
* `user`, waktu yang dihabiskan CPU untuk mengeksekusi kode di mode user (di luar kernel). 
* `sys`, waktu yang dihabiskan CPU untuk mengeksekusi kode di mode kernel (system call).

Waktu `user` dan `sys` kadang bisa lebih besar dari waktu `real` terutama untuk apabila banyak *thread* yang berjalan bersamaan. Pada kasus ini, waktu `user` dan `sys` adalah total waktu dari seluruh *thread* yang jalan.

    time for i in *.JPG; do convert -resize 800x600 $i t/$i; done

    real	0m19.802s
    user	0m52.848s
    sys	 0m2.280s

Happy coding!

Ref:
[What do 'real', 'user' and 'sys' mean in the output of time(1)?](http://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)
