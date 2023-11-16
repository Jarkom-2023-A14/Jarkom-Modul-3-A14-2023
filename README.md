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
## 2 Client yang melalui Switch3 mendapatkan range IP dari 10.6.3.16 - 10.6.3.32 dan 10.6.3.64 - 10.6.3.80
<b>Himmel</b>
```
subnet 10.6.3.0 netmask 255.255.255.0 {
    range 10.6.3.16 10.6.3.32;
    range 10.6.3.64 10.6.3.80;
    option routers 10.6.3.0;
    option broadcast-address 10.6.3.255;
    option domain-name-servers 10.6.1.2;
}
```
Ubah bagian ```subnet 10.6.3.0 netmask 255.255.255.0 {}``` pada file <b>/etc/dhcp/dhcpd.conf</b> untuk mengatur cara DHCP server menangani request DHCP dari subnet 10.6.3. Disini mereka akan diberikan ip 10.6.3.16 - 10.6.3.32, atau 10.6.3.64 - 10.6.3.80. Router dari subnet tersebut adalah 10.6.3.0, kemudian IP dari server DNS yang digunakan adalah 10.6.1.2 
