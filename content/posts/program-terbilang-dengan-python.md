---
title: "Program Terbilang Dengan Python"
date: 2015-08-22T14:04:16+07:00
description: "Membuat program terbilang bahasa Indonesia menggunakan Python"
tags:
- code
- python
---

Ada kalanya di dalam program kita membutuhkan sebuah fungsi untuk
menghasilkan sebuah kalimat terbilang untuk angka yang dimasukkan
(misalnya untuk mencetak kwitansi). Pada postingan ini, saya akan
menjelaskan fungsi untuk menghasilkan terbilang dengan menggunakan
python.

Saya akan mulai dengan menjelaskan proses pengembangan dari awal sampai
didapatkan sebuah program terbilang yang lengkap. Bagi yang ingin
melihat kode program lengkapnya, langsung saja ke-sini. Bagi yang
lainnya silahkan lanjutkan bacaannya.

## satuan

Kita akan mulai dari yang paling mudah terlebih dahulu yaitu mengubah
bilangan satuan menjadi terbilang. Kita hanya perlu memasangkan angka
yang diinput dengan terbilang yang dihasilkan, seperti:

```terminal
0 = nol, 1 = satu, dst 9 = sembilan
```

Di python kita menggunakan list yang menampung terbilang nol sampai
sembilan, yang diletakkan pada posisi index yang sesuai. Kemudian kita
tinggal melakukan pencarian ke dalam list sesuai dengan angka yang
di-input ke dalam fungsi. Kode python-nya:

```python
satuan = ['nol', 'satu', 'dua', 'tiga', 'empat', 
            'lima', 'enam', 'tujuh', 'delapan', 
            'sembilan']
def terbilang(angka):
    # satuan
    if angka < 10:
        return satuan[angka]
```

Berikutnya adalah mengubah bilangan puluhan menjadi terbilang.

## puluhan

Bilangan puluhan terdiri dari dua buah angka, angka pertama adalah
puluhan dan angka kedua adalah satuan. Misalkan angka 25, diuraikan
menjadi: **2 (puluhan) 5 (satuan)** ditulis menjadi: **dua puluh lima**.
Untuk mendapatkan terbilang dari bilangan puluhan, kita akan memisahkan
masing-masing angka yang menjadi puluhan dan yang menjadi satuan.
Caranya adalah dengan menggunakan operator bagi (/) dan operator modulus
(%).

Untuk mendapatkan angka puluhan kita gunakan operator bagi: 25 --> 25 /
10 = 2. Kita membagi angka yang diinput dengan angka 10 untuk
mendapatkan angka untuk puluhannya. Sedangkan untuk angka satuan kita
gunakan operator modulus: 25 --> 25 % 10 = 5. Kita menghitung sisa bagi
angka yang diinput dengan angka 10 untuk mendapatkan angka satuannya.

Setelah mendapatkan angka puluhan dan angka satuan, kita tinggal
dapatkan terbilang untuk angka tersebut dan kemudian hasilnya disatukan
menjadi terbilang yang lengkap.

```python
terbilang( angka / 10) + ' puluh ' + terbilang(angka % 10)
terbilang(25 / 10) + ' puluh ' + terbilang(25 % 10) --> terbilang(2) + ' puluh ' + terbilang(5) = 'dua puluh lima'
```

Tetapi bagaimana dengan bilangan yang satuannya nol? seperti 20, 30,
atau 70?

```python
terbilang(20 / 10) + ' puluh ' + terbilang(angka % 10) --> terbilang(2) + ' puluh ' + terbilang(0) = 'dua puluh nol'
```

Untuk itu kita perlu tambahkan pengecekan pada hasil modulus untuk
melihat apakah kita perlu menambahkan terbilang untuk satuan. Kodenya
menjadi:

```python
awalan = terbilang(angka / 10)
akhiran = ''
if angka % 10 > 0:
    satuan = terbilang(angka % 10)
return awalan + ' puluh ' + akhiran
```

Kode lengkapnya adalah:

```python
def terbilang(angka):
    # satuan
    if angka < 10:
        return satuan[angka]
    # puluhan
    elif angka >= 10 and angka <= 90:
        awalan = satuan[angka/10]
        akhiran = ''
        if angka % 10 > 0:
            akhiran = terbilang(angka % 10)
        return awalan + ' puluh ' + akhiran
```

Program di atas belum sempurna. Ada yang tidak sesuai dengan terbilang
angka 10 dan belasan (11 - 19). Untuk angka 10 terbilang yang dihasilkan
adalah satu puluh seharusnya adalah sepuluh dan untuk angka 19
seharusnya sembilan belas bukan satu puluh sembilan. Kita akan
selesaikan masalah ini pada bagian berikut ini.

## 10, 100 dan 1000

Untuk angka 10, 100 dan 1000 kita tuliskan sebagai sepuluh, seratus dan
seribu bukan satu puluh, satu ratus dan satu ribu. Berbeda dengan angka
20, 200 dan 2000 yang terbilangnya menjadi dua puluh, dua ratus dan dua
ribu. Untuk kasus khusus seperti ini kita akan menggantikan kata satu
dengan se.

Kode lengkapnya:

```python
def terbilang(angka):
    ...
    # puluhan
    elif angka >= 10 and angka <= 99:
        awalan = satuan[angka/10]
        if awalan == 'satu' and angka <= 1000:
            awalan = 'se'
        akhiran = ''
        if angka % 10 > 0:
            akhiran = terbilang(angka % 10)
        return awalan + ' puluh ' + akhiran
```

Berikutnya kita akan percantik tampilan untuk kasus khusus ini.

## se puluh vs sepuluh

