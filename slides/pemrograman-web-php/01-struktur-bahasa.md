---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Struktur Bahasa - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Struktur Bahasa PHP

Desdulianto

---

## Bahasan

* [PHP Language Reference](https://www.php.net/manual/en/langref.php)
* Versi Bahasa 8.3
* Tags
* Aturan Penulisan Kode
* Tipe Data
* Variable
* Constant

---

## Tags

* Script PHP ditulis di dalam blok tag `<?php` dan `?>`
* Apabila kode PHP digabung bersama dengan konten lain (misalnya HTML/CSS/Javascript), maka tag penutup `?>` harus disertakan untuk menandakan akhir dari kode PHP
* Apabila kode PHP berdiri sendiri (tidak dicampur dengan konten lain), maka tag penutup `?>` boleh tidak ditulis
* Source code disimpan ke file dengan ekstensi `.php`

---

## Aturan Penulisan Kode

* Case sensitive
* Statement/ekspresi diakhiri dengan simbol titik koma `;`
* Komentar mengikuti C-style atau shell-style:
  * komentar multi baris `/* komentar multi baris */`
  * komentar satu baris (C-style) `// komentar satu baris`
  * komentar satu baris (shell-style) `# komentar satu baris`

---

## Tipe Data

* *Dynamic typed language*
* Tipe data *built-in*: `null`, `bool`, `int`, `float`, `string`, `array`, `object`, `callable`, `void`, `never`, `mixed`
* User Defined Type: `interface`, `class`, `trait`, `enum`
* Apabila memungkinkan PHP akan melakukan konversi data sesuai dengan jenis operasi yang dijalankan (*type juggle*)

---

## Deklarasi tipe data

* Optional
* Deklarasi terhadap tipe argument ke function/method, nilai kembali (return value) function/method, class properties
* Contoh:

  ```php
  function add(int $a, int $b): int
  {
      return $a + $b;
  }
  ```

---

## Type Juggle

* Konversi tipe data secara implisit
* PHP akan mencoba untuk mengkonversi tipe data apabila operasi yang dilakukan tidak didukung oleh operan dari operasi tersebut
* Contoh:

  ```php
  $a = 42;
  $b = "3";
  $c = $a + $b;
  echo $c; // 45
  ```

---

## Type Casting

* Melakukan konversi tipe data secara eksplisit
* Casting yang diperbolehkan: `(int)`, `(bool)`, `(float)`, `(string)`, `(array)`, `(object)`
* Contoh:

  ```php
  $a = 42.9;
  $b = 3.7;
  $c = $a + (int) $b;
  $d = (int) ($a + $b);
  echo $c; // 45.9
  echo $d; // 46
  ```

---

## Variable

* Diawali dengan simbol `$`
* Terdiri dari simbol karakter (huruf besar dan kecil), angka dan *underscore* `_`
* Tidak diawali dengan angka
* Variable valid: `$nama`, `$_nama`, `$Nama`, `$nama1`, `$nama_lengkap`
* Variable invalid: `nama`, `$1nama`, `$nama lengkap`, `$nama$`
* Variable khusus `$this` digunakan untuk mengacu ke object di dalam definisi class

---

### Variable - assign by value

* Nilai yang di assign ke variable merupakan salinan dari hasil evaluasi ekspresi

  ```php
  $a = [1,2,3];
  $b = $a;
  $a[0] = 10 ; // $a berubah nilainya jadi [10, 2, 3], tapi $b tetap bernilai [1,2,3]
  ```

* Perubahan pada variable awal tidak mempengaruhi nilai pada variable yang di-assign dari variable awal

---

### Variable - assign by reference

* Nilai yang di-assign ke variable merupakan pointer yang mengacu ke nilai dari variable awal
* Assignment dilakukan dengan prefix `&` pada variable yang di-assign

  ```php
  $a = 10;
  $b = &$a;
  $a = 100; // $a berubah nilainya jadi 100, $b karena di-assign dengan reference nilainya ikut berubah menjadi 100
  ```

* Assignment terhadap object secara default selalu di-assign by reference

---

### Variable - isset, unset & variable global

* `isset(mixed $var, mixed ...$vars): bool` meng-check apakah variable sudah di-inisialisasi (diisikan nilai awal) atau belum
* `unset(mixed $var, mixed ...$vars): void` menghapus variable
* Ketika script PHP dijalankan, PHP akan meng-assign beberapa variable global/predefined seperti `$_SERVER`, `$_REQUEST`, dll sesuai dengan lingkungan eksekusi
  * [Predefined Variables](https://www.php.net/manual/en/reserved.variables.php)
  * [Super Global Variable](https://www.php.net/manual/en/language.variables.superglobals.php)

---

### Variable - Scope

* Umumnya scope variable di PHP berlaku global, terkecuali untuk function
* Akses variable di dalam function akan mengacu ke scope local, variable yang dikenal hanya variable yang di-inisialisasi di dalam function tersebut
* Variable scope local akan otomatis hilang apabila eksekusi telah meninggalkan scope di mana variable tersebut di-inisialisasi

---

### Variable - global

* Untuk mengacu ke variable global di dalam function gunakan keyword `global`

  ```php
  $a = 1;

  function incrementA()
  {
    global $a;

    $a++;
  }
  ```

---

### Variable - static

* Variable static di-definisikan di dalam fungsi
* Nilai variable static akan diingat terus ketika fungsi yang sama dipanggil berulang kali

  ```php
  function incrementStatic()
  {
    static $a = 0;
    $a++;
    return $a;
  }
  incrementStatic(); // 1
  incrementStatic(); // 2
  ```

---

## Constant

* Menampung nilai yang tidak dapat diubah lagi selama script berjalan
* Diakses tanpa prefix `$`
* PHP versi < 8.0.0 constant didefinisikan dengan fungsi `define`
* PHP versi >= 8.0.0 constant didefinisikan dengan keyword `const`

  ```php
  define('PI', 3.14)
  const VERSI = '8.3';
  echo PI; // 3.14
  echo VERSI; // 8.3
  ```

---

### Constant - sambungan

* [Predefined constant](https://www.php.net/manual/en/reserved.constants.php)
* [Magic constant](https://www.php.net/manual/en/language.constants.magic.php)
