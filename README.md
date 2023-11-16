# Jarkom-Modul-3-A14-2023
## 1 Lakukan Konfigurasi sesuai dengan topologi yang diberikan
Pertama, buat topologi tersebut kemudian set konfigurasi setiap nodenya sebagai berikut  
<b>Aura</b>
```
auto eth0
iface eth0 inet dhcp
Ip iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.6.0.0/16

auto eth1
iface eth1 inet static
	address 10.6.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.6.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.6.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.6.4.0
	netmask 255.255.255.0
```

<b>Himmel</b>
```
auto eth0
iface eth0 inet static
	address 10.6.1.1
	netmask 255.255.255.0
	gateway 10.6.1.0
```

<b>Heiter</b>
```
auto eth0
iface eth0 inet static
	address 10.6.1.2
	netmask 255.255.255.0
	gateway 10.6.1.0
```

<b>Denken</b>
```
auto eth0
iface eth0 inet static
	address 10.6.2.2
	netmask 255.255.255.0
	gateway 10.6.2.0
```

<b>Eisen</b>
```
auto eth0
iface eth0 inet static
	address 10.6.2.1
	netmask 255.255.255.0
	gateway 10.6.2.0
```

<b>Frieren</b>
```
auto eth0
iface eth0 inet static
	address 10.6.4.3
	netmask 255.255.255.0
	gateway 10.6.4.0
```

<b>Flamme</b>
```
auto eth0
iface eth0 inet static
	address 10.6.4.2
	netmask 255.255.255.0
	gateway 10.6.4.0
```

<b>Fern</b>
```
auto eth0
iface eth0 inet static
	address 10.6.4.1
	netmask 255.255.255.0
	gateway 10.6.4.0
```

<b>Lawine</b>
```
auto eth0
iface eth0 inet static
	address 10.6.3.3
	netmask 255.255.255.0
	gateway 10.6.3.0
```

<b>Linie</b>
```
auto eth0
iface eth0 inet static
	address 10.6.3.2
	netmask 255.255.255.0
	gateway 10.6.3.0
```

<b>Lugner</b>
```
auto eth0
iface eth0 inet static
	address 10.6.3.1
	netmask 255.255.255.0
	gateway 10.6.3.0
```

<b>Revolte</b>
```
auto eth0
iface eth0 inet dhcp
```

<b>Richter</b>
```
auto eth0
iface eth0 inet dhcp
```

<b>Sein</b>
```
auto eth0
iface eth0 inet dhcp
```

<b>Stark</b>
```
auto eth0
iface eth0 inet dhcp
```

<hr></hr>  

## Semua Client harus menggunakan konfigurasi dari DHCP Server
<b>Aura</b>
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.6.0.0/16
```
Jalankan kedua perintah ini untuk membuat semua node lainnya dapat mengakses NAT

```
apt-get update
apt-get install isc-dhcp-relay -y
```
Install relay untuk dhcp

```
echo 'SERVERS="10.6.1.1"
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=""
' > /etc/default/isc-dhcp-relay
```
Set nilai SERVERS menjadi IP dari DHCP server, kemudian INTERFACES sebagai sambungan koneksi ethernet yang dimiliki oleh server DHCP relay

```
echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf
```
Buat agar request DHCP dapat diforward dari client menuju DHCP server, dan sebaliknya

```
service isc-dhcp-relay start
```
Mulai service DHCP relay

<b>Himmel</b>
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
```
Digunakan untuk membuat Himmel bisa terkoneksi dengan internet

```
apt-get update
apt-get install isc-dhcp-server -y
```
Install server DHCP

```
echo 'DHCPDv4_PID=/var/run/dhcpd.pid
INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo 'option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

ddns-update-style none;

authoritative;

subnet 10.6.1.0 netmask 255.255.255.0{
}
subnet 10.6.2.0 netmask 255.255.255.0{
}
subnet 10.6.3.0 netmask 255.255.255.0 {
}
subnet 10.6.4.0 netmask 255.255.255.0 {
}
' > /etc/dhcp/dhcpd.conf
```
Set up server DHCP untuk menangani DHCP request

