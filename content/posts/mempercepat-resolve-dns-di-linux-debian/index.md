---
title: "Mempercepat Resolve DNS pada Linux Debian"
date: 2015-11-04T00:00:00+07:00
description: "Tip untuk mempercepat proses DNS resolve pada linux Debian"
tags: ['shell', 'unix', 'linux', 'sysadmin', 'network']
---
Salah satu cara untuk mempercepat akses Internet adalah dengan mempercepat proses DNS lookup. Coba perhatikan pada saat membuka website, biasanya pada bagian status bar browser (bagian bawah) akan terlihat proses yang sedang dikerjakan oleh browser. Apabila prosesnya tertahan lama di `Looking up ...` kemungkinan besar DNS server sedang sibuk sehingga lambat merespon request client.

Biasanya browser akan men-*cache* respon dari server DNS. Karena itu, apabila kita mengakses host yang sama, browser tidak perlu lagi melalui proses `Looking up ...` dan akses Internet akan terasa lebih responsif.

Pada artikel ini, kita akan melakukan cache DNS secara *system wide* di Linux Debian. Tool yang akan digunakan adalah `dnsmasq`.

## dnsmasq

`dnsmasq` adalah aplikasi proxy DNS sederhana. Biasanya digunakan untuk menyediakan *caching DNS server* untuk jaringan, sekaligus menyediakan layanan DHCP. Kita akan menggunakan fungsi cache DNS tanpa DHCP untuk mesin *localhost*.

Perintah untuk menginstall `dnsmasq` di Linux Debian.

    apt-get install dnsmasq

Selanjutnya, kita konfigurasi `dnsmasq`, edit file `/etc/dnsmasq.conf`.

* `dnsmasq` hanya perlu mendengarkan request dari mesin *localhost*. Edit baris dengan kata kunci `interface` dan `listen-address` menjadi:

```
    interface=lo
    listen-address=127.0.0.1
```

* Kita tidak membutuhkan layanan DHCP

```
    no-dhcp-interface=lo
```

* Naikkan jumlah cache untuk memaksimalkan fungsi cache DNS

```
    cache-size=1024
```

Restart layanan `dnsmasq`:

```
    # /etc/init.d/dnsmasq restart
```

Berikutnya supaya kita dapat memanfaatkan `dnsmasq`, kita konfigurasi mesin `localhost` sebagai name server. Edit file `/etc/resolv.conf`, tuliskan konfigurasi sebagai berikut:

```
    nameserver 127.0.0.1
```

`dnsmasq` sendiri akan melakukan resolving ke name server yang berada di jaringan lokal. Untuk itu, perlu ditambahkan name server jaringan. Edit `/etc/resolv.conf` menjadi:

```
    nameserver 127.0.0.1
    nameserver 192.168.1.1
```

Sesuaikan name server 192.168.1.1 dengan name server yang berada di jaringan. `dnsmasq` secara cerdik akan mengabaikan `nameserver 127.0.0.1` yang dikonfigurasi di `/etc/resolv.conf`.

## Jaringan dinamis (DHCP)

Konfigurasi `/etc/resolve.conf` di atas tidak bisa digunakan pada jaringan yang alamat IP-nya dikonfigurasi secara dinamis (DHCP), misalnya pada jaringan wifi. Ini dikarenakan setiap berpindah ke jaringan yang berbeda (misalnya dari jaringan di kantor ke jaringan rumah), alamat IP perangkat dan name server juga akan ikut berubah.

Untuk jaringan dinamis, name server di `/etc/resolv.conf` akan secara otomatis dikonfigurasikan melalui DHCP. Supaya tetap bisa menggunakan `dnsmasq` sebagai caching DNS, kita perlu menambahkan secara otomatis `nameserver 127.0.0.1` sebagai item pertama di `/etc/resolv.conf`.

DHCP client default yang terpasang di Linux adalah DHCP versi Internet Software Consortium (ISC) memungkinkan kita untuk menambahkan name server selain name server yang diperoleh dari server DHCP. Edit `/etc/dhcp/dhclient.conf`. Hilangkan tanda komentar atau tambahkan baris berikut bila belum ada:

```
    prepend domain-name-servers 127.0.0.1;
```

Setelah semuanya selesai, restart jaringan:

```
    # /etc/init.d/networking/restart
```

Sekarang kita sudah bisa menjelajah Internet dengan lebih responsif.

Happy hacking!
