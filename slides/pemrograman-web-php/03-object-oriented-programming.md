---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Object Oriented Programming - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Object Oriented Programming

Desdulianto

---

## Bahasan

* [Classes & Objects](https://www.php.net/manual/en/language.oop5.php)
  * [Abstract Class](https://www.php.net/manual/en/language.oop5.abstract.php)
  * [Interfaces](https://www.php.net/manual/en/language.oop5.interfaces.php)
  * [Traits](https://www.php.net/manual/en/language.oop5.traits.php)
* [Namespaces](https://www.php.net/manual/en/language.namespaces.php)
* [Enumerations](https://www.php.net/manual/en/language.enumerations.php)

---

## Classes & Objects

  ```php
  // definisi class
  class Person {
    public string $name;
    public function __construct(string $name) {
        $this->name = $name;
    }
    public function intro(): string {
        return "Hello, my name is $this->name";
    }
  }

  // object
  $budi = new Person('Budi');
  $budi->intro();
  ```

---

## Definisi class

* Terdiri dari *property* dan *method*
* Access modifier menentukan bagaimana property & method dapat diakses
  * `public`, `protected`, `private`
* Access modifier default `public`
* `$this` mengacu ke object yang didefinisikan oleh class
* Akses ke *property* dan `method` dengan operator `->`
* Object dibuat dengan keyword `new`

---

### Constructor & Destructor

* `__construct` untuk meng-inisialisasi object dipanggil otomatis ketika object dibuat dengan keyword `new`
* `__destruct` untuk menjalankan proses cleanup ketika object tidak dibutuhkan lagi. Biasanya jika ada resource yang di-*acquire* oleh object, perlu di-*release* pada *destructor*. Destructor akan otomatis jalan ketika tidak ada lagi reference terhadap object

---

### Constructor Promotion

* Tersedia sejak PHP versi 8
* Shorthand untuk men-deklarasi dan menginisialisasi property

  ```php
  class Person {
    public function __construct(
        public string $name,
        public string $gender
    ) {
        // constructor body
    }
  }
  $budi = new Person('Budi', 'Pria');
  ```

---

### Property

* Menampung data di dalam object
* Deklarasi bisa dengan menggunakan [type declaration](https://www.php.net/manual/en/language.types.declarations.php)
* Property `readonly` mencegah perubahan nilai
* Biasanya di-inisialisasi melalui constructor

---

### Class Constant

* Deklarasi constant di dalam class
* Diakses dengan menggunakan scope resolution operator `::`

  ```php
  class Person {
    const MAX_NAME_LENGTH = 200;
  }
  Person::MAX_NAME_LENGTH;
  ```

---

### Visibility

* Didefinisikan dengan access modifier (`public`, `protected`, `private`)
* Access `public` dapat diakses dari mana saja
* Access `protected` hanya dapat diakses dari dalam class dan class turunan
* Access `private` hanya dapat diakses dari dalam class itu sendiri
* Property & Method `public` dan `protected` akan diwariskan ke class turunan, `private` tidak diwariskan

---

### Inheritance

* Membuat class turunan dari sebuah super class (parent class)
* Subclass hanya bisa diturunkan dari satu super class (parent class)

  ```php
  class Person { ... }
  class Student extends Person { ... }
  ```

* Variable `parent` mengacu ke parent class
* Operator `instanceof` untuk men-check apakah object merupakan instance dari suatu class `$budi instanceof Student // true jika $budi adalah Student`

---

### Static keyword

* Dapat diakses tanpa perlu membuat object
* Nilai property static bersifat global terhadap class yang sama
* Diakses dengan `self` atau `static` dan operator `::`

  ```php
  class SomeClass {
    static private int $timeSpawn = 0;

    public function __construct() { self::$timeSpawn++; }
    public static function timeSpawn() { return self::$timeSpawn; }
  }
  ```

---

## Abstract Class

* Class dengan method yang body nya belum di definisikan
* Tidak dapat digunakan untuk membuat object
* Memerlukan concrete class yang mengimplementasi seluruh method abstract

  ```php
  abstract class Bangun2D {
    abstract public function luas();
  }
  class PersegiPanjang extends Bangun2D {
    public function luas() { return $this->panjang * $this->lebar; }
  }
  ```

---

## Interface

* Berisikan deklarasi method tanpa implementasi -- contract
* Class dapat mengimplementasikan satu atau lebih interface

  ```php
  interface SomeInterface {
    public function someMethod($parameter);
  }

  class SomeClass implements SomeInterface {
    public function someMethod($parameter) {
      // method implementation
    }
  }
  ```

---

## Trait

* *Code reuse* antara class yang bukan diturunkan dari super class yang sama

  ```php
  trait SomeTrait {
    function someMethod($parameter) {
      // method implementation
    }
  }

  class SomeClass {
    use SomeTrait;
    ...
  }
  ```

---

## Keyword Final

* Apabila dipasangkan pada class, class tersebut tidak dapat diturunkan lagi
* Apabila dipasangkan ke constant atau method, constant atau method tersebut tidak dapat di override oleh class turunan

  ```php
  class SomeClass {
    final const SomeFinalConstant = 1;

    final public function SomeMethod($parameter) {
      // method implementation
    }
  }
  ```

---

## Object Cloning

* Membuat salinan dari sebuah object, menggunakan keyword `clone`
* `clone` menyalin property object secara *shallow copy*
  * *shallow copy*, property reference hasil clone tetap mengacu ke object yang sama
  * *deep copy* membuat salinan terhadap property reference sehingga mengacu ke object yang berbeda
* `clone` akan memanggil method `__clone()` (jika ada) pada object hasil salinan untuk melakukan proses apabila dibutuhkan, misalnya melakukan proses *deep copy* untuk property reference

---

## Object Comparison

* Operator `==` membandingkan object berdasarkan masing-masing property object
* Operator `===` membandingkan apakah instance object mengacu ke object yang sama

  ```php
  class Person { public function __construct(public string $name) {} }

  $budi = new Person('budi');
  $budi1 = new Person('budi');
  $budi == $budi1; // true
  $budi === $budi1; // false
  ```

---

## Namespace

* Digunakan untuk mengorganisasikan kode PHP
* Menghindari terjadinya *name collision*
* Dapat menggunaka alias ketika mengimport module

  ```php
  // domain/library/SomeClass.php
  namespace domain\library;
  class SomeClass { ... }

  // otherdomain/code.php
  namespace otherdomain;
  use domain\library\SomeClass as ClassAlias;
  ```

---

## Enumerations

* Sejak PHP versi 8.1.0
* Mendifinisikan tipe data dengan batasan nilai, mis: Bulan (Januari - Desember)

  ```php
  enum Bulan {
    case Januari;
    case Februari;
    ...
    case Desember;
  }

  $januari = Bulan::Januari;
  ```

---

### Backed Enumerations

* Enum dengan nilai scalar, diakses melalui property `value`
* `from` dan `tryFrom` untuk konversi dari nilai scalar kembali ke enum

  ```php
  enum Bulan {
    case Januari = 'Jan';
    case Februari = 'Feb';
    ...
    case Desember = 'Des';
  }

  $bulan = Bulan::Januari;
  $bulan->value; // 'Jan'
  $bulan = Bulan::from('Jan'); // Bulan::Januari
  ```
