---
title: "Install Ubuntu Server 14.04 (dengan screenshot)"
date: 2016-06-17T00:00:00+07:00
description: "Panduan menginstall Ubuntu Server 14.04"
tags: ['linux', 'sysadmin', 'guide', 'server']
---
Panduan *step-by-step* instalasi Ubuntu Server 14.04 dengan menggunakan partisi **LVM**.

1. Boot dari CD/USB Flashdisk installer Ubuntu Server 14.04 dan kemudian pilih bahasa yang akan digunakan.
![Bahasa instalasi][bahasa-installer]

2. Pilih menu Install Ubuntu Server untuk melakukan instalasi.
![Menu install][menu-install]

3. Pilih bahasa yang akan digunakan di dalam OS.
![Bahasa OS][bahasa-os]

4. Konfigurasi regional.
![Location][location]
![Location][location-1]
![Location][location-2]
![Locales][locales]
![Keyboard][keyboard]
![Keyboard][keyboard-1]
![Keyboard][keyboard-2]

5. Konfigurasi jaringan. Apabila pada jaringan lokal tidak ada DHCP server, lakukan konfigurasi IP secara manual.
![Konfigurasi IP][konfigurasi-ip]
Contoh ini menggunakan jaringan 192.168.100.0/24 dan 192.168.100.1 sebagai gateway dan untuk DNS menggunakan DNS publik google (8.8.8.8 dan 8.8.4.4)
![Konfigurasi IP][konfigurasi-ip-1]
![Konfigurasi IP][konfigurasi-ip-2]
![Konfigurasi IP][konfigurasi-ip-3]
Berikutnya masukkan nama host dan nama domain.
![Hostname][hostname]
![Domain name][domain-name]

6. Tambahkan user account. User account pertama pada Ubuntu adalah user spesial yang dapat menjalankan perintah sebagai **root** dengan perintah `sudo`.
![User baru][user]
Home directory user dapat di-encrypt untuk meningkatkan keamanan sistem. Tetapi apabila kunci enkripsi hilang, data user tidak dapat lagi dibaca. Untuk contoh ini kita tidak meng-enkrip home directory.
![Encrypt Home Directory][encrypt-homedir]

7. Konfigurasi zona waktu.
![Timezone][timezone]
![Timezone][timezone-1]

