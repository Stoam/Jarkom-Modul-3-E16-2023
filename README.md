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
  - [No 1](#soal-1)
  - [No 2](#soal-2)
  - [No 3](#soal-3)
  - [No 4](#soal-4)
  - [No 5](#soal-5)
  - [No 6](#soal-6)
  - [No 7](#soal-7)
  - [No 8](#soal-8)
  - [No 9](#soal-9)
  - [No 10](#soal-10)
  - [No 11](#soal-11)
  - [No 12](#soal-12)
  - [No 13](#soal-13)
  - [No 14](#soal-14)
  - [No 15](#soal-15)
  - [No 16](#soal-16)
  - [No 17](#soal-17)
  - [No 18](#soal-18)
  - [No 19](#soal-19)
  - [No 20](#soal-20)
  - [Grimoire](#Grimoire)
 
## Topologi
Berikut adalah topologi yang digunakan pada praktikum modul 3

![image](https://github.com/Stoam/Jarkom-Modul-3-E16-2023/assets/58579201/c1c9d365-833d-4193-a3f9-7782a65073cb)

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
	  gateway 192.214.1.0
	 
  ```
- **Heiter (DNS Server)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.1.2
	  netmask 255.255.255.0
	  gateway 192.214.1.0
	 
  ```
- **Denken (Database Server)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.2.1
	  netmask 255.255.255.0
	  gateway 192.214.2.0

  ```
- **Eisen (Load Balancer)**
  ```
  auto eth0
  iface eth0 inet static
	  address 192.214.2.2
	  netmask 255.255.255.0
	  gateway 192.214.2.0
	
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

	hwaddress ether 9e:9f:dd:f4:ac:68
    ```
  - **Linie**
    ```
	auto eth0
	iface eth0 inet static
		address 192.214.3.2
		netmask 255.255.255.0
		gateway 192.214.3.0

	hwaddress ether 66:1b:73:09:0b:07
 
    ```
  - **Lugner**
    ```
	auto eth0
	iface eth0 inet static
		address 192.214.3.3
		netmask 255.255.255.0
	gateway 192.214.3.0

	hwaddress ether 86:89:ee:02:8d:5e
    ```
- **Frieren, Flamme, Fern - Laravel Worker**

  - **Frieren**
    ```
    auto eth0
	iface eth0 inet static
	address 192.214.4.1
	netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether 2a:19:54:95:f7:cf
    ```
  - **Flamme**
    ```
   	auto eth0
	iface eth0 inet static
		address 192.214.4.2
		netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether 0e:c3:2e:4c:95:bd
    ```
  - **Fern**
    ```
    	auto eth0
	iface eth0 inet static
		address 192.214.4.1
		netmask 255.255.255.0
	gateway 192.214.4.0

	hwaddress ether f6:35:c1:d2:5b:44
    ```
## Files
Sebelum menjawab soal - soal yang diberikan, pastikan file - file ini ada pada `/root` masing - masing node

## DHCP Relay (Aura)

```bash
#!/bin/bash

apt-get update
if ! dpkg -l | grep -q isc-dhcp-relay; then
    apt-get install isc-dhcp-relay -y
fi

echo "
SERVERS=\"192.214.1.1\"
INTERFACES=\"eth1 eth2 eth3 eth4\"
OPTIONS=
" > /etc/default/isc-dhcp-relay

echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf

service isc-dhcp-relay restart
```

[<< Daftar Isi](#daftar-isi)

## Database Server (Denken)

```bash
echo 'nameserver 192.168.122.1  # IP NAT' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y

echo '

# The MariaDB configuration file
#
# The MariaDB/MySQL tools read configuration files in the following order:
# 1. "/etc/mysql/mariadb.cnf" (this file) to set global defaults,
# 2. "/etc/mysql/conf.d/*.cnf" to set global options.
# 3. "/etc/mysql/mariadb.conf.d/*.cnf" to set MariaDB-only options.
# 4. "~/.my.cnf" to set user-specific options.
#
# If the same option is defined multiple times, the last one will apply.
#
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.

#
# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address' > /etc/mysql/my.cnf

service mysql restart
```

[<< Daftar Isi](#daftar-isi)

## Load Balancer (Eisen)

```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install nginx -y
apt-get install apache2-utils -y
apt-get install lynx -y

echo '
upstream backend {
 	server 192.214.3.1;
 	server 192.214.3.2;
  server 192.214.3.3;
}

server {
 	listen 80;
 	server_name granz.channel.e16.com;

 	location / {
 	    proxy_pass http://backend;
 	}
    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}

' > /etc/nginx/sites-available/default

ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
service nginx restart
```

[<< Daftar Isi](#daftar-isi)

## Client (Stark, Sein, Revolte, Richter)

```bash
echo 'nameserver 192.168.122.1  # IP NAT' > /etc/resolv.conf

apt-get update

if ! dpkg -l | grep -q dnsutils; then
    apt-get install dnsutils -y
fi

if ! dpkg -l | grep -q lynx; then
    apt-get install lynx -y
fi

if ! dpkg -l | grep -q apache2-utils; then
    apt-get install apache2-utils -y
fi

if ! dpkg -l | grep -q htop; then
    apt-get install htop -y
fi

if ! dpkg -l | grep -q less; then
    apt-get install less -y
fi

if ! dpkg -l | grep -q jq; then
    apt-get install jq -y
fi
```


## Worker PHP (Lawine, Linie, Lugner)

```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install ca-certificates wget unzip nginx php php-fpm lynx -y
apt-get install htop -y

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O granz.channel.yyy.com.zip
unzip granz.channel.yyy.com.zip
mkdir -p /var/www/granz.channel.e16.com
mv modul-3/* /var/www/granz.channel.e16.com
rm granz.channel.yyy.com.zip
rm -rf modul-3

echo "
server {

    listen 80;

    root /var/www/granz.channel.e16.com;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files \$uri \$uri/ /index.php?\$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php\$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/granz.channel.e16.com_error.log;
    access_log /var/log/nginx/granz.channel.e16.com_access.log;
}
" > /etc/nginx/sites-available/default

ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
service php7.0-fpm start
service nginx restart
```

[<< Daftar Isi](#daftar-isi)

## Worker Laravel (Frieren, Flamme, Fren)

```bash
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y
apt-get install htop -y

apt -y install lsb-release apt-transport-https ca-certificates
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
apt update

apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y
apt-get install wget -y

service nginx start
service php8.0-fpm start

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer

apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update

cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.214.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoke16
DB_USERNAME=kelompoke16
DB_PASSWORD=passworde16

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate:fresh
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage
```

## Soal Praktikum
## Soal 1

> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

### Solusi

- Atur konfigurasi setiap node sesuai dengan setup di atas
- Untuk register domain pada `riegel.canyon.e16.com` dan `granz.chanel.e16.com` yang mengarah ke worker dengan ip `192.168.x.`, tambahkan script berikut dalam Heiter (DNS Server)
  ```python
  echo "
  zone \"granz.channel.e16.com\" {
      type master;
      file \"/etc/bind/granz/granz.channel.e16.com\";
  };

  zone \"riegel.canyon.e16.com\" {
      type master;
      file \"/etc/bind/riegel/riegel.canyon.e16.com\";
  };
  " > /etc/bind/named.conf.local

  echo "
  options {
          directory \"/var/cache/bind\";

          forwarders {
                  192.168.122.1; // IP NAT
          };

          allow-query{any;};

          auth-nxdomain no;
          listen-on-v6 { any; };
  };
  " > /etc/bind/named.conf.options

  mkdir -p /etc/bind/granz
  echo "
  ;
  ; BIND data file for local loopback interface
  ;
  \$TTL    604800
  @       IN      SOA     granz.channel.e16.com. root.granz.channel.e16.com. (
                       2023111401         ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @                 IN      NS      granz.channel.e16.com.
  @                 IN      A       192.214.3.3 ; IP PHP Worker (Lawine)
  www               IN      CNAME   granz.channel.e16.com.
  " > /etc/bind/granz/granz.channel.e16.com

  mkdir -p /etc/bind/riegel
  echo "
  ;
  ; BIND data file for local loopback interface
  ;
  \$TTL    604800
  @       IN      SOA     riegel.canyon.e16.com. root.riegel.canyon.e16.com. (
                       2023111401         ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @           IN      NS      riegel.canyon.e16.com.
  @           IN      A       192.214.4.3 ; IP Laravel Worker (Frieren)
  www         IN      CNAME   riegel.canyon.e16.com.
  " > /etc/bind/riegel/riegel.canyon.e16.com
  ```

### Screenshot Hasil (Client Sein)

![image]()

## Soal 2

> 2. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 **(2)**

### Solusi

- Tambahkan Konfigurasi DHCP pada Himmel (DHCP Server) di /etc/dhcp/dhcpd.conf
  ```python
  # eth3
  subnet 192.214.3.0 netmask 255.255.255.0 {
      range 192.214.3.16 192.214.3.32;
      range 192.214.3.64 192.214.3.80;
      option routers 192.214.3.200;
  }
  ```

### Screenshot Hasil

- Client Richter (Switch 3)
  ![image]()

## Soal 3

> 3. Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 **(3)**

### Solusi

- Tambahkan Konfigurasi DHCP pada Himmel (DHCP Server) di /etc/dhcp/dhcpd.conf
  ```python
  # eth4
  subnet 192.214.4.0 netmask 255.255.255.0 {
      range 192.214.4.12 192.214.4.20;
      range 192.214.4.160 192.214.4.168;
      option routers 192.214.4.200;
  }
  ```

### Screenshot Hasil

- Client Sein (Switch 4)
  ![image]()

## Soal 4

> 4. Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut **(4)**

### Solusi

- Tambahkan Konfigurasi DHCP untuk `option broadcast-address` dan `option domain-name-servers` pada /etc/dhcp/dhcpd.conf
  ```python
  # eth3
  subnet 192.214.3.0 netmask 255.255.255.0 {
      range 192.214.3.16 192.214.3.32;
      range 192.214.3.64 192.214.3.80;
      option routers 192.214.3.200;
      option broadcast-address 192.214.3.255;
      option domain-name-servers 192.214.1.2;
  }

  # eth4
  subnet 192.214.4.0 netmask 255.255.255.0 {
      range 192.214.4.12 192.214.4.20;
      range 192.214.4.160 192.214.4.168;
      option routers 192.214.4.200;
      option broadcast-address 192.214.4.255;
      option domain-name-servers 192.214.1.2;
  }
  ```
- Restart isc-dhcp-server
  ```python
  service isc-dhcp-server restart
  ```
- Jalankan Setup konfigurasi DHCP Relay (Aura)

### Screenshot Hasil

- Dari Client Sein
  ![image]()

## Soal 5

> 5. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit **(5)**

### Solusi

- Di DHCP Server, tambahkan `default-lease-time` dan `max-lease-team` pada /etc/dhcp/dhcpd.conf
  ```python
  # eth3
  subnet 192.214.3.0 netmask 255.255.255.0 {
      range 192.214.3.16 192.214.3.32;
      range 192.214.3.64 192.214.3.80;
      option routers 192.214.3.200;
      option broadcast-address 192.214.3.255;
      option domain-name-servers 192.214.1.2;
      default-lease-time 180;
      max-lease-time 5760;
  }

  # eth4
  subnet 192.214.4.0 netmask 255.255.255.0 {
      range 192.214.4.12 192.214.4.20;
      range 192.214.4.160 192.214.4.168;
      option routers 192.214.4.200;
      option broadcast-address 192.214.4.255;
      option domain-name-servers 192.214.1.2;
      default-lease-time 720;
      max-lease-time 5760;
  }
  ```

### Screenshot Hasil

- Client Richter (Switch 3)
  ![image]()
- Client Sein (Switch 4)
  ![image]()

[<< Daftar Isi](#daftar-isi)

## Soal 6

> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website [berikut](https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing) dengan menggunakan php 7.3. **(6)**

### Solusi

- Lakukan apt install dengan modul yang dibutuhkan
  ```python
  apt-get update
  apt-get install ca-certificates wget unzip nginx php php-fpm lynx -y
  ```
- Download file → unzip → pindahkan folder ke /var/www/
  ```python
  wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1' -O granz.channel.yyy.com.zip
  unzip granz.channel.yyy.com.zip
  mkdir -p /var/www/granz.channel.e16.com
  mv modul-3/* /var/www/granz.channel.e16.com
  rm granz.channel.yyy.com.zip
  rm -rf modul-3
  ```
- Tambahkan konfigurasi pada /etc/nginx/sites-available/default
  ```python
  echo "
  server {

      listen 80;

      root /var/www/granz.channel.e16.com;

      index index.php index.html index.htm;
      server_name _;

      location / {
              try_files \$uri \$uri/ /index.php?\$query_string;
      }

      location ~ \.php\$ {
              include snippets/fastcgi-php.conf;
              fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
      }

      location ~ /\.ht {
              deny all;
      }

      error_log /var/log/nginx/granz.channel.e16.com_error.log;
      access_log /var/log/nginx/granz.channel.e16.com_access.log;
  }
  " > /etc/nginx/sites-available/default
  ```
- Start service php7.0-fpm dan nginx
  ```python
  ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
  service php7.0-fpm start
  service nginx restart
  ```

### Screenshot Hasil

- Pada masing masing Worker, berikan perintah `lynx localhost`
  ![image]()

## Soal 7

> Kepala suku dari [Bredt Region](https://frieren.fandom.com/wiki/Bredt_Region) memberikan resource server sebagai berikut: 1. Lawine, 4GB, 2vCPU, dan 80 GB SSD.2. Linie, 2GB, 2vCPU, dan 50 GB SSD. 3. Lugner 1GB, 1vCPU, dan 25 GB SSD. aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

### Solusi

- Pada Heiter (DNS Server), modifikasi untuk mengarahkan `granz.channel.e16.com` dan `riegel.canyon.e16.com` ke Load Balancer Eisen
  ```python
  echo "
  ;
  ; BIND data file for local loopback interface
  ;
  \$TTL    604800
  @       IN      SOA     granz.channel.e16.com. root.granz.channel.e16.com. (
                       2023111401         ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @                 IN      NS      granz.channel.e16.com.
  @                 IN      A       192.214.2.2 ; IP Eisen
  www               IN      CNAME   granz.channel.e16.com.
  " > /etc/bind/granz/granz.channel.e16.com

  echo "
  ;
  ; BIND data file for local loopback interface
  ;
  \$TTL    604800
  @       IN      SOA     riegel.canyon.e16.com. root.riegel.canyon.e16.com. (
                       2023111401         ; Serial
                           604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                           604800 )       ; Negative Cache TTL
  ;
  @           IN      NS      riegel.canyon.e16.com.
  @           IN      A       192.214.2.2 ; IP Eisen
  www         IN      CNAME   riegel.canyon.e16.com.
  " > /etc/bind/riegel/riegel.canyon.e16.com

  service bind9 restart
  ```
- Pada Eisen, berikan konfigurasi LB dengan nginx berdasarkan informasi spesifikasi yang diberikan pada soal sebagai berikut: lawine 4GB 2vCPU maka weight=8, linie 2GB 2vCPU maka weight=4, dan lugner 1GB 1vCPU maka weight=1
  ```python
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf

  apt-get update
  apt-get install nginx -y

  echo '
  upstream backend {
   	server 192.214.3.3 weight=8;
   	server 192.214.3.2 weight=4;
    server 192.214.3.1 weight=1;
  }

  server {
   	listen 80;
   	server_name granz.channel.e16.com;

   	location / {
   	    proxy_pass http://backend;
   	}
      error_log /var/log/nginx/lb_error.log;
      access_log /var/log/nginx/lb_access.log;
  }

  ' > /etc/nginx/sites-available/default

  ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
  service nginx restart
  ```
- Pada Client, install modul untuk melakukan testing Apache Benchmark
  ```python
  if ! dpkg -l | grep -q apache2-utils; then
      apt-get install apache2-utils -y
  fi
  ```
- Lakukan testing pada client dengan perintah berikut
  ```python
  ab -n 1000 -c 100 http://www.granz.channel.e16.com/
  ```

### Screenshot Hasil

- Load Balancing pada Client (Richter - Revolte - Sein)
  ![image]()
- Hasil Testing pada Richter (Client)
  ![image]()
  ![image]()
  ![image]()

## Soal 8

> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:

1. Nama Algoritma Load Balancer
2. Report hasil testing pada Apache Benchmark
3. Grafik request per second untuk masing masing algoritma.
4. Analisis **(8)**

### Solusi

- Lakukan Konfigurasi untuk masing masing algoritma Load Balancer pada Esien
  - **Round Robin**
    ```python
    upstream backend {
     	server 192.214.3.1;
     	server 192.214.3.2;
      server 192.214.3.3;
    }
    ```
  - **Least-connection**
    ```python
    upstream backend {
    	least_conn;
     	server 192.214.3.1;
     	server 192.214.3.2;
      server 192.214.3.3;
    }
    ```
  - **IP Hash**
    ```python
    upstream backend {
    	ip_hash;
     	server 192.214.3.1;
     	server 192.214.3.2;
      server 192.214.3.3;
    }
    ```
  - **Generic Hash**
    ```python
    upstream backend {
    	hash $request_uri consistent;
     	server 192.214.3.1;
     	server 192.214.3.2;
      server 192.214.3.3;
    }
    ```
- Lakukan Testing pada Client dengan perintah berikut
  ```python
  ab -n 200 -c 10 http://www.granz.channel.e16.com/
  ```

### Screenshot Hasil

- **Round Robin**
  ![image]()
  ![image]()
- **Least-connection**
  ![image]()
  ![image]()
- **IP Hash**
  ![image]()
  ![image]()
- **Generic Hash**
  ![image]()
  ![image]()

## Soal 9

> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. **(9)**

### Solusi

- Lakukan Konfigurasi untuk masing masing algoritma Load Balancer pada Esien
  - **Round Robin 1**
    ```python
    upstream backend {
     	server 192.214.3.1;
    }
    ```
  - **Round Robin 2**
    ```python
    upstream backend {
     	server 192.214.3.1;
     	server 192.214.3.2;
    }
    ```
  - **Round Robin 3**
    ```python
    upstream backend {
     	server 192.214.3.1;
     	server 192.214.3.2;
      server 192.214.3.3;
    }
    ```
- Lakukan Testing pada Client dengan perintah berikut
  ```python
  ab -n 100 -c 10 http://www.granz.channel.e16.com/
  ```

### Screenshot Hasil

- **Round Robin 1**
  ![image]()
  ![image]()
- **Round Robin 2**
  ![image]()
  ![image]()
- **Round Robin 3**
  ![image]()
  ![image]()

## Soal 10

> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ **(10)**

### Solusi

- Buat folder dan masukkan Autentikasi
  ```python
  mkdir /etc/nginx/rahasisakita
  htpasswd -c -b /etc/nginx/rahasisakita/htpasswd netics ajke16
  ```
- Tambahkan Konfigurasi pada /etc/nginx/sites-available/default
  ```python
  location / {
   	    proxy_pass http://backend;
        auth_basic \"Autitenikasi\";
        auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
  }
  ```

### Screenshot Hasil

- Saat mengakses granz.chanel.e16.com
  ![image]()
  ![image]()
  ![image]()
  ![image]()

## Soal 11

> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman [https://www.its.ac.id](https://www.its.ac.id/). **(11) hint: (proxy_pass)**

### Solusi

- Tambahkan Konfigurasi location ~ /its pada /etc/nginx/sites-available/default
  ```python
  location ~ /its {
      proxy_pass https://www.its.ac.id;
  }
  ```
- Untuk pengetesan dapat menggunakan /its pada lynx
  ```python
  lynx www.granz.channel.e16.com/its
  ```

### Screenshot Hasil

- Pada Richter (Client)
  ![image]()

## Soal 12

> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. **(12)**

### Solusi

- Tambahkan Konfigurasi /location untuk memfix ip client pada /etc/nginx/sites-available/default
  ```python
  location / {
          allow 192.214.3.69;
          allow 192.214.3.70;
          allow 192.214.4.167;
          allow 192.214.4.168;
          deny all;
  	 	    proxy_pass http://backend;
          auth_basic \"Autitentikasi\";
          auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
   	}
  ```

- Tambahkan konfigurasi untuk Fix IP client pada DHCP Server (Himmel)
  ```python
  host Revolte {
      hardware ethernet 0b:14:21:14:bd:e0;
      fixed-address 192.214.3.69;
  }

  host Richter {
      hardware ethernet 38:6b:5d:9a:d1:43;
      fixed-address 192.214.3.70;
  }

  host Sein {
      hardware ethernet 12:d6:25:38:11:27;
      fixed-address 192.214.4.167;
  }

  host Stark {
      hardware ethernet 32:34:a8:48:6d:10;
      fixed-address 192.214.4.168;
  }
  ```

- Lalu lakukan konfigurasi juga terhadap masing masing client seperti ini 
```python
  auto eth0
  iface eth0 inet dhcp
  hwaddress ether [sesuaikan dengan konfigurasi di atas]
```

### Screenshot Hasil

- Pada Richter dengan IP acak 192.214.3.24; `Tidak bisa Akses`
  ![image]()
  ![image]()
- Untuk membuktikan Pemberian akses, IP dari Client di Fix kan agar sesuai dengan hak akses pada LB. Lalu dilakukan pengujian ulang `berhasil`
  ![image]()

## Soal 13

> Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern

### Solusi

- Jalankan setup konfigurasi pada Denken (Database Server) dan Worker Laravel
- Masuk ke mariadb `mysql -u root -p` dan jalankan query berikut
  ```python
  CREATE USER 'kelompoke16'@'%' IDENTIFIED BY 'passworde16';
  CREATE USER 'kelompok1e6'@'localhost' IDENTIFIED BY 'password1e6';
  CREATE DATABASE dbkelompok1e6;
  GRANT ALL PRIVILEGES ON *.* TO 'kelompoke16'@'%';
  GRANT ALL PRIVILEGES ON *.* TO 'kelompoke16'@'localhost';
  FLUSH PRIVILEGES;
  ```
- Restart mysql `service mysql restart`
- Pada Worker yang sudah disetup mariadb-client, lakukan testing untuk mengakses database
  ```python
  mysql --host=192.214.2.1 --port=3306 --user=kelompoke16 --password=passworde16
  ```

### Screenshot Hasil

- Query SQL
  ![image]()
- Testing akses databases dari Worker
  ![image]()

## Soal 14

> Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

### Solusi

- Lakukan setup pada setiap worker untuk menginstall php8 composer serta file Laravel-github
- Pada `php artisan migrate` terdapat eror yang dapat diselesaikan dengan menambahkan kode ini pada /app/Providers/AppServiceProvider.php
  ```python
  use Illuminate\Support\Facades\Schema;

  /**
   * Bootstrap any application services.
   *
   * @return void
   */
  public function boot()
  {
      Schema::defaultStringLength(191);
  }
  ```
- Setelah berhasil konfigurasi laravel, pada masing masing worker, lakukan setting untuk nginx sesuai dengan port yang telah ditentukan masing masing worker
  ```python
  echo 'server {
      listen 8001; # Fern
      listen 8002; # Flamme
      listen 8003; # Frieren

      root /var/www/laravel-praktikum-jarkom/public;

      index index.php index.html index.htm;
      server_name _;

      location / {
              try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
      }

      location ~ /\.ht {
              deny all;
      }

      error_log /var/log/nginx/error.log;
      access_log /var/log/nginx/access.log;
  }' > /etc/nginx/sites-available/default

  ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
  service php8.0-fpm start
  service nginx restart
  ```
- Restart Nginx dan php8.0-fpm

### Screenshot Hasil

- Jalankan `lynx localhost:[port]` pada setiap worker
  ![image]()

## Soal 15
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. POST /auth/register

### Solusi

- Buat file json untuk data yang akan di post
  ```python
  echo '
  {
    "username": "kelompoke16",
    "password": "passworde16"
  }' > register.json
  ```
- Lakukan pengujian apache benchmark pada salah satu client (Sein) dengan perintah
  ```python
  ab -n 100 -c 10 -p register.json -T application/json http://192.214.4.1:8001/api/auth/register
  ```
- Untuk mendapatkan response, lakukan curl seperti berikut
  ```python
  curl -X POST -H "Content-Type: application/json" -d @register.json http://192.214.4.1:8001/api/auth/register -o register_response.txt
  ```

### Screenshot Hasil

- Pengujian apache benchmark dari Sein (Client) ke Fern (Worker Laravel)
  ![image]()
- Response
  ![image]()

## Soal 16
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. POST /auth/login

### Solusi

- Buat file json untuk data yang akan di post
  ```python
  echo '
  {
    "username": "kelompoke16",
    "password": "passworde16"
  }' > register.json
  ```
- Lakukan pengujian apache benchmark pada salah satu client (Sein) dengan perintah
  ```python
  ab -n 100 -c 10 -p register.json -T application/json http://192.214.4.1:8001/api/auth/login
  ```
- Untuk mendapatkan response, lakukan curl seperti berikut
  ```python
  curl -X POST -H "Content-Type: application/json" -d @register.json http://192.214.4.1:8001/api/auth/login > login_response.txt
  ```

### Screenshot Hasil

- Pengujian apache benchmark dari Sein (Client) ke Fern (Worker Laravel)
  ![image]()
- Hasil Akhir
  ![image]()
- Response
  ![image]()

## Soal 17
> Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. GET /me

### Solusi

- Pada endpoint Get /me diperlukan Request Header berupa Bearer Token, oleh karena itu sebelum mengakses perlu didapatkan tokennya terlebih dahulu
  ```python
  curl -X POST -H "Content-Type: application/json" -d @register.json http://192.214.4.1:8001/api/auth/login > login_response.txt
  ```
- Setelah mendapatkan token, set token sebagai global
  ```python
  token=$(cat login_response.txt | jq -r '.token')
  ```
- Jalankan Testing
  ```python
  ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.214.4.1:8001/api/me
  ```
- Untuk mendapatkan response, lakukan curl seperti berikut
  ```python
  curl -X GET -H "Authorization: Bearer $token" http://192.214.4.1:8001/api/me > get_response.txt
  ```

### Screenshot Hasil

- Pengujian apache benchmark dari Sein (Client) ke Fern (Worker Laravel)
  ![image]()
- Response
  ![image]()

## Soal 18
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern

### Solusi

- Untuk mengimplementasikan Load Balancer pada Laravel Worker, buat konfigurasi baru untuk worker laravel pada LB (Eisen)
  ```python
  	echo 'upstream backend {
  	    server 192.214.4.1:8001; # IP Fern
  	    server 192.214.4.2:8002; # IP Flamme
  	    server 192.214.4.3:8003; # IP Frieren
  	}

  	server {
  	    listen 80;
  	    server_name riegel.canyon.e16.com;

  	    location / {
  	        proxy_pass http://backend;
  	    }
  	    error_log /var/log/nginx/lb_laravel_error.log;
  	    access_log /var/log/nginx/lb_laravel_access.log;
  	}
  	' > /etc/nginx/sites-available/default

  	ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
  	service nginx restart
  ```
- Setelah itu restart nginx dan jalankan test ulang dari client dengan
  ```python
  ab -n 100 -c 10 -H "Authorization: Bearer $token" http://riegel.canyon.e16.com/api/me
  ```

### Screenshot Hasil

- Pengujian apache benchmark dari Sein (Client) pada 3 Worker Laravel
  ![image]()
  ![image]()
- Log Access
  ![image]()

## Soal 19
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan pm.max_children, pm.start_servers, pm.min_spare_servers, pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire

### Solusi

- Percobaan 1
  ```python
  echo '[laravel]
  user = laravel
  group = laravel
  listen = /var/run/php8.1-fpm-dressrosa-site.sock
  listen.owner = www-data
  listen.group = www-data
  php_admin_value[disable_functions] = exec,passthru,shell_exec,system
  php_admin_flag[allow_url_fopen] = off

  ; Choose how the process manager will control the number of child processes.

  pm = dynamic
  pm.max_children = 50
  pm.start_servers = 10
  pm.min_spare_servers = 2
  pm.max_spare_servers = 15
  pm.process_idle_timeout = 10s

  ;contoh diatas konfigurasi untuk mengatur jumalh proses PHP-FPM yang berjalan
  ' > /etc/php/8.0/fpm/pool.d/www.conf

  service php8.0-fpm restart
  service nginx restart
  ```
- Percobaan 2
  ```python
  echo '[laravel]
  user = laravel
  group = laravel
  listen = /var/run/php8.1-fpm-dressrosa-site.sock
  listen.owner = www-data
  listen.group = www-data
  php_admin_value[disable_functions] = exec,passthru,shell_exec,system
  php_admin_flag[allow_url_fopen] = off

  ; Choose how the process manager will control the number of child processes.

  pm = dynamic
  pm.max_children = 75
  pm.start_servers = 15
  pm.min_spare_servers = 5
  pm.max_spare_servers = 25
  pm.process_idle_timeout = 10s

  ;contoh diatas konfigurasi untuk mengatur jumalh proses PHP-FPM yang berjalan
  ' > /etc/php/8.0/fpm/pool.d/www.conf

  service php8.0-fpm restart
  service nginx restart
  ```
- Percobaan 3
  ```python
  echo '[laravel]
  user = laravel
  group = laravel
  listen = /var/run/php8.1-fpm-dressrosa-site.sock
  listen.owner = www-data
  listen.group = www-data
  php_admin_value[disable_functions] = exec,passthru,shell_exec,system
  php_admin_flag[allow_url_fopen] = off

  ; Choose how the process manager will control the number of child processes.

  pm = dynamic
  pm.max_children = 100
  pm.start_servers = 20
  pm.min_spare_servers = 10
  pm.max_spare_servers = 30
  pm.process_idle_timeout = 10s

  ;contoh diatas konfigurasi untuk mengatur jumalh proses PHP-FPM yang berjalan
  ' > /etc/php/8.0/fpm/pool.d/www.conf

  service php8.0-fpm restart
  service nginx restart
  ```

### Screenshot Hasil

- Percobaan 1 menggunakan endpoint Get /me sebanyak 100 request dengan 10 request/second.
  ![image]()
  ![image]()
- Percobaan 2 menggunakan endpoint Get /me sebanyak 100 request dengan 10 request/second.
  ![image]()
  ![image]()
- Percobaan 3 menggunakan endpoint Get /me sebanyak 100 request dengan 10 request/second.
  ![image]()
  ![image]()

## Soal 20
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

### Solusi

- Edit konfigurasi pada LB Eisen untuk menambahkan `least_conn` dalam upstream
  ```python
  echo 'upstream backend {
  			least_conn;
  	    server 192.214.4.1:8001; # IP Fern
  	    server 192.214.4.2:8002; # IP Flamme
  	    server 192.214.4.3:8003; # IP Frieren
  	}

  	server {
  	    listen 80;
  	    server_name riegel.canyon.e16.com;

  	    location / {
  	        proxy_pass http://backend;
  	    }
  	    error_log /var/log/nginx/lb_laravel_error.log;
  	    access_log /var/log/nginx/lb_laravel_access.log;
  	}
  	' > /etc/nginx/sites-available/default

  	ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
  	service nginx restart
  ```

### Screenshot Hasil

- Pengujian menggunakan endpoint Get /me sebanyak 100 request dengan 10 request/second.
  ![image]()
  ![image]()

[<< Daftar Isi](#daftar-isi)

## Grimoire
