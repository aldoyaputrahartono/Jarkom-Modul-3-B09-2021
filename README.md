# Jarkom-Modul-3-B09-2021

Repositori Praktikum Jarkom Modul 3

|NRP           |Nama                   |
|:------------:|:---------------------:|
|05111940000084|Aldo Yaputra Hartono   |
|05111840000023|Izzulhaq Fawwaz Syauqiy|
|05111940000092|Maximilian H M Lingga  |

## Soal 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, dan Water7 sebagai Proxy Server.

### Jawaban
Membuat topologi sebagai berikut:

![01-01](https://user-images.githubusercontent.com/31863229/140699255-14c7439e-3cfe-445e-87b9-646a2ef110ff.PNG)

Lakukan setting network masing-masing node dengan fitur `Edit network configuration` dengan setting sebagai berikut:

**Foosha (sebagai Router / DHCP Relay)**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.181.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.181.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.181.3.1
	netmask 255.255.255.0
```

**Loguetown (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.1.2
	netmask 255.255.255.0
	gateway 192.181.1.1
```

**Alabasta (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.1.3
	netmask 255.255.255.0
	gateway 192.181.1.1
```

**EniesLobby (sebagai DNS Master)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.2
	netmask 255.255.255.0
	gateway 192.181.2.1
```

**Water7 (sebagai Proxy Server)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.3
	netmask 255.255.255.0
	gateway 192.181.2.1
```

**Jipangu (sebagai DHCP Server)**
```
auto eth0
iface eth0 inet static
	address 192.181.2.4
	netmask 255.255.255.0
	gateway 192.181.2.1
```

**Skypie (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.3.2
	netmask 255.255.255.0
	gateway 192.181.3.1
```

**TottoLand (sebagai Client)**
```
auto eth0
iface eth0 inet static
	address 192.181.3.3
	netmask 255.255.255.0
	gateway 192.181.3.1
```

Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.181.0.0/16` pada router `Foosha`.

Ketikkan `echo nameserver 192.168.122.1 > /etc/resolv.conf` pada node ubuntu yang lain.

Restart semua node dan coba `ping google.com`. Berikut bukti `Loguetown` dapat mengakses internet.

![01-02](https://user-images.githubusercontent.com/31863229/138602128-fdeaf005-5b76-4cbf-b324-8701cba646af.PNG)

## Soal 2
Foosha sebagai DHCP Relay.

### Jawaban
**Pada Foosha**
- Install aplikasi isc-dhcp-relay.

  ```
  apt-get install isc-dhcp-relay -y
  ```
- Edit file `/etc/default/isc-dhcp-relay` seperti pada gambar berikut:

  ![02-01](https://user-images.githubusercontent.com/31863229/140716872-dfb89d0f-bcf6-4b3c-abd1-6730ad0d0e02.PNG)
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

## Soal 3
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169.

### Jawaban
**Pada Jipangu**
- Install aplikasi isc-dhcp-server.

  ```
  apt-get install isc-dhcp-server -y
  ```
- Edit file `/etc/default/isc-dhcp-server` seperti pada gambar berikut:

  ![03-01](https://user-images.githubusercontent.com/31863229/140718528-90cb2cce-6b39-4cbd-a561-9d8e23c8520e.PNG)
- Edit file `/etc/dhcp/dhcpd.conf` seperti pada gambar berikut:

  ![03-02](https://user-images.githubusercontent.com/31863229/140905888-4ff3871a-d81c-46c7-82c1-20177952a5e8.PNG)
- Restart isc-dhcp-server.

  ```
  service isc-dhcp-server restart
  ```

**Pada Loguetown**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

  ![03-03](https://user-images.githubusercontent.com/31863229/140718537-a8f636ee-9bd3-4423-aaf5-128920e858fc.PNG)
- Restart Loguetown dengan klik stop dan start pada node Loguetown.
- Lakukan testing pada IP dan nameserver.

  ![03-04](https://user-images.githubusercontent.com/31863229/140905902-162eb453-9291-443b-a9b4-e20e6fffb5ad.PNG)

**Pada Alabasta**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

  ![03-03](https://user-images.githubusercontent.com/31863229/140718537-a8f636ee-9bd3-4423-aaf5-128920e858fc.PNG)
- Restart Alabasta dengan klik stop dan start pada node Alabasta.
- Lakukan testing pada IP dan nameserver.

  ![03-05](https://user-images.githubusercontent.com/31863229/140905905-58b843fa-f3e5-4254-84d5-31dd83d1091f.PNG)

## Soal 4
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50.

### Jawaban
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti pada gambar berikut:

  ![04-01](https://user-images.githubusercontent.com/31863229/140906280-9ca9166d-bdac-430c-9657-ec2f6eb80e33.PNG)
- Restart isc-dhcp-server.

  ```
  service isc-dhcp-server restart
  ```

**Pada TottoLand**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

  ![03-03](https://user-images.githubusercontent.com/31863229/140718537-a8f636ee-9bd3-4423-aaf5-128920e858fc.PNG)
- Restart TottoLand dengan klik stop dan start pada node TottoLand.
- Lakukan testing pada IP dan nameserver.

  ![04-02](https://user-images.githubusercontent.com/31863229/140906285-c2f44252-7546-4b37-89ed-274608ea0062.PNG)

## Soal 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

### Jawaban
**Pada EniesLobby**
- Install aplikasi bind9.

  ```
  apt-get install bind9 -y
  ```
- Edit file `/etc/bind/named.conf.options` seperti pada gambar berikut:

  ![05-01](https://user-images.githubusercontent.com/31863229/140724873-b4a09064-8198-48c9-a18d-e501739872ba.PNG)
- Restart bind9.

  ```
  service bind9 restart
  ```

**Pada Loguetown**
- Lakukan ping `google.com`.

  ![05-02](https://user-images.githubusercontent.com/31863229/140724881-a4b97d8d-dfe9-400f-b41e-c24a8ff762bf.PNG)

**Pada Alabasta**
- Lakukan ping `google.com`.

  ![05-03](https://user-images.githubusercontent.com/31863229/140724883-5ea80709-8bfd-4701-a852-ee3f67ab4671.PNG)

**Pada Skypie**
- Lakukan ping `google.com`.

  ![05-04](https://user-images.githubusercontent.com/31863229/140724887-a509b39f-f006-4595-8cc7-88cce9038d13.PNG)

**Pada TottoLand**
- Lakukan ping `google.com`.

  ![05-05](https://user-images.githubusercontent.com/31863229/140724891-80d278a9-893f-4693-ab40-2b55ea662d4a.PNG)

## Soal 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

### Jawaban
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` pada bagian `default-lease-time` dan `max-lease-time` seperti pada gambar berikut:

  ![03-02](https://user-images.githubusercontent.com/31863229/140905888-4ff3871a-d81c-46c7-82c1-20177952a5e8.PNG)
  
  ![04-01](https://user-images.githubusercontent.com/31863229/140906280-9ca9166d-bdac-430c-9657-ec2f6eb80e33.PNG)
- Restart isc-dhcp-server.

  ```
  service isc-dhcp-server restart
  ```

## Soal 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69.

### Jawaban
**Pada Jipangu**
- Edit file `/etc/dhcp/dhcpd.conf` seperti pada gambar berikut:
  
  ![04-01](https://user-images.githubusercontent.com/31863229/140906280-9ca9166d-bdac-430c-9657-ec2f6eb80e33.PNG)
- Restart isc-dhcp-server.

  ```
  service isc-dhcp-server restart
  ```

**Pada Skypie**
- Edit file `/etc/network/interfaces` seperti pada gambar berikut:

  ![07-01](https://user-images.githubusercontent.com/31863229/140727156-ee850176-352b-48f6-b074-24d3ccb2afe7.PNG)
- Restart Skypie dengan klik stop dan start pada node Skypie.
- Lakukan testing pada IP dan nameserver.

  ![07-02](https://user-images.githubusercontent.com/31863229/140907284-7eeca7ab-b260-4e7a-bd2d-37a81c86d998.PNG)

## Soal 8
Pada Loguetown, proxy harus bisa diakses dengan nama `jualbelikapal.yyy.com` dengan port yang digunakan adalah 5000.

### Jawaban
**Pada EniesLobby**
- Edit file `/etc/bind/named.conf.local` seperti pada gambar berikut:

  ![08-01](https://user-images.githubusercontent.com/31863229/140797619-e89b875a-d7fb-4aa4-af76-73ecc19bdd67.PNG)
- Buat folder `modul3` di dalam `/etc/bind`.

  ```
  mkdir /etc/bind/modul3
  ```
- Copykan file `db.local` pada path `/etc/bind` ke dalam folder `modul3` yang baru saja dibuat dan ubah namanya menjadi `jualbelikapal.B09.com`.

  ```
  cp /etc/bind/db.local /etc/bind/modul3/jualbelikapal.B09.com
  ```
- Edit file `/etc/bind/modul3/jualbelikapal.B09.com` seperti pada gambar berikut:

  ![08-02](https://user-images.githubusercontent.com/31863229/140797631-bfc53c55-bf04-4f90-870e-d46c70c2745a.PNG)
- Restart bind9.

  ```
  service bind9 restart
  ```

**Pada Skypie**
- Install aplikasi apache, PHP, dan libapache2-mod-php7.0.

  ```
  apt-get install apache2 -y
  apt-get install php -y
  apt-get install libapache2-mod-php7.0 -y
  ```
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `jualbelikapal.B09.com.conf`.
- Edit file `jualbelikapal.B09.com.conf` seperti pada gambar berikut:

  ![08-03](https://user-images.githubusercontent.com/31863229/140797633-ee316577-487f-4e49-aed0-6125e6fa575c.PNG)
- Edit file `/etc/apache2/ports.conf` untuk mengaktifkan port 5000 seperti pada gambar berikut:

  ![08-04](https://user-images.githubusercontent.com/31863229/140797636-281db610-e474-475f-ba34-22de36af64a2.PNG)
- Aktifkan konfigurasi jualbelikapal.B09.com.

  ```
  a2ensite jualbelikapal.B09.com
  ```
- Restart apache.

  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Buatlah sebuah directory baru di dalam `/var/www` dengan nama `jualbelikapal.B09.com`.

  ```
  mkdir jualbelikapal.B09.com
  ```
- Masuk ke directory `/var/www/jualbelikapal.B09.com` dan buat file `index.php`.

  ![08-05](https://user-images.githubusercontent.com/31863229/140797638-cfef796f-6d9e-4055-819c-2e140002b613.PNG)

**Pada Water7**
- Install aplikasi squid dan apache2-utils.

  ```
  apt-get install squid -y
  apt-get install apache2-utils -y
  ```
- Backup terlebih dahulu file konfigurasi default yang disediakan Squid.

  ```
  mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
  ```
- Buat konfigurasi Squid baru.

  ```
  nano /etc/squid/squid.conf
  ```
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  ![08-06](https://user-images.githubusercontent.com/31863229/140797642-dfc9c00c-a440-4abb-a883-d4a9fd4e2f65.PNG)
- Tambahkan IP EniesLobby (192.181.2.2) pada file `/etc/resolv.conf`.
- Restart squid.

  ```
  service squid restart
  ```

**Pada Loguetown**
- Install aplikasi lynx.

  ```
  apt-get install lynx -y
  ```
- Aktifkan proxy dengan syntax berikut:

  ```
  export http_proxy="http://192.181.2.3:8080"
  ```
- Buka `jualbelikapal.B09.com:5000` menggunakan lynx.

  ![08-07](https://user-images.githubusercontent.com/31863229/140797644-7c3a18c5-a614-4471-b225-5544745e9220.PNG)

## Soal 9
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu `luffybelikapalyyy` dengan password `luffy_yyy` dan `zorobelikapalyyy` dengan password `zoro_yyy`.

### Jawaban
**Pada Water7**
- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username `luffybelikapalB09`. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi `luffy_B09`.

  ```
  htpasswd -cm /etc/squid/passwd luffybelikapalB09
  ```
- Jalankan perintah berikut untuk membuat akun autentikasi baru dengan username `zorobelikapalB09`. Kita akan diminta untuk memasukkan password baru dan confirm password tersebut diisi `zoro_B09`.

  ```
  htpasswd -m /etc/squid/passwd zorobelikapalB09
  ```
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  ![09-01](https://user-images.githubusercontent.com/31863229/140799670-82ff1946-ffef-4acf-9e68-3dccf798c1d0.PNG)
- Restart squid.

  ```
  service squid restart
  ```

**Pada Loguetown**
- Buka `jualbelikapal.B09.com:5000` menggunakan lynx.

  ![09-02](https://user-images.githubusercontent.com/31863229/140799675-bb858219-0201-4d42-a721-4a7840919cb4.PNG)
  ![09-03](https://user-images.githubusercontent.com/31863229/140799677-211b50a7-78e2-4ef5-aa1f-62e135c9c9f0.PNG)
  ![08-07](https://user-images.githubusercontent.com/31863229/140797644-7c3a18c5-a614-4471-b225-5544745e9220.PNG)

## Soal 10
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

### Jawaban
**Pada Water7**
- Mendefinisikan jadwal yang tidak restricted sebagai berikut:

  ```
  Senin		07:00 - 11:00
  Selasa		07:00 - 11:00, 17:00 - 23:59
  Rabu		00:00 - 03:00, 07:00 - 11:00, 17:00 - 23:59
  Kamis		00:00 - 03:00, 07:00 - 11:00, 17:00 - 23:59
  Jumat		00:00 - 03:00, 17:00 - 23:59
  Sabtu		00:00 - 03:00
  ```
- Buat file baru bernama `acl.conf` di folder squid.

  ```
  nano /etc/squid/acl.conf
  ```
- Edit file `/etc/squid/acl.conf` seperti pada gambar berikut:

  ![10-01](https://user-images.githubusercontent.com/31863229/140803916-825ec305-e62b-4513-a967-0d856884c78c.PNG)
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  ![10-02](https://user-images.githubusercontent.com/31863229/140803919-c9f52b4b-4d77-4869-9a26-a6494f4f624e.PNG)
- Restart squid.

  ```
  service squid restart
  ```

## Soal 11
Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses `google.com`, akan diredirect menuju `super.franky.yyy.com` dengan website yang sama pada soal shift modul 2. Web server `super.franky.yyy.com` berada pada node Skypie.

### Jawaban
**Pada EniesLobby**
- Edit file `/etc/bind/named.conf.local` seperti pada gambar berikut:

  ![11-01](https://user-images.githubusercontent.com/31863229/140810695-adac3260-5da3-44de-9475-b53d9946eff5.PNG)
- Buat folder `kaizoku` di dalam `/etc/bind`.

  ```
  mkdir /etc/bind/kaizoku
  ```
- Copykan file `db.local` pada path `/etc/bind` ke dalam folder `kaizoku` yang baru saja dibuat dan ubah namanya menjadi `super.franky.B09.com`.

  ```
  cp /etc/bind/db.local /etc/bind/kaizoku/super.franky.B09.com
  ```
- Edit file `/etc/bind/kaizoku/super.franky.B09.com` seperti pada gambar berikut:

  ![11-02](https://user-images.githubusercontent.com/31863229/140810704-58d4ac56-1320-478b-97ad-814f40a1e2a5.PNG)
- Restart bind9.

  ```
  service bind9 restart
  ```

**Pada Skypie**
- Pindah ke directory `/etc/apache2/sites-available`.
- Copy file `000-default.conf` menjadi file `super.franky.B09.com.conf`.
- Edit file `super.franky.B09.com.conf` seperti pada gambar berikut:

  ![11-03](https://user-images.githubusercontent.com/31863229/140810707-fb849aa4-9180-4b03-b71c-96e1ad2f1099.PNG)
- Aktifkan konfigurasi super.franky.B09.com.

  ```
  a2ensite super.franky.B09.com
  ```
- Restart apache.

  ```
  service apache2 restart
  ```
- Pindah ke directory `/var/www`.
- Download file zip menggunakan `wget`.

  ```
  wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip
  ```
- Lakukan unzip.

  ```
  unzip super.franky.zip
  ```
- Rename folder `super.franky` menjadi `super.franky.B09.com` dan terdapat isi file seperti pada gambar berikut:

  ![11-04](https://user-images.githubusercontent.com/31863229/140810714-d3d876f9-d957-4d56-9ca9-d015a3554a22.PNG)

**Pada Water7**
- Buat file bernama `restrict-sites.acl` di folder squid.
- Tambahkan alamat url `google.com` yang akan diredirect.
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  ![11-05](https://user-images.githubusercontent.com/31863229/140810715-bc5de660-2399-4f2b-9708-f8331acfeb8a.PNG)
- Restart squid.

  ```
  service squid restart
  ```

**Pada Loguetown**
- Buka `google.com` menggunakan lynx.

  ![11-06](https://user-images.githubusercontent.com/31863229/140810718-9175a128-fb96-4558-a5f4-b8f66c92014a.PNG)

## Soal 12
Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di `super.franky.yyy.com`. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps.

### Jawaban
**Pada Water7**
- Buat file bernama `file_download` di folder squid.
- Tambahkan format gambar yang akan dibatasi kecepatannya pada `/etc/squid/file_download`.

  ```
  \.jpg$
  \.png$
  ```
- Buat file bernama `acl-bandwidth.conf` di folder squid.
- Edit file `/etc/squid/acl-bandwidth.conf` seperti pada gambar berikut:

  ![12-01](https://user-images.githubusercontent.com/31863229/140817038-d1f3eef1-dbc2-4c77-a123-a9c899af8d29.PNG)
- Edit file `/etc/squid/squid.conf` seperti pada gambar berikut:

  ![12-02](https://user-images.githubusercontent.com/31863229/140817047-aeb3fc8d-d93f-45e5-b98b-ba65db5ef5b5.PNG)
- Restart squid.

  ```
  service squid restart
  ```

**Pada Loguetown**
- Install aplikasi speedtest-cli.

  ```
  apt install speedtest-cli -y
  ```
- Jalankan script `export PYTHONHTTPSVERIFY=0` untuk menonaktifkan verifikasi certificate pada saat menjalankan script Python.
- Eksekusi speed test dengan perintah `speedtest`.

## Soal 13
Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya.

### Jawaban
**Pada Loguetown**
- Eksekusi speed test dengan perintah `speedtest`.

  ![13-01](https://user-images.githubusercontent.com/31863229/140817737-32d24333-5c32-44ac-b18a-15b383cc2c79.PNG)

## Kendala
1. Sedikit kesulitan mengenai DHCP Relay pada soal 2 karena tidak ada di modul.
2. Sedikit kesulitan pada soal 8 karena hasil lynx tidak dapat tampil. Solusinya menambahkan IP EniesLobby pada file `/etc/resolv.conf` di Water7.
3. Kesulitan pada soal 10 untuk mengecek pembatasan waktu akses.
4. Kesulitan pada soal 12 karena speedtest terkendala proxy (http error 407: proxy authentication required).

## Pembagian Tugas
|Nama                   |Soal  |
|:---------------------:|:----:|
|Maximilian H M Lingga  ||
|Izzulhaq Fawwaz Syauqiy||
|Aldo Yaputra Hartono   ||
