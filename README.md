# Jarkom Modul 2
Anggota Kelompok :
- Oktavia Anggraeni P. (5027211001)
- Brigita Naraduhita P.P. (5027211055)


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

## <a name="2"></a> Soal 2
**Deskripsi:** Membuat website utama pada node arjuna dengan akses ke arjuna.IT15.com 

**Pandudewanata**
- Masuk ke node `pandudewanata` dengan menggunakan ` telnet 192.168.0.3 5002`
- Ketikkan ` iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.71.0.0/16`
- Maka akan didapatkan `nameserver 192.168.122.1` dalam `/etc/resolv.conf`
- Untuk memastikan telah terhubung dengan internet maka lakukan `ping google.com -c 5`
  
**Yudhistira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- Setting nameserver dengan `nano /etc/resolv.conf`
 ```bash
 nameserver 192.168.122.1
 ```
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

### <a name="3"></a> Soal 3
**Deskripsi:** Membuat website utama dengan akses ke abimanyu.IT15.com

**Yudhistira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- - Setting nameserver dengan `nano /etc/resolv.conf`
 ```bash
 nameserver 192.168.122.1
 ```
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

### <a name="6"></a> Soal 6
**Deskripsi:** Membuat DNS Slave di Werkudara sebagai cadangan jika server DNS utama di Yudhistira mengalami kegagalan

**Werkudara**
- Masuk ke node `Werkudara` dengan menggunakan `telnet 192.168.0.3 5015`
- - Setting nameserver dengan `nano /etc/resolv.conf`
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
