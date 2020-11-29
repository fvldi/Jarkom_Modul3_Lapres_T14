# Laporan Resmi Praktikum Komunikasi Data dan Jaringan Komputer Modul 3  Kelompok T14

## Anggota Kelompok:

1. Hisyam Zulkarnain F – 05311840000019
2. Muhamad Rifaldi - 05311840000022

**Soal 1**

Melakukan setting UML sesuai dengan topologi berikut
![image](https://user-images.githubusercontent.com/61223768/100406298-13c87a80-3098-11eb-9226-6731d444f218.png)

* buat `topologi.sh` sebagai berikut

<img width="646" alt="topologi" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/1a.PNG">

* Setting router SURABAYA pada `/etc/sysctl.conf` lalu
* un-comment `net.ipv4.ip_forward=1` 
* Aktifkan dengan command `sysctl -p`
* Buat `iptables.sh` pada uml surabaya dengan konfigurasi

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.168.0.0/16

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.168.1.0/16
```
* Jalankan iptables.sh
* setting source list pada `nano /etc/apt/sources.list` dengan konfigurasi `deb http://boyo.its.ac.id/debian stretch main contrib non-free`
* Setting interface di tiap uml dengan `nano /etc/network/interfaces` seperti berikut

* uml SURABAYA

```
**uml surabaya

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.82
netmask 255.255.255.252
gateway 10.151.76.81

auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 10.151.77.161
netmask 255.255.255.248

**uml malang

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.162
netmask 255.255.255.248
gateway 10.151.77.161

**uml mojokerto

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.163
netmask 255.255.255.248
gateway 10.151.77.161

**uml tuban

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.164
netmask 255.255.255.248
gateway 10.151.77.161

**uml gresik

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp 

**uml sidoarjo

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

**uml madiun

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

**uml banyuwangi

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

* lakukan `service networking restart` pada masing masing uml


**soal 2**

melakukan setting router surabaya sebagai DHCP relay
* Install `isc-dhcp-relay pada` uml SURABAYA dengan command `apt-get install isc-dhcp-relay`
* lakukan setting pada file `/etc/default/isc-dhcp-relay` seperti berikut

<img width="380" alt="relay" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/2.PNG">

* lakukan `service isc-dhcp-relay restart` untuk menjalankan dhcp-relay

**soal 3**

Setting client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan
192.168.0.110 sampai 192.168.0.200.
* Install `isc-dhcp-server` pada uml TUBAN dengan command `apt-get install isc-dhcp-server`
* lakukan setting file `/etc/default/isc-dhcp-server` seperti berikut

<img width="377" alt="server" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3a.PNG">

* lakukan `service isc-dhcp-server restart` untuk menjalankan dhcp-server
* Edit file `/etc/dhcp/dhcpd.conf` dengan konfigurasi sebagai berikut

<img width="389" alt="server2" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3b4a5a6a.PNG">

* lakukan test pada client subnet 1 yaitu Gresik dan Sidoarjo

<img width="381" alt="no 3 test gresik" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3c.PNG">
<img width="379" alt="no 3 test sidoarjo" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3d.PNG">

**soal 4**

Setting lient pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
* pada file /etc/dhcp/dhcpd.conf pastikan konfigurasi sebagai berikut

<img width="383" alt="server3" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3b4a5a6a.PNG">

* Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server
* lakukan test pada client di subnet 3 yaitu Madiun dan Banyuwangi

<img width="386" alt="no 4 test banyuwangi" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/4b.PNG">
<img width="383" alt="no 4 test madiun" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/4c.PNG">

**soal 5**

Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
* pada file /etc/dhcp/dhcpd.conf pastikan konfigurasi sebagai berikut

<img width="382" alt="server4" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3b4a5a6a.PNG">

* Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server

**soal 6**

Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client
pada subnet 3 mendapatkan peminjaman IP selama 10 menit.
* pada file /etc/dhcp/dhcpd.conf pastikan konfigurasi sebagai berikut

<img width="381" alt="server5" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/3b4a5a6a.PNG">

* Jalankan `service isc-dhcp-server restart` untuk menjalankan dhcp-server

**soal 7**

Setting user autentikasi
* lakukan instalasi squid
* lakukan setting pada `htpasswd -c /etc/squid/passwd userta_t14` dengan password: inipassw0rdta_t14 seperti berikut

<img width="378" alt="no 7" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/7.png">

**soal 8**

Setting agar Proxy server hanya dapat diakses pada hari Selasa-Rabu pukul 13.00-18.00.
* Lakukan setting pada `nano /etc/squid/acl.conf` dengan konfigurasi sebagai berikut

`acl WAKTU time TW 13:00-18:00`

<img width="388" alt="no 8" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/8.png">


**soal 9**

Setting agar Proxy server hanya dapat diakses pada hari Selasa-Kamis pukul 21.00-09.00 keesokan harinya.
* tambahkan code berikut pada `nano /etc/squid/acl.conf` 
```
acl WAKTU time TWH 21:00-23:59
acl WAKTU time WHF 00:00-09:00
```

<img width="384" alt="no 9" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/8.png">

* lalu pada `/etc/squid/acl.conf` dapat ditambahkan seperti berikut

<img width="381" alt="no 9 kedua" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/9.png">

**soal 10**

Setting agar ketika mengakses google.com, maka akan melakukan redirect menuju monta.if.its.ac.id.
* Lakukan setting pada ` /etc/squid/squid.conf`

* berikut hasilnya

<img width="960" alt="no 10 test" src="https://raw.githubusercontent.com/fvldi/Jarkom_Modul3_Lapres_T14/main/img/10.png">