Hasilnya sudah lebih baik sekarang, tetapi 10 menjadi ’se puluh’ bukan
’sepuluh’. permasalahan spasi ini juga cukup mengganggu estetika
penulisan terbilang. Kita akan menambahkan spasi antara awalan dan puluh
apabila awalan bukan ’se’ dan juga menambahkan spasi sebelum akhiran
apabilan akhiran bukan ’’. Kita akan gunakan teknik string formating dan
ekspresi **if** di dalam kode ini.

Kodenya adalah:

```python
def terbilang(angka):
    ...
    # puluhan
    elif angka >= 10 and angka <= 99:
        ...
        if angka % 10 > 0:
            akhiran = terbilang(angka % 10)
            return '%s%s%s%s%s' % (awalan, ' ' if awalan != 'se' else '', 'puluh', ' ' if akhiran != '' else '', akhiran)
```

Selesai permasalahan untuk kasus bilangan 10, 100 dan 1000. Berikutnya
adalah giliran bilangan belasan.

## belasan

Sejauh ini kita sudah berhasil menghasilkan terbilang untuk satuan dan
puluhan. Khusus untuk belasan, format penulisan berbeda dengan puluhan
lainnya. Misalnya 19 terbilangnya adalah sembilan belas bukan sepuluh
sembilan (apabila kita mengikuti aturan yang sama untuk angka 23 dsb).
Untuk belasan, angka satuannya akan disebutkan terlebih dahulu baru
diikuti dengan akhiran belas. Cara kita mengatasi masalah ini adalah
dengan menukar posisi satuan yang tadinya menjadi akhiran sekarang
menjadi awalan.

Kodenya adalah:

```python
def terbilang(angka):
    # satuan
    ...
    # belasan
    elif angka >= 11 and angka <= 19:
        awalan = satuan[angka%10]
        if awalan == 'satu':
            awalan = 'se'
        return '%s%s%s' % (awalan, ' ' if awalan != 'se' else '', 'belas')
    # puluhan
    ...
```

Akhirnya terbilang yang dihasilkan sudah jauh lebih baik. Dan
selanjutnya kita akan membuat terbilang untuk angka ratusan, ribuan dan
seterusnya sampai tidak terhingga.

## ratusan, ribuan, jutaan, miliaran, triliunan, dst

Teknik yang kita gunakan untuk menghasilkan terbilang untuk bilangan
puluhan dapat digunakan juga pada bilangan ratusan, ribuan dst. Misalnya
angka 164, yang kita lakukan adalah pisahkan angka menjadi bagian
ratusan, puluhan dan satuan.

```terminal
1 (ratusan) 6 (puluhan) 4 (satuan)
```

Dan kemudian gunakan teknik rekursif untuk menghasilkan terbilang.

Kodenya:

```python
def terbilang(angka):
    # puluhan
    ...
    # ratusan
    elif angka >= 100 and angka <= 999:
        awalan = satuan[angka/100]
        if awalan == 'satu' and angka <= 1000:
            awalan = 'se'

        akhiran = ''
        if angka % 10 > 0:
            akhiran = terbilang(angka % 10)
        return '%s%s%s%s%s' % (awalan, ' ' if awalan != 'se' else '', 'ratus', ' ' if akhiran != '' else '', akhiran)
```

Perhatikan bahwa kode yang kita gunakan untuk menghasilkan terbilang
untuk angka ratusan mirip dengan angka puluhan. kita hanya perlu
mengubah range angka yang diperiksa (100 dan 999) kemudian angka yang
digunakan pada operasi bagi dan modulus (100) dan satuannya (’ratus’).
Hal yang sama bisa digunakan untuk bilangan ribuan, jutaan dan
seterusnya.

Untuk mempermudah dan mempersingkat kode kita dapat menggunakan teknik
perulangan. Kita akan buat string satuan yang sesuai dengan angkanya (10
–\> puluh, 100 –\> ratus, 1000 –\> ribu, 1000000 –\> juta dst) dan
kemudian tinggal menggunakan nilai tersebut di dalam perulangan kita.

Kita membuat sebuah *dictionary* yang menghubungkan antara satuan
terbilang dengan angka terbilang (10 –\> puluh, 100 –\> ratus, dst)
dengan nama **ket** dan diurutkan dari terbesar ke kecil (dengan nama
**keys**). Kemudian pada perulangan (yang menggantikan bagian puluhan)
kita mencari satuan terbilang yang paling besar (i) yang mendekati angka
yang kita masukkan. Apabila angka yang dimasukkan dibagi dengan i lebih
besar dari 10 gunakan teknik rekursif untuk mendapatkan terbilangnya
(misalnya 135667 –\> kita menggunakan rekusif untuk mendapatkan
terbilang seratus tiga puluh lima ribu).

```python
awalan = satuan[angka/i] if angka / i < 10 else terbilang(angka/i)
```

Variable tengah di-isikan dengan terbilang dari satuan yang tepat
(diambil dari variable keys).

Kode di atas memungkinkan kita menghasilkan terbilang dari bilangan
satuan sampai dengan milyaran. apabila diperlukan untuk bilangan yang
lebih besar, kita hanya perlu menambahkan angka dan terbilangnya pada
variable ket. misalnya untuk triliunan:

```python
ket = {10: 'puluh', 100: 'ratus', 1000: 'ribu', 1000000: 'juta', 1000000000: 'milyar', 1000000000000: 'triliun'}
```

Silahkan dicobakan kode tersebut dengan bilangan berapapun yang
diinginkan untuk melihat hasilnya.

Happy Coding.

[Code lengkap](https://bitbucket.org/desdulianto/terbilang)