8. Berikutnya langkah yang cukup panjang, yaitu melakukan konfigurasi partisi pada harddisk. Pada contoh ini kita menggunakan partisi LVM. [LVM](https://en.wikipedia.org/wiki/Logical_Volume_Manager_%28Linux%29) memudahkan kita apabila pada masa mendatang ingin dilakukan perubahan tata letak partisi.

  - Bentuk partisi kosong pada harddisk baru, misalnya **sda**

![Partisi kosong][partisi-kosong]
![Partisi kosong][partisi-kosong-1]
![Partisi kosong][partisi-kosong-2]
![Partisi kosong][partisi-kosong-3]

  - Ubuntu tidak dapat melakukan boot langsung dari partisi LVM. Oleh karena itu, kita harus membuat partisi biasa yang khusus untuk menampung kernel dan data booting. Pilih **FREE SPACE** pada **sda** dan kemudian **Create new partition**.

![Partisi boot](09-partition-disks-4.png)
![Partisi boot](09-partition-disks-5.png)
![Partisi boot](09-partition-disks-6.png)
![Partisi boot](09-partition-disks-7.png)
![Partisi boot](09-partition-disks-8.png)
![Partisi boot](09-partition-disks-9.png)
![Partisi boot](09-partition-disks-11.png)

9. Berikutnya adalah membuat partisi LVM pada **FREE SPACE**. Gunakan seluruh space yang tersisa sebagai partisi LVM.

![Partisi LVM](09-partition-disks-13.png)
![Partisi LVM](09-partition-disks-14.png)
![Partisi LVM](09-partition-disks-15.png)
![Partisi LVM](09-partition-disks-16.png)
![Partisi LVM](09-partition-disks-17.png)
![Partisi LVM](09-partition-disks-18.png)

10. Pada partisi LVM yang baru, bentuk Volume Group (VG). Pilih partisi **sda5** yang dibentuk pada langkah 9.
![Volume Group](09-partition-disks-19.png)
![Volume Group](09-partition-disks-20.png)
![Volume Group](09-partition-disks-21.png)
![Volume Group](09-partition-disks-22.png)

11. Setelah selesai membuat Volume Group bentuk Logical Volume (LV) untuk menampung partisi yang akan digunakan pada sistem. Pada contoh ini kita membuat LV untuk partisi root, opt, var dan home. Tidak seluruh space yang tersedia harus dialokasikan sekarang ini. Apabila dibutuhkan, sediakan space kosong pada VG bersangkutan untuk keperluan snapshot supaya dapat dilakukan backup tanpa harus mematikan sistem.

![Logical Volume](09-partition-disks-23.png)
![Logical Volume](09-partition-disks-24.png)
![Logical Volume](09-partition-disks-25.png)
![Logical Volume](09-partition-disks-26.png)
![Logical Volume](09-partition-disks-27.png)

12. Bentuk filesystem dan mount point di atas partisi LV yang telah dibentuk sebelumnya.

![root mountpoint](09-partition-disks-30.png)
![home mountpoint](09-partition-disks-28.png)
![opt mountpoint](09-partition-disks-29.png)
![var mountpoint](09-partition-disks-31.png)

13. Setelah selesai membentuk partisi, lanjutkan proses instalasi dengan memilih menu **Finish partitioning and write changes to disk**. Karena kita tidak membentuk partisi untuk swap, akan ditanyakan apakah mau kembali ke menu partisi untuk membentuk partisi swap. Apabila ingin membuat partisi swap, pilih **yes** jika tidak lanjutkan dengan pilih **no**.

![swap partition](09-partition-disks-32.png)
Berikutnya akan ditampilkan partisi yang dibentuk dan yang akan diformat, pilih **yes** untuk melanjutkan.
![confirm partition](09-partition-disks-33.png)

14. Setelah proses pembentukan partisi dan format selesai, dilanjutkan dengan proses untuk memilih package yang akan di-install.

  - Installer akan menanyakan informasi proxy. Jika jaringan tidak terhubung melalui proxy server, kosongkan saja.

![proxy](10-package-manager.png)

  - Menentukan konfigurasi automatic update.

![automatic update](11-configure-tasksel.png)

  - Pemilihan package yang akan di-install. Pada contoh ini kita hanya menginstall package openssh server.

![package selection](12-software-selection.png)

15. Setelah proses instalasi selesai, install GRUB boot manager supaya sistem dapat melakukan boot ke sistem Linux.

![boot manager](13-install-grub.png)
![finish installation](14-finish-installation.png)

[bahasa-installer]: 00-boot-language.png
[menu-install]: 01-boot-install-ubuntu-server.png
[bahasa-os]: 02-select-language.png
[location]: 03-select-location.png
[location-1]: 03-select-location-1.png
[location-2]: 03-select-location-2.png
[locales]: 04-configure-locales.png
[keyboard]: 05-configure-keyboard.png
[keyboard-1]: 05-configure-keyboard-1.png
[keyboard-2]: 05-configure-keyboard-2.png
[konfigurasi-ip]: configure-network-manually.png
[konfigurasi-ip-1]: configure-network-manually-1.png
[konfigurasi-ip-2]: configure-network-manually-2.png
[konfigurasi-ip-3]: configure-network-manually-3.png
[hostname]: 06-configure-network.png
[domain-name]: configure-network-manually-domain.png
[user]: 07-user-password.png
[encrypt-homedir]: 07-user-password-2.png
[timezone]: 08-configure-clock.png
[timezone-1]: 08-configure-clock-1.png
[partisi-kosong]: 09-partition-disks.png
[partisi-kosong-1]: 09-partition-disks-1.png
[partisi-kosong-2]: 09-partition-disks-2.png
[partisi-kosong-3]: 09-partition-disks-3.png