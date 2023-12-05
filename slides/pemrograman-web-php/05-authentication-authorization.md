---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Authentication & Authorization - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Authentication & Authorization

Desdulianto

---

## Bahasan

* Intro
* Session Authentication
* HTTP Authentication

---

## Intro

* Memproteksi akses terhadap resource
* Challenge-response Authentication
  * User & password pair
  * Secret word
  * Identification token
* HTTP Response
  * [401 Unauthorized](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401)
  * [403 Forbidden](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/403)

---

## Session Authentication

* Memanfaatkan PHP session untuk menentukan apakah request sudah ter-autentikasi atau belum
* Client mengirimkan informasi session id melalui cookie
  * Menggunakan HTML form untuk input *credential*
  * Gunakan HTTPS untuk mencegah penyadapan ketika cookie ditransfer
* Session dimanage sendiri oleh aplikasi

---

## HTTP Authentication

* [HTTP Authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)
* Mekanisme:
  * Server mengirimkan response HTTP 401 dan cara untuk melakukan authentication oleh client
  * Client melakukan authentication sesuai skema dari server, biasanya akan ditampilkan prompt untuk meng-input username dan password
* [HTTP Authorization Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization)
* [PHP HTTP Authentication](https://www.php.net/manual/en/features.http-auth.php)

---

### [Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication)

* Server mengirimkan response HTTP 401 dengan header `WWW-Authenticate: Basic realm="some realm"`
* Client menampilkan prompt/input dialog untuk memasukkan *credential* (username dan password)
  * username dan password digabung jadi satu 1 text dengan dipisahkan `:`
  * di-encode ke base64 dan kemudian dikirim ke server melalui request header `Authorization: Basic xxx`

---

### [Digest Authentication](https://en.wikipedia.org/wiki/Digest_access_authentication)

* Mekanisme dasar sama dengan Basic Authentication
* *Credential* diproteksi dengan metode hashing/digest sesuai dengan parameter yang dikirim oleh server
* Tujuannya adalah memproteksi *credential* yang dikirim ke server

---

### [Token Authentication & Authorization](https://swagger.io/docs/specification/authentication/bearer-authentication/)

* Schema HTTP Authorization dengan menggunakan token bearer `Authorization: Bearer <token>`
* Token di-generate oleh authentication service, bisa dari web yang bersangkutan (first party) atau dari pihak ketiga (third party, misalnya login menggunakan akun Google, Facebook, dll)
  * [OAuth 2.0](https://oauth.net/2/)
* Token yang populer dipakai [JSON Web Token (JWT)](https://jwt.io/)

---

#### Mekanisme First Party Authentication & Authorization

* Server mengirimkan HTTP 401
* Client melakukan authentication (bisa melalui web form atau mekanisme lain)
  * Server authentication akan menvalidasi authentication, dan mengirimkan authorization token
  * Client menggunakan authorization token untuk mengakses resource
* Server melakukan validasi terhadap authorization token, jika valid, request diperbolehkan
