---
title: "Memberikan urutan nomor pada nama file di Linux"
date: 2015-09-12T00:00:00+07:00
description: "Tip untuk penamaan file dengan menggunakan nomor urut di linux"
tags: ['code', 'shell', 'text', 'linux', 'unix', 'sysadmin']
---
Microsoft Windows memiliki fitur yang cukup powerful untuk melakukan perubahan terhadap banyak file. Dengan menyorot file yang akan diubah nama filenya, kemudian pilih perintah rename dan masukkan nama file baru, seluruh file yang disorot akan diubah nama filenya dengan menambahkan urutan nomor `(1)`, `(2)`, `(3)` dan seterusnya. Sayang di Linux, saya belum menemukan fitur ini di desktop manager.

Tetapi walaupun demikian, melakukan perubahan nama file terhadap banyak file sekaligus merupakan hal yang relatif gampang dilakukan pada *shell* di Linux. Pada pembahasan kali ini, kita akan mengubah nama file dengan menambahkan nomor urut. Perintah yang akan kita gunakan adalah `ls`, `nl` dan `cut`.

Misalnya kita memiliki file sebagai berikut:

    8f3vwnx.jpg
    8yoQEOm.jpg
    fXQp8bi.jpg
    J1sgIAT.jpg
    lVsqIuh.jpg

Akan kita ubah menjadi `foto01.jpg`, `foto02.jpg`, `...`, `foto10.jpg`.

## Penomoran

Yang perlu kita lakukan pertama kali adalah memberikan nomor untuk setiap nama file. Hal ini bisa dilakukan dengan meng-`pipe` perintah `ls` ke `nl`. `ls` untuk menampilkan nama file dan `nl` untuk memberikan penomoran untuk setiap nama file yang dihasilkan dari `ls`.

    ls | nl

     1	8f3vwnx.jpg
     2	8yoQEOm.jpg
     3	fXQp8bi.jpg
     4	J1sgIAT.jpg
     5	lVsqIuh.jpg

Angkanya perlu diubah formatnya supaya menjadi `01`, `02` dan seterusnya. Tambahkan opsi `-w 2` (width angka dua digit) dan `-n rz` (angka di-rata kanan dan tambahkan `0` di depan -- "right zero").

    ls | nl -w 2 -n rz

    01	    8f3vwnx.jpg
    02	    8yoQEOm.jpg
    03	    fXQp8bi.jpg
    04	    J1sgIAT.jpg
    05	    lVsqIuh.jpg

Sebagai pembatas antara angka dengan nama file, kita gunakan karakter `#`. Silahkan gunakan karakter lain, asal nama file yang diubah tidak memiliki karakter pembatas ini. Tambahkan opsi `-s#` pada perintah `nl`.

    ls | nl -w 2 -n rz -s#
    
    01#8f3vwnx.jpg
    02#8yoQEOm.jpg
    03#fXQp8bi.jpg
    04#J1sgIAT.jpg
    05#lVsqIuh.jpg

Berikutnya kita tinggal mengubah nama file sesuai dengan nomor urutnya. Untuk mendapatkan nomor urut dan nama file, kita akan memotong hasil perintah `ls` sesuai dengan pembatas yang telah ditentukan.

## Perubahan nama file

    for i in `ls | nl -w 2 -n rz -s#`; do\
        nomor=`echo $i | cut -f1 -d#`;\
        file=`echo $i | cut -f2 -d#`;\
        mv $file foto-$nomor.jpg;
    done

``nomor=`echo $i | cut -f1 -d#`;`` digunakan untuk menyimpan nomor urut `01`, `02` dan seterusnya pada variable `$nomor`. Perintah `cut` digunakan untuk memotong string (pada contoh ini nomor dan nama file), opsi `-f1` maksudnya adalah ambil field pertama dari hasil pemotongan dan `-d#` menentukan batas field. Dengan menggunakan perintah `cut` yang sama kita ambil field kedua dan simpan ke variable `$file`.

Dan perintah terakhir pada loop `for` adalah mengubah nama file sesuai dengan yang kita inginkan. Apabila diinginkan bentuk nama file yang lain, misalnya `01-8f3vwnx.jpg`, kita tinggal mengganti perintah `mv $file foto-$nomor.jpg;` menjadi `mv $file $nomor-$file.jpg`.

## Urutan Spefisik

Contoh di atas melakukan penomoran sesuai dengan urutan file yang ditampilkan oleh `ls`. Secara *default* menampilkan file yang diurutkan sesuai dengan nama file. Apabila kita ingin penomoran sesuai dengan urutan tertentu, tinggal tambahkan opsi pada perintah `ls` di atas.

Misalnya, apabila file akan diberi nomor sesuai dengan waktu modifikasi (file terbaru ditampilkan dahulu):

    for i in `ls -t | nl -w 2 -n rz -s#`; do\
        nomor=`echo $i | cut -f1 -d#`;\
        file=`echo $i | cut -f2 -d#`;\
        mv $file foto-$nomor.jpg;
    done

Atau ditampilkan dari urutan waktu modifikasi paling lama ke yang paling baru:

    for i in `ls -t | nl -w 2 -n rz -s#`; do\
        nomor=`echo $i | cut -f1 -d#`;\
        file=`echo $i | cut -f2 -d#`;\
        mv $file foto-$nomor.jpg;
    done

Atau urutannya sesuai dengan yang diinginkan oleh user:

    for i in `ls -f J1sgIAT.jpg fXQp8bi.jpg 8yoQEOm.jpg lVsqIuh.jpg 8f3vwnx.jpg | nl -w 2 -n rz -s#`; do\
        nomor=`echo $i | cut -f1 -d#`;\
        file=`echo $i | cut -f2 -d#`;\
        mv $file foto-$nomor.jpg;
    done

Tool `ls`, `nl` dan `cut` memberikan kemampuan yang cukup *powerful* untuk melakukan perubahan nama file yang banyak, walaupun tentu belum segampang yang dapat dilakukan di Windows. Dengan menguasai ide merangkai perintah-perintah kecil untuk menyelesaikan masalah yang besar akan sangat membantu pekerjaan seorang *system administrator*.

Happy coding!
