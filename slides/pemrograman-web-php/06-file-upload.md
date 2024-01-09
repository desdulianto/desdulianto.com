---
marp: true
theme: gaia
footer: 'Pemrograman Web Dengan PHP - File Upload - Desdulianto'
paginate: true
---
<!-- _paginate: skip -->
# File Upload

Desdulianto

---

## Bahasan

* Form File Upload
* Penanganan File Upload di Frontend
* Penanganan File Upload di Backend

---

## Form File Upload

* Menggunakan tag

  ```html
  <form method="post" enctype="multipart/form-data" action="handler.php">
    <input type="file" name="userfile">
    <input type="submit" value="send">
  </form>
  ````

---

### Attribute File Upload

* Attribute `accept` untuk menentukan jenis file yang bisa dipilih oleh user
  * MIME type
  * file extension

* Attribute `multiple` untuk mengupload beberapa file
* Attribute `capture` untuk mengambil foto dari camera device, jika tersedia

---

## Penanganan File Upload di Frontend

* [input type file](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file)
* [Using files from web applications](https://developer.mozilla.org/en-US/docs/Web/API/File_API/Using_files_from_web_applications)

---

## Penanganan File Upload di Backend

* File yang diupload diakses melalui variable `$_FILES`
* [PHP File Upload Post Method](https://www.php.net/manual/en/features.file-upload.post-method.php)
* File yang diupload disimpan pada lokasi sementara dan perlu dipindahkan ke lokasi lain
