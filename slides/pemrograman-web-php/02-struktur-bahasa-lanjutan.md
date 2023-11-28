---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Struktur Bahasa Lanjutan - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Struktur Bahasa Lanjutan

Desdulianto

---

## Bahasan

* [Expressions](https://www.php.net/manual/en/language.expressions.php)
* [Operators](https://www.php.net/manual/en/language.operators.php)
* [Control Structures](https://www.php.net/manual/en/language.control-structures.php)
* [Functions](https://www.php.net/manual/en/language.functions.php)

---

## Expressions

* Semua operasi yang menghasilkan nilai
  * literal, contoh: `5`, `"Budi"`, `true`, `[1,2,3]`
  * operasi menggunakan operator, contoh: `5+5`, `"Hello " . $nama"`
  * pemanggilan fungsi, contoh: `array_sum([1,2,3])`
    * fungsi yang tidak mengembalikan nilai akan menghasilkan nilai `null`
  * statement tertentu yang menghasilkan nilai, contoh: `new SomeClass()`

---

## Operators

* Operasi yang di-*apply* ke satu atau lebih expression untuk menghasilkan nilai baru
* Operator dikelompokkan ke beberapa jenis:
  * Operator [aritmatika](https://www.php.net/manual/en/language.operators.arithmetic.php), [increment & decrement](https://www.php.net/manual/en/language.operators.increment.php), [assignment](https://www.php.net/manual/en/language.operators.assignment.php)
  * Operator [bitwise](https://www.php.net/manual/en/language.operators.bitwise.php)
  * Operator [perbandingan](https://www.php.net/manual/en/language.operators.comparison.php), [logical](https://www.php.net/manual/en/language.operators.logical.php)
  * Operator [string](https://www.php.net/manual/en/language.operators.string.php), [array](https://www.php.net/manual/en/language.operators.array.php)
  * Operator [Error Control](https://www.php.net/manual/en/language.operators.errorcontrol.php), [Execution](https://www.php.net/manual/en/language.operators.execution.php) & [Type](https://www.php.net/manual/en/language.operators.type.php)

---

### Operators - sambungan

* Berdasarkan banyak operan: *unary*, *binary*, *ternary*
* Eksekusi operator mengikuti aturan [precedence](https://www.php.net/manual/en/language.operators.precedence.php)
  * sebaiknya urutan operasi ditulis secara eksplisit menggunakan kurung dari pada bergantung dengan aturan *precedence*
* Kesalahan yang sering terjadi, menggunakan `+` untuk menggabungkan text (*text concatenation*). PHP menggunakan operator `.`

  ```php
  echo "Hello " . $nama;
  ```

---

## Control Structures

* Menentukan alur eksekusi program
* Conditional statement, memilih alur eksekusi mana yang akan dikerjakan
* Loop statement, mengulang eksekusi sebanyak n kali atau sesuai kriteria pengulangan
* [Link](https://www.php.net/manual/en/language.control-structures.php)

---

### Conditional Statement

* Memilih alur eksekusi program
* `if`, `else`, `elseif`/`else if`
* *ternary operator* `logical_expression ? true_value : false_value`
* `switch`, `case`, `default`, `break`
* `match`, `default` (sejak versi 8)

---

### Loop Statement

* Mengulang eksekusi sebanyak n kali atau sesuai kriteria perulangan
* `for`
* `foreach`
* `while`, `do-while`
* `break`, `continue`

---

### Other Statement

* Mengakhiri dan mengembalikan nilai dari fungsi: `return`
* Meng-import code dari file lain `require` dan `include`
  * `require` lebih strict dibanding `include` dan akan menghasilkan error kalau gagal meng-import file
* Meng-import code dari file lain jika belum di-import `require_once` dan `include_once`
* Loncat ke label tertentu di source code: `goto`

---

## Functions

* Mendefinisikan fungsi

  ```php
  function nama_fungsi($param1, $param2, ... $paramN)
  {
    // function body
  }
  ```

* Memanggil fungsi

  ```php
  nama_fungsi(1, 2, 3, 4, 5);
  ```

* keyword `return` untuk mengakhiri dan mengembalikan nilai

---

### Parameter & Argument

* Parameter, nama untuk variable input pada definisi fungsi
* Argument, nilai yang dikirimkan sebagai parameter pada saat pemanggilan fungsi mengikuti urutan pada saat definisi

  ```php
  function add($a, $b)
  {
    // $a & $b adalah parameter
    return $a + b;
  }

  add(10, 5); // 10 dan 5 adalah argumen yang dikirim ke parameter $a dan $b
  ```

---

#### Parameter & Argument: pass by reference

* Mengirimkan argument sebagai reference

  ```php
  function add_10_in_place(&$a)
  {
    $a += 10;
  }
  $a = 10;
  add_10_in_place($a); // $a sekarang menjadi 20
  ```

---

#### Parameter & Argument: default parameter value

* Parameter bersifat optional, jika tidak di-assign, parameter akan berisi default value

  ```php
  function hello($name = 'World')
  {
    return "Hello " . $name;
  }

  hello(); // Hello World
  hello("Budi"); // Hello Budi
  ```

* Didefinisikan setelah parameter mandatory (tanpa default value)

---

#### Parameter & Argument: variadic parameter

* Parameter diperlakukan sebagai array

  ```php
  function sum(...$a)
  {
    $sum = 0;
    foreach ($a as $i) {
        $sum += $i
    }
    return $sum;
  }
  sum(); // 0
  sum(1,2,3); // 1 + 2 + 3
  ```

---

#### Parameter & Argument: named parameter

* Parameter dapat di-assign mengikuti nama parameter (tidak perlu mengikuti urutan definisi)
* Sejak PHP versi 8

  ```php
  function greet($greeting, $name)
  {
    return "Hello $name, $greeting";
  }
  hello('Selamat pagi', 'Budi');
  hello(name: 'Budi', greeting: 'Selamat pagi');
  ```

---

### Anonymous Function

* Mendefinisikan fungsi tanpa di-assign dengan sebuah nama
* Biasanya digunakan sebagai argument yang dikirim ke parameter `callable`
* Disebut juga dengan nama *closure*

  ```php
  $hello = function hello($name) {
    return "Hello, $name";
  }
  $hello('Budi'); // Hello Budi
  ```

---

#### Anonymous Function: use keyword

* Variable di luar scope di-inject keyword `use`

  ```php
  $name = 'Budi';
  $a = function hello() use($name) { // inject $name
    return "Hello, $name";
  }
  $a();
  ```

---

#### Anonymous Function: arrow function

* Anonymous function yang isinya berupa expression dapat ditulis dengan notasi arrow (arrow function)

  ```php
  $add = fn($a, $b) => $a + $b;
  $add(1, 5);
  ```

* Arrow function akan langsung meng-capture variable di-luar scope tanpa perlu diinject dengan keyword `use`
