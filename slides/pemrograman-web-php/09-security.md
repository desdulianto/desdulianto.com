---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Web Security - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Web Security

Desdulianto

---

## Bahasan

* HTTPS
* SQL Injection
* Cross-site Request Forgery (CSRF)
* Resources

---

## HTTPS

* Komunikasi data HTTP bersifat plain text
* HTTPS mengenkripsi data HTTP sehingga data yang disadap tidak bisa dibaca oleh penyerang
* Menggunakan skema enkripsi public key/shared key
  * Server mengirimkan certificate yang terdiri dari public key dan identitas server yang di-sign oleh Certificate Authority (CA)
  * Client menverifikasi identitas sertifikat server, dan melakukan secret key exchange dengan server
  * Komunikasi selanjutnya dilakukan secara terenkripsi

---

## SQL Injection

* Penyerang mengirimkan data yang tanpa sengaja dieksekusi sebagai perintah SQL
* Data input tidak divalidasi dan disanitasi dengan benar dan ketat
* Hindari memasang parameter ke SQL query dengan string interpolation
  * Gunakan prepared statement
  * Validasi dan sanitasi input

---

## Cross-site Request Forgery (CSRF)

* Code di-inject sebagai content halaman web, user yang mengunjungi halaman web ini secara tidak sadar akan mengeksekusi code penyerang
* Sanitasi content dari user, ketika disimpan dan ketika ditampilkan
* CSRF token pada form
* Implementasi CORS untuk menfilter request AJAX dari domain tertentu

---

## Resources

* [Open Worldwide Application Security Project (OWASP)](https://owasp.org/), *best practice* mengenai keamanan aplikasi
* [Web Application Firewall](https://en.wikipedia.org/wiki/Web_application_firewall), monitoring and attack prevention
* [Common Vulnerability and Exposures](https://www.cve.org/), daftar masalah keamanan yang dipublish untuk umum
