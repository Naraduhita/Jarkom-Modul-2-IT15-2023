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
  
