# Jarkom-Modul-3-B09-2021

Repositori Praktikum Jarkom Modul 3

|NRP           |Nama                   |
|:------------:|:---------------------:|
|05111940000084|Aldo Yaputra Hartono   |
|05111840000023|Izzulhaq Fawwaz Syauqiy|
|05111940000092|Maximilian H M Lingga  |

## Soal 1
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server.

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

  ![02-01](https://user-images.githubusercontent.com/31863229/140704116-bbc56e08-a722-460c-96b1-1f22098de936.PNG)
- Restart isc-dhcp-relay.

  ```
  service isc-dhcp-relay restart
  ```

## Soal 3


### Jawaban


## Soal 4


### Jawaban


## Soal 5


### Jawaban


## Soal 6


### Jawaban


## Soal 7


### Jawaban


## Soal 8


### Jawaban


## Soal 9


### Jawaban


## Soal 10


### Jawaban


## Soal 11


### Jawaban


## Soal 12


### Jawaban


## Soal 13


### Jawaban


## Kendala


## Pembagian Tugas
|Nama                   |Soal  |
|:---------------------:|:----:|
|Maximilian H M Lingga  ||
|Izzulhaq Fawwaz Syauqiy||
|Aldo Yaputra Hartono   ||
