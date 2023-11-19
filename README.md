# Jarkom-Modul-3-E16-2023
Laporan Praktikum Jaringan Komputer Modul 3 Tahun 2023

## Anggota Kelompok

| NRP        | Nama                 |
| ---        | ---                  |
| 5025211058 | Nadya Zuhria Amana   |
| 5025211127 | Nadif Mustafa        |

## Daftar Isi

# Lapres Praktikum 3
## Daftar Isi
- [Topologi](#Topologi)
- [Network Configuration](#network-configuration)
- [Files](#Files)
- [Soal Praktikum](#Soal-Praktikum)
  - [No 0](#No-0)
  - [No 1](#No-1)
  - [No 2](#No-2)
  - [No 3](#No-3)
  - [No 4](#No-4)
  - [No 5](#No-5)
  - [No 6](#No-6)
  - [No 7](#No-7)
  - [No 8](#No-8)
  - [No 9](#No-9)
  - [No 10](#No-10)
  - [No 11](#No-11)
  - [No 12](#No-12)
  - [No 13](#No-13)
  - [No 14](#No-14)
  - [No 15](#No-15)
  - [No 16](#No-16)
  - [No 17](#No-17)
  - [No 18](#No-18)
  - [No 19](#No-19)
  - [No 20](#No-20)
  - [Grimoire](#Grimoire)
 
## Topologi
Berikut adalah topologi yang digunakan pada praktikum modul 3

![](/images/topologi.png)

## Network Configuration
- **Aura (Router)**
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
	  address 192.214.1.0
	  netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
	  address 192.214.2.0
	  netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
	  address 192.214.3.0
	  netmask 255.255.255.0

  auto eth4
  iface eth4 inet static
	  address 192.214.4.0
	  netmask 255.255.255.0
  ```
- **Himmel (DHCP Server)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.1.1
	  netmask 255.255.255.0
	  gateway 192.220.1.0
	 
  ```
- **Heiter (DNS Server)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.1.2
	  netmask 255.255.255.0
	  gateway 192.220.1.0
	 
  ```
- **Denken (Database Server)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.2.1
	  netmask 255.255.255.0
	  gateway 192.220.2.0

  ```
- **Eisen (Load Balancer)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.2.2
	  netmask 255.255.255.0
	  gateway 192.220.2.0
	
  ```
- **Sein, Stark, Revolte, Richter (Clients) **
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
- **Lawine, Linie, Lugner - PHP Worker**

  - **Lawine**
    ```
	auto eth0
	iface eth0 inet static
		address 192.214.3.1
		netmask 255.255.255.0
		gateway 192.214.3.0

	hwaddress ether 1a:ea:3e:d1:a3:83
    ```
  - **Linie**
    ```
	auto eth0
	iface eth0 inet static
		address 192.214.3.2
		netmask 255.255.255.0
		gateway 192.214.3.0

	hwaddress ether 2a:d0:fa:ad:f6:20
 
    ```
  - **Lugner**
    ```
	auto eth0
	iface eth0 inet static
		address 192.214.3.3
		netmask 255.255.255.0
	gateway 192.214.3.0

	hwaddress ether 7a:b6:0f:b7:a7:70
    ```
- **Frieren, Flamme, Fern - Laravel Worker**

  - **Frieren**
    ```
    auto eth0
	iface eth0 inet static
	address 192.214.4.1
	netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether d6:7a:43:23:c0:f5
    ```
  - **Flamme**
    ```
   	auto eth0
	iface eth0 inet static
		address 192.214.4.2
		netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether 16:12:b5:cf:e8:cc
    ```
  - **Fern**
    ```
    	auto eth0
	iface eth0 inet static
		address 192.214.4.1
		netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether d6:7a:43:23:c0:f5
    ```
## Files
Sebelum menjawab soal - soal yang diberikan, pastikan file - file ini ada pada `/root` masing - masing node  
- **Aura - (Router) **  

- **Himmel - (DHCP Server)**

- **Heiter - (DNS Server) **

- **Denken - (Database Server) **

- **Eisen - (Load Balancer)**

- **Sein, Stark, Revolte, Richter (Clients) **

- **Lawine, Linie, Lugner (PHP Worker) **

- **Frieren, Flamme, Fern (Laravel Worker)**


## Soal Praktikum
### No 0
> Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1

#### Answer:  


#### Testing:  


### No 1
> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan. Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.

#### Answer:  


#### Testing:  


![](/images/ipa-sein.png)


### No 2
> Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

#### Answer:  

#### Testing:  

### No 3
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

#### Answer:  

#### Testing:  

### No 4
> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

#### Answer:  

#### Testing:  

### No 5
> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

#### Answer:  

#### Testing:  

### No 6
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3

#### Answer:  

#### Testing:  

### No 7
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second

#### Answer:  

#### Testing:  

### No 8
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut: Nama Algoritma Load Balancer, Report hasil testing pada Apache Benchmark, Grafik request per second untuk masing masing algoritma., Analisis

#### Answer:  

#### Testing:  

### No 9
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire

#### Answer:  

#### Testing:  

### No 10
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

#### Answer:  

#### Testing:  

### No 11
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id

#### Answer:  

#### Testing:  

### No 12
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168

#### Answer:  

#### Testing:  

### No 13
> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern

#### Answer:  

#### Testing:  

### No 14
> Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

#### Answer:  

#### Testing:  

### No 15
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. POST /auth/register

#### Answer:  

#### Testing:  

### No 16
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. POST /auth/login

#### Answer:  

#### Testing:  

### No 17
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. GET /me

#### Answer:  

#### Testing:  

### No 18
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern

#### Answer:  

#### Testing:  

### No 19
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan pm.max_children, pm.start_servers, pm.min_spare_servers, pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire

#### Answer:  

#### Testing:  

### No 20
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

#### Answer:  

#### Testing:  

### Grimoire