<hr></hr>  

## 2 Client yang melalui Switch3 mendapatkan range IP dari 10.6.3.16 - 10.6.3.32 dan 10.6.3.64 - 10.6.3.80
<b>Himmel</b>
```
subnet 10.6.3.0 netmask 255.255.255.0 {
    range 10.6.3.16 10.6.3.32;
    range 10.6.3.64 10.6.3.80;
    option routers 10.6.3.0;
    option broadcast-address 10.6.3.255;
}
```
Ubah bagian ```subnet 10.6.3.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> untuk mengatur cara DHCP server menangani request DHCP dari subnet 10.6.3.0 Disini mereka akan diberikan ip 10.6.3.16 - 10.6.3.32, atau 10.6.3.64 - 10.6.3.80. Router dari subnet tersebut adalah 10.6.3.0

<hr></hr>  

## 3 Client yang melalui Switch4 mendapatkan range IP dari 10.6.4.12 - 10.6.4.20 dan 10.6.4.160 - 10.6.4.168
<b>Himmel</b>
```
subnet 10.6.4.0 netmask 255.255.255.0 {
    range 10.6.4.12 10.6.4.20;
    range 10.6.4.160 10.6.4.168;
    option routers 10.6.4.0;
    option broadcast-address 10.6.4.255;
}
```
Ubah bagian ```subnet 10.6.4.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> untuk mengatur cara DHCP server menangani request DHCP dari subnet 10.6.4.0 Disini mereka akan diberikan ip 10.6.4.12 - 10.6.4.20, atau 10.6.4.160 - 10.6.4.168. Router dari subnet tersebut adalah 10.6.4.0

<hr></hr>  

## 4 Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
<b>Himmel</b>
```
    option domain-name-servers 10.6.1.2;
```
Tambahkan baris tersebut kedalam fungsi ```subnet 10.6.3.0 netmask 255.255.255.0 {}``` dan ```subnet 10.6.4.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> sehingga mereka akan menggunakan DNS dari server dengan IP 10.6.1.2 (Heiter)

<hr></hr>  
## 5 Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
<b>Himmel</b>
```
    default-lease-time 180;
    max-lease-time 5760;
```
Tambahkan baris tersebut kedalam fungsi ```subnet 10.6.3.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> sehingga mereka akan memiliki waktu peminjaman secara default selama 180 detik (3 menit) dan waktu peminjaman maksimal 5760 detik (96 menit)

```
    default-lease-time 720;
    max-lease-time 5760;
```
Tambahkan baris tersebut kedalam fungsi ```subnet 10.6.4.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> sehingga mereka akan memiliki waktu peminjaman secara default selama 720 detik (12 menit) dan waktu peminjaman maksimal 5760 detik (96 menit)


<hr></hr>  

## 6 Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3

<hr></hr>  

## 7 Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
## Lawine, 4GB, 2vCPU, dan 80 GB SSD.
## Linie, 2GB, 2vCPU, dan 50 GB SSD.
## Lugner 1GB, 1vCPU, dan 25 GB SSD.
## Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second

<hr></hr>  

## 8 Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
## a) Nama Algoritma Load Balancer
## b) Report hasil testing pada Apache Benchmark
## c) Grafik request per second untuk masing masing algoritma. 
## d) Analisis

<hr></hr> 

## 9 Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire

<hr></hr>  

## 10 Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

<hr></hr>  

## 11 Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

<hr></hr>

## 12 Selanjutnya LB ini hanya boleh diakses oleh client dengan IP 10.6.3.69, 10.6.3.70, 10.6.4.167, dan 10.6.4.168.

<hr></hr>  

## 13 Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern

<hr></hr>  

## 14 Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer 

<hr></hr>  

## Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire
## 15 POST /auth/register
## 16 POST /auth/login
## 17 GET /me

<hr></hr>  

## 18 Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern

<hr></hr>  

## 19 Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
## - pm.max_children
## - pm.start_servers
## - pm.min_spare_servers
## - pm.max_spare_servers
## sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire

<hr></hr>  

## 20 Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.
