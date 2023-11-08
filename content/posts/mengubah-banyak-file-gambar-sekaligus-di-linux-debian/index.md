---
title: "Mengubah Banyak File Gambar Sekaligus di Linux Debian"
date: 2015-09-02T00:00:00+07:00
description: "Tip untuk memanipulasi banyak file gambar sekalian menggunakan command line di linux"
tags: ['code', 'shell', 'text', 'unix', 'linux', 'sysadmin']
---
Saya sering dikirimi file gambar dengan resolusi besar atau dengan format mentah (RAW dan BMP) untuk ditampilkan pada halaman web. Gambar resolusi besar memperlambat loading web, begitu juga halnya dengan file gambar RAW/BMP yang berukuran besar. Oleh karena itu, perlu dilakukan *resize* atau perubahan format file.

Melakukan *resize*/konversi format dengan aplikasi Desktop akan memakan waktu, terutama kalau ukuran file besar. Untung di Linux hal ini bisa dikerjakan dengan sangat mudah tanpa harus menghabiskan banyak waktu.

## convert

`convert` adalah bagian dari paket aplikasi `imagemagick`. Dengan menggunakan *tool* ini, kita dapat melakukan banyak pekerjaan editing file gambar yang biasanya dilakukan menggunakan aplikasi berbasis GUI.

Pada artikel ini, kita akan membahas dua fungsi yang paling sering saya gunakan yaitu untuk mengubah resolusi gambar dan melakukan konversi format file gambar.

### konversi

Untuk melakukan konversi format file gambar:

    convert <file asli.ext> <file hasil.ext>

Format file gambar ditentukan oleh extensi file hasil. Misalnya untuk mengubah file `foto.bmp` menjadi `foto.jpg`:

    convert foto.bmp foto.jpg

### resize

Perintah untuk melakukan resize:

    convert -resize <X>x<Y> <file asli> <file hasil>

Nilai `X` dan `Y` adalah resolusi lebar (*width*) dan tinggi (*height*) file gambar. Misalnya untuk mengubah resolusi gambar `foto.jpg` menjadi 800x600 pixel:

    convert -resize 800x600 foto.jpg foto-1.jpg

## manipulasi banyak file

Salah satu fitur dari *shell* `bash` di Linux adalah kita dapat melakukan perulangan `for`.

    for <var> in <list>; do <perintah>; done

`for` akan mengulang `perintah` sebanyak item yang ada pada `list`. Pada setiap perulangan, item `list` akan disimpan sebagai variable `var` untuk digunakan pada `perintah`.

Misalnya, untuk mengubah seluruh file `bmp` menjadi `jpg`:

    for i in *.bmp; do convert "$i" "`basename \"$i\" bmp`jpg"; done

Contoh di atas menggunakan variable `i` untuk me-refer ke masing-masing nama file `*.jpg`. Perintah `basename` digunakan untuk menghasilkan nama file tanpa ada nama directory dan tanpa ektensi file. Jadi misalnya `foto.bmp` akan menjadi `foto` yang kemudian digabungkan dengan extensi file yang kita inginkan.

Pakai cara yang sama untuk mengubah resolusi file gambar:

    for i in *.jpg; do convert -resize 800x600 "$i" "`basename \"$i\" jpg`-1.jpg"; done

Pada perintah ini kita mengubah resolusi file gambar menjadi 800x600 dan file gambar akan disimpan dengan menambahkan `-1` dibelakang nama file asli. File `foto.jpg`  akan menjadi `foto-1.jpg`.

Happy coding!
