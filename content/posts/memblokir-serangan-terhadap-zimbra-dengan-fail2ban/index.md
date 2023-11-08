---
title: "Memblokir Serangan Brute Force Terhadap Zimbra Dengan fail2ban"
date: 2017-07-15T00:00:00+07:00
description: "Panduan untuk memblokir serangan brute force terhadap email server Zimbra dengan menggunakan fail2ban"
tags: ['server', 'sysadmin', 'linux', 'zimbra', 'fail2ban']
---
Zimbra salah satu software open source yang populer untuk layanan email memiliki fitur untuk [meng-*lockout*](https://www.zimbra.com/docs/ne/4.5.10/administration_guide#account) email apabila terjadi kesalahan login oleh user. Tujuan dari fitur ini tentunya untuk menghalangi serangan *brute force* terhadap account email. Tetapi kadang, administrator Zimbra jadi disibukkan oleh permintaan (atau malah dimarahi) oleh user yang accountnya terkunci karena menjadi korban usaha *brute force*.

Salah satu cara yang efektif untuk mengatasi masalah *brute force* ini bisa dengan secara pro-aktif memblokir alamat IP penyerang yang mencoba melakukan *brute force*. Dan untungnya ada software yang memang berfungsi untuk keperluan ini, yaitu [**fail2ban**](https://www.fail2ban.org/wiki/index.php/Main_Page).

*fail2ban* me-monitor *log file* untuk mencari percobaan login yang gagal, dan kemudian memblokir IP penyerang dengan menambahkan *rule iptables*. Berikut ini cara untuk memblokir percobaan *brute force* terhadap Zimbra.

Panduan ini menggunakan OS Debian Linux, sesuaikan perintah yang digunakan sesuai dengan distribusi Linux yang Anda gunakan.

1. Install *fail2ban* apabila belum.
  `apt-get install fail2ban`
2. Tambahkan *filter fail2ban* untuk menyaring informasi pada log file yang berkaitan dengan usaha *brute force*.

  1. Buat file baru di `/etc/fail2ban/filter.d/zimbra.conf`

  2. Tambahkan konfigurasi berikut:

```
    failregex = oip=<HOST>;.* account - authentication failed for .* \(no such account\)$
            oip=<HOST>;.* security - cmd=Auth; .* error=authentication failed for .*, invalid password;$
            ;oip=<HOST>;.* security - cmd=Auth; .* protocol=soap; error=authentication failed for .* invalid password;$
            oip=<HOST>;.* SoapEngine - handler exception: authentication failed for .*, account not found$
            WARN .*;ip=<HOST>;ua=ZimbraWebClient .* security - cmd=AdminAuth; .* error=authentication failed for .*;$
            NOQUEUE: reject: RCPT from .*\[<HOST>\]: 550 5.1.1 .*: Recipient address rejected:
```

  Konfigurasi `failregex` berisikan aturan `regex` untuk mencari string pada file log yang menunjukkan kegagalan login. Tag `<HOST>` digunakan untuk menunjukkan alamat IP host yang melakukan percobaan login.

3. Tambahkan konfigurasi jail untuk menentukan aksi apa yang akan dilakukan apabila ditemukan serangan *brute force*.

    1. Buat file baru `/etc/fail2ban/jail.d/zimbra.conf`. File ini berisikan referensi ke filter pada langkah nomor 2 sebelumnya dan aksi yang akan dilakukan apabila *fail2ban* menemukan IP yang melakukan penyerangan.

    2. Tambahkan konfigurasi berikut:

```
    [zimbra-audit]
    enabled = true
    filter = zimbra
    action = iptables-allports[name=zimbra-audit]
    logpath = /opt/zimbra/log/audit.log
    bantime = 2592000
    findtime = 86400
    maxretry = 2
    ignoreip = 127.0.0.0/8 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
```

* `[zimbra-audit]` adalah  nama jail untuk mengidentifikasi rule filter dan action.
* `filter` diisikan dengan nama file filter yang dibuat di langkah 2 sebelumnya (tanpa ektensi `.conf`).
* `action` diisikan dengan aksi apa yang hendak dilakukan apabila ditemukan percobaan serangan. Pada contoh ini koneksi dari IP penyerang akan diblok seluruhnya. `[name=zimbra-audit]` merupakan parameter yang dikirim ke object action, pada contoh ini adalah nama *chain* iptables yang digunakan untuk memblokir IP. Daftar action dan parameter *fail2ban* dapat dilihat di `/etc/fail2ban/action.d`.
* `logpath`berisikan file log yang akan dimonitor oleh *fail2ban*. Isikan dengan file log `audit.log` Zimbra.
* `bantime` dalam satuan detik merupakan waktu berapa lama IP penyerang akan diblokir. Pada contoh ini, IP penyerang di blokir selama 1 bulan (2592000 detik)
* `findtime` dalam satuan detik. *fail2ban* akan menyimpan informasi IP yang ditemukan di dalam file log selama jangka waktu yang ditentukan pada konfigurasi ini. Pada contoh ini, IP penyerang akan dicatat dan diingat selama 1 hari (86400 detik).
* `maxretry` jumlah percobaan maximum. Apabila dalam jangka waktu `findtime` ditemukan IP penyerang melebihi dari jumlah `maxretry` maka *fail2ban* akan menjalankan perintah pada konfigurasi `action`.
* `ignoreip` diisikan dengan alamat IP atau network yang di-ignore oleh *fail2ban*. IP yang terdaftar disini tidak akan diblokir oleh *fail2ban* walaupun terjadi kegagalan login sebanyak rule yang ditentukan di atas. Biasanya saya memasukkan IP network local beserta dengan IP lainnya (yang sifatnya static) yang dianggap `trusted`.

4. Restart *fail2ban*: `/etc/init.d/fail2ban restart` atau `systemctl restart fail2ban` apabila menggunaka `systemd`.

  Untuk melihat apakah ada IP yang diblokir atau dicatat oleh *fail2ban* gunakan perintah `fail2ban-client`. Misalnya untuk melihat status jail `zimbra-audit` sesuai dengan konfigurasi di atas: `fail2ban-client status zimbra-audit`.

  Contoh tampilan:
```
Status for the jail: zimbra-audit
|- Filter
|  |- Currently failed:	0
|  |- Total failed:	0
|  `- File list:	/opt/zimbra/log/audit.log
`- Actions
   |- Currently banned:	16
   |- Total banned:	16
   `- Banned IP list:	104.193.9.76 154.118.11.129 154.118.33.192 154.118.34.247 154.118.49.120 154.120.108.209 197.242.117.145 202.60.94.19 212.227.198.184 222.47.26.140 37.49.224.170 37.49.224.176 37.49.224.251 41.217.126.122 47.91.40.144 94.74.81.49
```


IP yang di-ban bisa dilihat juga dengan menggunakan perintah `iptables`: `iptables -L f2b-zimbra-audit -n`. Sesuaikan `f2b-zimbra-audit` dengan nama jail yang dikonfigurasikan.

Apabila ada IP yang ingin di-unban atau di-ban secara manual, gunakan kembali perintah `fail2ban-client`. Contoh:

1. `fail2ban-client set zimbra-audit banip 172.16.10.18` untuk mem-ban IP 172.16.10.18 secara manual.
2. `fail2ban-client set zimbra-audit unbanip 172.16.10.18` untuk mem-unban IP 172.16.10.18.

Ref:

* https://www.vavai.net/2011/10/tips-improving-zimbra-mail-server-security-with-fail2ban/
* http://linux-sys-adm.com/how-to-configure-firewall-and-fail2ban-for-prevent-brute-force-attack-zimbra-8.6-on-ubuntu-server-14.04-lts-step-by-step/