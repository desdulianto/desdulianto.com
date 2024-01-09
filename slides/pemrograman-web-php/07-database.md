---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Database - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Database

Desdulianto

---

## Bahasan

* Database Type
* Security
* Search & Pagination

---

## Database Type

* Relational
  * SQL - Structured Query Language
  * ACID
  * contoh: Mysql, MariaDB, Postgresql, SQL Server, sqlite dll
* Non-Relational
  * Custom query language, NoSQL
  * BASE
  * contoh: MongoDB, elasticsearch, redis, dll

---

### Database Relasional

* Digunakan untuk kebutuhan data yang perlu integritas dan konsistensi yang tinggi vs performance
* Paling umum digunakan sebelum ditambahkan dengan database non-relasional (No-SQL)
* Query menggunakan bahasa SQL
* Mendukung transaction untuk melakukan operasi yang bersifat *atomic*

---

### Database Driver

* Database relasional yang populer digunakan pada PHP, Mysql dan Postgresql
* Database driver:
  * mysqli (terbatas hanya digunakan untuk database Mysql)
  * PDO (bisa digunakan untuk database lain misalnya Mysql, Postgresql, sqlite, dll)

---

### PHP Data Objects (PDO)

* [PHP Data Objects](https://www.php.net/manual/en/book.pdo.php)
* Abstraksi fitur-fitur untuk operasi terkait database
* Memerlukan driver untuk koneksi ke database, misalnya: Mysql, Postgresql
* Karena merupakan abstraksi akan lebih mudah untuk mengganti driver database jika dibutuhkan

---

#### Database Connection

* Menggunakan class `PDO` dan connection string/database source name (DSN) untuk menentukan jenis, nama dan parameter database yang akan digunakan
* Membuka connection

  ```php
  <?php
    $dbh = new PDO('mysql:host=localhost;dbname=test', $user, $pass);
  ?>
  ```

* Koneksi akan ditutup jika script selesai, atau dengan dengan meng-assign `null` ke variable resource koneksi `$dbh = null`

---

#### Database Persistent Connection

* Untuk meningkatkan performance database jangan terlalu sering membuka tutup koneksi ke database
* PDO dapat membuka koneksi persistent yang tidak ditutup setelah script selesai

  ```php
  <?php
    $dbh = new PDO('mysql:host=localhost;dbname=test', $user, $pass, array(
        PDO::ATTR_PERSISTENT => true
    ));
  ?>
  ```

* Disebut juga *connection pooling*

---

### Security

* SQL Injection, user mengirimkan perintah SQL sebagai data dan dieksekusi oleh database engine
  * Input tidak di-sanitasi dan di-validasi dengan ketat
* Gunakan prepared statement untuk mengirimkan variable input SQL query
* [PDO prepared statements](https://www.php.net/manual/en/pdo.prepared-statements.php)

---

### Search & Pagination

* Operasi yang sering dilakukan terhadap database
* Index dan query yang tetap dapat meningkatkan performance searching
  * Apabila dibutuhkan gunakan database lain untuk pencarian yang kompleks, misalnya elasticsearch untuk full text search
* Hasil query perlu dipaginate supaya tidak membebani system

---

### Pagination

* Limit Offset pagination
  * Performance menurun jika jumlah halaman sudah banyak
  * Bisa mengunjungi halaman random
* Cursor Pagination
  * Performance cukup stabil walaupun jumlah halaman sudah banyak
  * Untuk mengunjungi halaman ke-x harus melalui cursor x-1 (atau x+1)

---

#### Limit Offset Pagination

* Menggunakan query `limit` dan `offset`
* Database engine akan meng-skip data sampai dengan `offset`

  ```sql
  SELECT * FROM table WHERE filter ORDER BY id LIMIT 10 OFFSET 100; -- skip 100 row pertama
  ```

---

#### Cursor Pagination

* Menggunakan query `limit` untuk membatasi jumlah row
* Menggunakan kolom *cursor* sebagai penanda *previous* atau *next* page

  ```sql
  SELECT * FROM table WHERE id>next_cursor ORDER BY id LIMIT 10; -- moving forward
  SELECT * FROM table WHERE id<next_cursor ORDER BY id DESC LIMIT 10; -- moving backward
  ```
