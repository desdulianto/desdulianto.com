---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - Client Side Scripting - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# Client Side Scripting

Desdulianto

---

## Bahasan

* Javascript
* Document Object Model (DOM)
* AJAX
* Fetch API
* Promise

---

## Javascript

* [MDN Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
* Script yang banyak didukung oleh web browser
* Interpreted, dynamic
* Cukup sulit untuk membuat aplikasi yang konsisten karena implementasi javascript yang berbeda antar web engine

  ```javascript
  <script>
    alert("hello world")
  </script>
  ```

---

## Document Object Model (DOM)

* [MDN Document Object Model](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
* Document HTML berupa struktur tree
* Javascript dapat memanipulasi DOM untuk membuat tampilan yang dinamis di sisi client

  ```javascript
  var output = document.getElementById('output');
  var h1 = document.createElement("h1");
  h1.appendChild(document.createTextNode('Hello world'));
  output.appendChild(h1);
  ```

---

## AJAX

* [Asynchronous Javascript And XML](https://developer.mozilla.org/en-US/docs/Glossary/AJAX)
* Teknik pemrograman untuk menulis aplikasi pada frontend
  * Web dapat melakukan request ke backend service untuk mengakses data, menggunakan fetch API
  * Mengubah tampilan halaman web dengan memanipulasi DOM
* Menghasilkan *single page application* (SPA) 
  * Beberapa framework populer: React, Vue, Angular

---

## Fetch API

* [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
* Mengakses resource menggunakan HTTP request (sebelumnya menggunakan `XMLHttpRequest`)
* `fetch` mengembalikan `Promise` yang di-*resolve* menjadi HTTP Response

  ```javascript
  fetch('some_resource').then((response) => return response.body; ).catch((error) => alert(error); );
  ```

---

## Promise

* [MDN Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* Object yang menampung hasil eksekusi proses *asynchronous*
* `Promise.resolve` mengembalikan hasil untuk proses *asynchronous* yang berhasil
  * hasil dihandle dengan `then`
* `Promise.reject` mengembalikan hasil untuk proses *asynchronous* yang gagal
  * hasil dihandle dengan `catch`
