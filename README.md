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

### <a name="1"></a> Soal 1
**Deskripsi:** Membuat topologi sesuai dengan pembagian. Memasukkan IP prefix sesuai pembagian.
- Yudhistira : DNS master
- Werkudara: DNS Slave
- Arjuna: Load balancer
- Prabukusuma, Abimanyu, Wisanggeni: web server

#### Pengerjaan
Berikut topologi yang telah dibuat:

### <a name="2"></a> Soal 2
**Deskripsi:** Membuat website utama pada node arjuna dengan akses ke arjuna.IT15.com 

**Pandudewanata**
- Masuk ke node `pandudewanata` dengan menggunakan ` telnet 192.168.0.3 5002`
- Ketikkan ` iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.71.0.0/16`
- Maka akan didapatkan `nameserver 192.168.122.1` dalam ` /etc/resolv.conf`
- Untuk memastikan telah terhubung dengan internet maka lakukan `ping google.com -c 5`
**Yudhitira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- Masukkan `nameserver 192.168.122.1` ke dalam `/etc/resolv.conf`
- Install aplikasi bind9 dengan perintah:
```bash
apt-get update
apt-get install bind9 -y
```
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

**Yudhitira**
- Masuk ke node `Yudhistira` dengan menggunakan `telnet 192.168.0.3 5013`
- Masukkan `nameserver 192.168.122.1` ke dalam `/etc/resolv.conf`
- Install aplikasi bind9 dengan perintah:
```bash
apt-get update
apt-get install bind9 -y
```
- Isikan configurasi domain dengan syntax: 
```bash 
zone "abimanyu.IT15.com" {
        type master;
        file "/etc/bind/jarkom/abimanyu.IT15.com";
};
```
- Restart bind9 dengan perintah `service bind9 restart`
- Buka file `abimanyu.IT15.com` pada `/etc/bind/jarkom/abimanyu.IT15.com`
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
- Untuk mencoba koneksi DNS, lakukan ping domain arjuna.IT15.com dengan:
```bash
ping abimanyu.IT15.com -c 5
```
