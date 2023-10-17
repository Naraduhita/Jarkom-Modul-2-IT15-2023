# Jarkom Modul 2

|    Anggota Kelompok     |     NRP     |
| ---                     | ---         |
| Oktavia Anggraeni P.    | 5027211001  |
| Brigita Naraduhita P.P. | 5027211055  |


### Topologi 5
- [Soal 1](#1)
- [Soal 2](#2)
- [Soal 3](#3)
- [Soal 4](#4)
- [Soal 5](#5)
- [Soal 6](#6)
- [Soal 7](#7)
- [Soal 8](#8)
- [Soal 9](#9)
- [Soal 10](#10)
- [Soal 11](#11)
- [Soal 12](#12)
- [Soal 13](#13)
- [Soal 14](#14)
- [Soal 15](#15)
- [Soal 16](#16)
- [Soal 17](#17)
- [Soal 18](#18)
- [Soal 19](#19)
- [Soal 20](#20)

## <a name="1"></a> Soal 1
**Deskripsi:** Membuat topologi sesuai dengan pembagian. Memasukkan IP prefix sesuai pembagian.
- Yudhistira : DNS master
- Werkudara: DNS Slave
- Arjuna: Load balancer
- Prabukusuma, Abimanyu, Wisanggeni: web server

**Configure**

Nakula Client:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.1.2
	netmask 255.255.255.0
	gateway 10.71.1.1
```
Sadewa Client:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.1.3
	netmask 255.255.255.0
	gateway 10.71.1.1
```
Pandudewanata :
```bash
# Static config for eth0
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.71.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.71.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.71.3.1
	netmask 255.255.255.0
```
Yudhistira DNS Master:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.2.2
	netmask 255.255.255.0
	gateway 10.71.2.1
```
Werkudara DNS Slave:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.2.3
	netmask 255.255.255.0
	gateway 10.71.2.1
```
Prabukusuma WebServer:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
        address 10.71.3.2
	netmask 255.255.255.0
	gateway 10.71.3.1
```
Abimanyu WebServer:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.3.3
	netmask 255.255.255.0
	gateway 10.71.3.1
```
Wisanggeni WebServer:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.3.4
	netmask 255.255.255.0
	gateway 10.71.3.1
```
Arjuna LoadBalancer:
```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 10.71.3.5
	netmask 255.255.255.0
	gateway 10.71.3.1
```
#### Pengerjaan
Berikut topologi yang telah dibuat:
<img src="https://i.ibb.co/YLFnfVg/topology.jpg">
<br>

## <a name="2"></a> Soal 2
**Deskripsi:** Membuat website utama pada node arjuna dengan akses ke arjuna.IT15.com 

**Pandudewanata**
- Masuk ke node `pandudewanata` dengan menggunakan ` telnet 192.168.0.3 5002`
- Ketikkan ` iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.71.0.0/16`
- Maka akan didapatkan `nameserver 192.168.122.1` dalam `/etc/resolv.conf`
- Untuk memastikan telah terhubung dengan internet maka lakukan `ping google.com -c 5`
<img src="https://i.ibb.co/L6RG850/No2-Pandudewanata.png">
  
**Yudhistira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- Setting nameserver dengan `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
- Install aplikasi bind9 dengan perintah:
```bash
apt-get update
apt-get install bind9 -y
```
- Membuka file di named.conf.local `nano /etc/bind/named.conf.local`
- Isikan configurasi domain dengan syntax: 
```bash 
zone "arjuna.IT15.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.IT15.com";
};
```
- Restart bind9 dengan perintah `service bind9 restart`
- Buka file `arjuna.IT15.com` pada `/etc/bind/jarkom/arjuna.IT15.com`
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.IT15.com. root.arjuna.IT15.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL;
@       IN      NS      arjuna.IT15.com.
@       IN      A       10.71.3.5   ; IP Arjuna
@       IN      AAAA    ::1' 
```
- Restart bind9 dengan perintah `service bind9 restart`
  
**Nakula**
-  Masuk ke node `nakula` sebagai `client` dengan menggunakan `telnet 192.168.0.3 5009`
-  Masukkan IP Yudhistira 
```bash
 # nameserver 192.168.122.1
nameserver 10.71.2.2
```
- Untuk mencoba koneksi DNS, lakukan ping domain arjuna.IT15.com dengan:
```bash
ping arjuna.IT15.com -c 5
```
<img src="https://i.ibb.co/RpJT49W/No2-Arjuna.png">
<br>

### <a name="3"></a> Soal 3
**Deskripsi:** Membuat website utama dengan akses ke abimanyu.IT15.com

**Yudhistira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- Setting nameserver dengan `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
- Install aplikasi bind9 dengan perintah:
```bash
apt-get update
apt-get install bind9 -y
```
- Membuka file di named.conf.local `nano /etc/bind/named.conf.local`
- Isikan configurasi domain dengan syntax: 
```bash 
zone "abimanyu.IT15.com" {
        type master;
        file "/etc/bind/jarkom/abimanyu.IT15.com";
};
```
- Restart bind9 dengan perintah `service bind9 restart`
- Buka file `abimanyu.IT15.com` pada `/etc/bind/jarkom/abimanyu.IT15.com`
- Masukkan syntax berikut:
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT15.com. root.abimanyu.IT15.com (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL;
@       IN      NS      abimanyu.IT15.com.
@       IN      A       10.71.3.3   ; IP Abimanyu
@       IN      AAAA    ::1
```
- Restart bind9 dengan perintah `service bind9 restart`
  
**Nakula**
-  Masuk ke node `nakula` sebagai `client` dengan menggunakan `telnet 192.168.0.3 5009`
-  Masukkan IP Yudhistira 
```bash
# nameserver 192.168.122.1
nameserver 10.71.2.2
```
- Untuk mencoba koneksi DNS, lakukan ping domain abimanyu.IT15.com dengan:
```bash
ping abimanyu.IT15.com -c 5
```
<img src="https://i.ibb.co/QX4fqFT/No3-Abimanyu.png">
<br>

### <a name="4"></a> Soal 4
**Deskripsi:**  Buat subdomain parikesit.abimanyu.IT15.com,  DNS di Yudhistira dan mengarah ke Abimanyu.

**Yudhistira**
- Buka file `abimanyu.IT15.com` pada `/etc/bind/jarkom/abimanyu.IT15.com`
- Tambahkan konfigurasi seperti berikut:
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT15.com. root.abimanyu.IT15.com (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL;
@         IN      NS      abimanyu.IT15.com.
@         IN      A       10.71.3.3   ; IP Abimanyu
www       IN      CNAME   abimanyu.IT15.com.
parikesit IN      A       10.71.3.3   ; IP Yudhistira
@       IN      AAAA    ::1
```
- Restart bind9 dengan perintah `service bind9 restart`
  
**Nakula**
- Untuk mencoba koneksi subdomain, lakukan ping dengan: 
```bash
ping parikesit.abimanyu.IT15.com -c 5
```
<img src="https://i.ibb.co/g3hJY0G/No4-Parikesit.png">
<br>

### <a name="5"></a> Soal 5
**Deskripsi:** reverse domain untuk domain utama (abimanyu)

**Yudhistira**
- Membuka file di named.conf.local `nano /etc/bind/named.conf.local`
- Isi dengan syntax: 
```bash 
zone "3.71.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/3.71.10.in-addr.arpa";
};
```
- Buka file `nano /etc/bind/jarkom/3.71.10.in-addr.arpa` 
- Isikan dengan syntax:
```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.IT15.com. root.abimanyu.IT15.com. (                     
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.71.10.in-addr.arpa.   IN      NS      abimanyu.IT15.com.
3                       IN      PTR     abimanyu.IT15.com.
```
- Restart bind9 dengan perintah `service bind9 restart`

**Nakula**
- Install package dnsutils
```bash
apt-get update
apt-get install dnsutils
```
- Kembalikan nameserver agar tersambung dengan abimanyu
```bash
host -t PTR 10.71.3.3
```
<img src="https://i.ibb.co/fDHdsDF/no-5.jpg">
<br>

### <a name="6"></a> Soal 6
**Deskripsi:** Membuat DNS Slave di Werkudara sebagai cadangan jika server DNS utama di Yudhistira mengalami kegagalan

**Werkudara**
- Masuk ke node `Werkudara` dengan menggunakan `telnet 192.168.0.3 5015`
- Setting nameserver dengan `nano /etc/resolv.conf`
 ```bash
 nameserver 192.168.122.1
 ```
- Install aplikasi bind9 dengan perintah:
```bash
apt-get update
apt-get install bind9 -y
```
- Edit file `/etc/bind/named.conf.local` dan isikan syntax berikut:
```bash
zone "arjuna.IT15.com" {
	type slave;
	file "/etc/bind/jarkom/arjuna.IT15.com";
	masters { 10.71.2.2; }; //IP Yudhistira
};
zone "abimanyu.IT15.com" {
	type slave;
	file "/etc/bind/jarkom/abimanyu.IT15.com";
	masters { 10.71.2.2; }; //IP Yudhistira
};
```
- Restart bind9 dengan perintah `service bind9 restart`

**Yudhistira**
- Matikan service bind9 `service bind9 stop`

**Nakula**
- Masukkan IP Werkudara
```bash
nameserver	10.71.2.2 
nameserver	10.71.2.3 
```
- Untuk mencoba koneksi maka lakukan ping:
```bash
ping arjuna.IT15.com -c 5
ping abimanyu.IT15.com -c 5
ping parikesit.abimanyu.IT15.com -c 5
```
<img src="https://i.ibb.co/py4yyjQ/no-6.jpg">
<br>

### <a name="7"></a> Soal 7
**Deskripsi:** Buat subdomain baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

**Yudhistira**
- Buka file `nano /etc/bind/jarkom/baratayuda.abimanyu.IT15.com`
- Tambahkan konfigurasi seperti berikut:
```bash
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.IT15.com. root.baratayuda.abimanyu.IT15.com. (
                    20221006012         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.IT15.com.
@       IN      A1      10.71.3.3 ;IP abimanyu
www     IN      CNAME   baratayuda.abimanyu.IT15.com.
ns1     IN      A       10.71.2.3 ; IP werkudara
baratayuda      IN      NS     ns1
```
- Buka file named.conf.options `nano /etc/bind/named.conf.options`
- Tambahkan `allow-query{any;};` seperti berikut:
```bash
options {
        directory "/var/cache/bind";
        //dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;  
        listen-on-v6 { any; };
};
```
- Edit file `/etc/bind/named.conf.local` dan isikan syntax berikut:
```bash
zone "baratayuda.abimanyu.IT15.com" {
        type master;
        file "/etc/bind/jarkom/baratayuda.abimanyu.IT15.com";
        allow-transfer {10.71.2.3; };
};
```
- Restart bind9 dengan perintah `service bind9 restart`

**Werkudara**
- Buka file named.conf.options `nano /etc/bind/named.conf.options`
- Tambahkan `allow-query{any;};` seperti berikut:
```bash
options {
        directory "/var/cache/bind";
        //dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;  
        listen-on-v6 { any; };
};
```
- Edit file `/etc/bind/named.conf.local` dan isikan syntax berikut:
```bash
zone "baratayuda.abimanyu.IT15.com"{
        type master;
        file "/etc/bind/jarkom/baratayuda.abimanyu.IT15.com"
};
```
- Buat direktori baru bernama jarkom `mkdir /etc/bind/jarkom`
- Buka file `/etc/bind/jarkom/baratayuda.abimanyu.IT15.com`
- Tambahkan konfigurasi seperti berikut:
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.IT15.com. root.baratayuda.abimanyu.IT15.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.IT15.com.
@       IN      A       10.71.3.3  ;abimanyu
www     IN      CNAME       baratayuda.abimanyu.IT15.com.
```
- Restart bind9 dengan perintah `service bind9 restart`

**Nakula**
- Untuk mencoba koneksi maka lakukan ping pada client:
```bash
ping baratayuda.abimanyu.IT15.com -c 5
ping www.baratayuda.abimanyu.IT15.com
```
<img src="https://i.ibb.co/ZS4c16T/No7-Nakula.png">
<br>

### <a name="8"></a> Soal 8

**Deskripsi:**  Buat subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.IT15.com yang mengarah ke Abimanyu.

**Werkudara**
- Buka file named.conf.local `nano /etc/bind/named.conf.local`
- Isikan syntax berikut:
```bash
zone "rjp.baratayuda.abimanyu.IT15.com" {
        type master;
        file "/etc/bind/jarkom/rjp.baratayuda.abimanyu.IT15.com";
};
```
- Buka file `nano /etc/bind/jarkom/rjp.baratayuda.abimanyu.IT15.com` 
- Isikan syntax berikut:
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rjp.baratayuda.abimanyu.IT15.com. root.rjp.baratayuda.abimanyu.IT15.com. (
                     2022100601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rjp.baratayuda.abimanyu.IT15.com.
@       IN      A       10.71.3.3  ;abimanyu
www     IN      A       10.71.3.3
```

**Nakula**
- Untuk mencoba koneksi maka lakukan ping pada client:
```bash
ping rjp.baratayuda.abimanyu.IT15.com -c 5
ping www.rjp.baratayuda.abimanyu.IT15.com -c 5
```
<img src="https://i.ibb.co/jTsfz2M/No8-Abimanyu.png">
<br>

### <a name="9"></a> Soal 9

**Deskripsi:**
Lakukan deployment pada masing-masing worker. Dengan Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker yaitu Prabukusuma, Abimanyu, dan Wisanggeni.

**Prabukusuma**
- Masuk ke node `Prabukusuma` dengan menggunakan `telnet 192.168.0.3 5017`
- Setting nameserver dengan `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
- Lakukan instalasi Nginx 
```bash
apt-get update
apt install nginx php php-fpm -y
```
- Lakukan instalasi Lynx
```bash
apt-get update
apt-get install lynx
```
- Jalankan perintah `service nginx start`
- Membuat direktori `mkdir /var/www/jarkom`
- Buka file `nano /var/www/jarkom/index.php`
- Isi syntax berikut:
```bash
<?php
echo "ini adalah halaman Prabukusuma";
?>
```
- Buat direktori `mkdir /etc/nginx/sites-available`
- Masukkan config ke file `/etc/nginx/sites-available/jarkom` (Port:8001)
```bash
server {
        listen 8001;
        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;
        location / {
 try_files $uri $uri/ /index.php?$query_string; }
 # pass PHP scripts to FastCGI server
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; }

 location ~ /\.ht {
 deny all; }
 error_log /var/log/nginx/jarkom_error.log;
 access_log /var/log/nginx/jarkom_access.log;
 }
```
*Pastikan menggunakan port 8001, menggunakan php7.0 sesuai dengan PHP yang diinstal*

- Jalankan command:
```bash
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom
rm /etc/nginx/sites-enabled/default
service nginx restart
service php7.0-fpm start
service php7.0-fpm status
```
- Lalu jalankan command `lynx http://10.71.3.2:8001` *(IP Prabukusuma dan Port 8001)*
<img src="https://i.ibb.co/B6dzV3f/No9-Prabukusuma.png">

**Abimanyu**
- Masuk ke node `Abimanyu` dengan menggunakan `telnet 192.168.0.3 5019`
- Setting nameserver dengan `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
- Lakukan instalasi Nginx 
```bash
apt-get update
apt install nginx php php-fpm -y
```
- Lakukan instalasi Lynx
```bash
apt-get update
apt-get install lynx
```
- Jalankan perintah `service nginx start`
- Membuat direktori `mkdir /var/www/jarkom`
- Buka file `nano /var/www/jarkom/index.php`
- Isi syntax berikut:
```bash
<?php
echo "ini adalah halaman Abimanyu";
?>
```
- Buat direktori `mkdir /etc/nginx/sites-available`
- Masukkan config ke file `/etc/nginx/sites-available/jarkom` (Port:8002)
```bash
server {
        listen 8002;
        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;
        location / {
 try_files $uri $uri/ /index.php?$query_string; }
 # pass PHP scripts to FastCGI server
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; }

 location ~ /\.ht {
 deny all; }
 error_log /var/log/nginx/jarkom_error.log;
 access_log /var/log/nginx/jarkom_access.log;
 }
```
*Pastikan menggunakan port 8002, menggunakan php7.0 sesuai dengan PHP yang diinstal*

- Jalankan command:
```bash
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom
rm /etc/nginx/sites-enabled/default
service nginx restart
service php7.0-fpm start
service php7.0-fpm status
```
- Lalu jalankan command `lynx  http://10.71.3.3:8002` *(IP Abimanyu dan Port 8002)*
<img src="https://i.ibb.co/Wy7ZWQ6/No9-Abimanyu.png">

**Wisanggeni**
- Masuk ke node `Wisanggeni` dengan menggunakan `telnet 192.168.0.3 5021`
- Setting nameserver dengan `echo "nameserver 192.168.122.1" > /etc/resolv.conf`
- Lakukan instalasi Nginx 
```bash
apt-get update
apt install nginx php php-fpm -y
```
- Lakukan instalasi Lynx
```bash
apt-get update
apt-get install lynx
```
- Jalankan perintah `service nginx start`
- Membuat direktori `mkdir /var/www/jarkom`
- Buka file `nano /var/www/jarkom/index.php`
- Isi syntax berikut:
```bash
<?php
echo "ini adalah halaman Wisanggeni";
?>
```
- Buat direktori `mkdir /etc/nginx/sites-available`
- Masukkan config ke file `/etc/nginx/sites-available/jarkom` (Port:8003)
```bash
server {
        listen 8003;
        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;
        location / {
 try_files $uri $uri/ /index.php?$query_string; }
 # pass PHP scripts to FastCGI server
 location ~ \.php$ {
 include snippets/fastcgi-php.conf;
 fastcgi_pass unix:/var/run/php/php7.0-fpm.sock; }

 location ~ /\.ht {
 deny all; }
 error_log /var/log/nginx/jarkom_error.log;
 access_log /var/log/nginx/jarkom_access.log;
 }
```
*Pastikan menggunakan port 8003, menggunakan php7.0 sesuai dengan PHP yang diinstal*

- Jalankan command:
```bash
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom
rm /etc/nginx/sites-enabled/default
service nginx restart
service php7.0-fpm start
service php7.0-fpm status
```
- Lalu jalankan command `lynx http://10.71.3.4:8003 ` *(IP Wisanggeni dan Port 8003)*
<img src="https://i.ibb.co/z5xDt1n/No9-wisanggeni.png">
<br>

### <a name="10"></a> Soal 10

**Deskripsi:** 
Gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003.
- Prabukusuma:8001
- Abimanyu:8002
- Wisanggeni:8003

**Arjuna**
- Masuk ke node `Arjuna` dengan menggunakan `telnet 192.168.0.3 5023`
- Lakukan instalasi Nginx 
```bash
apt-get update
apt install nginx php php-fpm -y
```
- Lakukan instalasi Lynx
```bash
apt-get update
apt-get install lynx
```
- Jalankan perintah `service nginx start`
- Masukkan config ke Arjuna di `/etc/nginx/sites-available/default`
```bash
http {
    upstream nodes_lb {
    		server 10.71.3.2:8001;
server 10.71.3.3:8002;
server 10.71.3.4:8003;  }
}
server {
    listen 80;
    listen [::]:80;

    server_name arjuna.IT15.com;

    location / {
        proxy_pass http://nodes_lb;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
- Masukkan config ke Arjuna di ` /etc/nginx/nginx.conf`
```bash
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 768;
    # multi_accept on;
}

http {
    upstream nodes_lb {
      	server 10.71.3.2:8001;
server 10.71.3.3:8002;
server 10.71.3.4:8003;

    }

    server {
        listen 80;
        listen [::]:80;

        server_name arjuna.IT15.com;

        location / {
            proxy_pass http://nodes_lb;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```
Run command :
```bash
ln -s /etc/nginx/sites-available/arjuna.IT15.com /etc/nginx/sites-enabled/arjuna.IT15.com
rm /etc/nginx/sites-enabled/default
service nginx restart
```
**Nakula**
- Lalu jalankan command `lynx http://www.arjuna.IT15.com`
<img src="https://i.ibb.co/F8bxGZF/No10.png">
<br>

### <a name="11"></a> Soal 11
Lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy.
**Abimanyu**
- Lakukan instalasi Apache2
```bash
apt-get update
apt install apache2
```
- Lakukan instalasi wget dan unzip (jika belum)
```bash
apt install wget
apt install unzip
```
- Masuk ke direktori untuk DocumentRoot `cd /var/www`
- Download file resource untuk abimanyu.yyy.com, unzip, dan mengatur direktori file
```bash
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' -O abimanyu
unzip abimanyu -d abimanyu.IT15
mv abimanyu.IT15/abimanyu.yyy.com/* abimanyu.IT15
rm -r abimanyu.IT15/abimanyu.yyy.com
```
- Masukkan config ke Abimanyu di direktori `/etc/apache2/sites-available/abimanyu.IT15.conf`
```bash
<VirtualHost *:80>
    ServerName abimanyu.IT15.com
    ServerAlias www.abimanyu.IT15.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.IT15

    RewriteEngine On

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/acces.log combined
</VirtualHost>
```
- Jalankan command untuk mengaktifkan apache2
```bash
a2ensite abimanyu.IT15.conf
service apache2 reload
service apache2 restart
```
**Nakula**
- Untuk mencoba koneksi, lakukan ping website dan jalankan command lynx
```bash
ping http://www.abimanyu.IT15.com -c 2
lynx http://www.abimanyu.IT15.com
```
<img src ="https://i.ibb.co/BV92sF2/akulahabimanyu-11.png">
<br>

### <a name="12"></a> Soal 12
**Abimanyu**
- Masuk kembali ke direktori conf untuk menambahkan 'alias home' `nano /etc/apache2/sites-available/abimanyu.IT15.conf `
```bash
<VirtualHost *:80>
    ServerName abimanyu.IT15.com
    ServerAlias www.abimanyu.IT15.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.IT15

    <Directory /var/www/abimanyu.IT15>
      Options +Indexes
    </Directory>

    Alias /home /var/www/abimanyu.IT15/index.php/home

    RewriteEngine On

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/acces.log combined
</VirtualHost>
```

- Jalankan command berikut ini untuk memproses file pada .conf dan untuk mengaktifkan apache2
```bash
cd /etc/apache2/sites-available/
a2enmod rewrite
a2ensite abimanyu.IT15.conf
service apache2 reload
service apache2 start
service apache2 status
```
**Nakula**
- Jalankan command `lynx http://www.abimanyu.IT15.com/home`
<img source="https://i.ibb.co/2tfsFgW/akulahabimanyu-12.png">
<br>

