# Jarkom-Modul-3-C11-2021

## Anggota Kelompok C11 : <br>
- 05111940000001 Christopher Baptista
- 05111940000093 Riki Mi’roj Achmad
- 05111940000219 M Rizqi Fiqih Thalib

### Soal 1 
Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server
jawab : <br>

EniesLobby sebagai DNS Server, maka di EniesLobby diinstall Bind9

```
apt-get update
apt-get install bind9 -y
```

Jipangu sebagai DHCP Server, maka pada jipangu diinstall isc-dhcp-server
Pada Jipangu jalankan command

```
apt-get update
apt-get install isc-dhcp-server -y
```

Kemudian setting INTERFACES yang digunakan oleh Jipangu pada file /etc/default/isc-dhcp-server 

```
INTERFACES="eth0"
```

Water7 sebagai Proxy Server dan Water7 diinstall squid

```
apt-get update
apt-get install squid -y
```

Setelah Proxy Server diinstall lalu check status nya dengan

```
service squid status
```

Jika status squid belum running. Maka lakukan command ini terlebih dahulu

```
service squid start
```

check kembali status dan squid akan running

### Soal 2.  
Foosha sebagai DHCP Relay
Jawab : <br>

Foosha sebagai DHCP Relay atau router install isc-dhcp-relay
Pada Foosha jalankan command 

```
apt-get update
apt-get install isc-dhcp-relay -y
```
Kemudian edit file /etc/default/isc-dhcp-relay dengan menambahkan ```SERVER = "IP Jipangu" ``` dan ``` INTERFACES = "eth1 eth2 eth3" ```

Setelah itu, pada bagian "Additional options that are passed to the DHCP relay daemon" boleh dikosongkan

untuk melihat hasil tersebut, bisa buka pada Foosha

```
nano /etc/default/isc-dhcp-relay
```


### Soal 3. 
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169.
Jawab : <br>

Pada node Jipangu buka " etc/dhcp/dhcpd.conf"
Pada Jipangu jalankan command 

```
nano /etc/dhcp/dhcpd.conf
```
lalu tambahkan konfigurasi berikut

```
  subnet 192.189.1.0 netmask 255.255.255.0 {
        range 192.189.1.20 192.189.1.99;
        range 192.189.1.150 192.189.1.169;
        option routers 192.189.1.1;
        option broadcast-address 192.189.1.255;
        option domain-name-servers 192.189.2.2;
        default-lease-time 360;
        max-lease-time 7200;
    }
```
Lalu jalankan command ```service isc-dhcp-server restart```

### Soal 4. 
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50 Jawab : <br>

pada Jipangu buka "/etc/dhcp/dhcpd.conf"

```
nano /etc/dhcp/dhcpd.conf
```

lalu tambahkan konfigurasi berikut

```
   subnet 192.189.3.0 netmask 255.255.255.0 {
        range 192.189.3.30 192.189.3.50;
        option routers 192.189.3.1;
        option broadcast-address 192.189.3.255;
        option domain-name-servers 192.189.2.2;
        default-lease-time 720;
        max-lease-time 7200;
    }
```

Lalu jalankan command service isc-dhcp-server restart

### Soal 5. 
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut Jawab : <br>
pada Jipangu "/etc/dhcp/dhcpd.conf" tambahkan konfigurasi 

Ditambahkan ```192.189.2.2``` sebagai DNS nya yang meripakan IP dari DNS Server yaitu EniesLobby


### Soal 6. 
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit Jawab : <br>

Edit file /etc/dhcp/dhcpd.conf dengan menambahkan baris ini pada subnet 192.189.1.0
```
        default-lease-time 360;
        max-lease-time 7200;
```
dan menambahkan baris ini pada subnet 192.189.3.0
```
        default-lease-time 720;
        max-lease-time 7200;
```

### Soal 7. 
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69
Jawab : <br>
Pada Jipangu "/etc/dhcp/dhcpd.conf" ditambahkan konfigurasi berikut

```
host Skypie {
    hardware ethernet d2:61:cf:ce:81:09;
    fixed-address 192.189.3.69;
}
```

yang mana ```hardware ethernet d2:61:cf:ce:81:09``` bisa diliat pada node Skypie dengan menjalankan perintah ```ip a```. Kemudian liat pada bagian eth0, dan liat pada bagian link/ether

Lalu jalankan command ```service isc-dhcp-server restart```

pada node skypie buka "/etc/network/interfaces"

```
nano /etc/network/interfaces
```

dan tambahkan konfigurasi berikut

```
hwaddress ether d2:61:cf:ce:81:09
```
restart Node melalui GNS3. Dan IP pada skypie akan berubah menjadi ```192.189.3.69```

### Soal 8. 
Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000 jawab : <br>

Buatlah Domain jualbelikapal.C11.com pada EniesLobby menggunakan bind9
Buat Config di Bind9 Enies Lobby

Config Domainnya adalah sebagai berikut dengan membuat file bernama jualbelikapal.C11.com :

untuk named.conf.local kita ubah menjadi seperti berikut :


Pada node Water7 lakukan 

```
nano /etc/squid/squid.conf
```

kemudian isikan konfigurasi berikut

```
http_port 5000
visible_hostname jualbelikapal.C11.com
http_access allow all
```

lalu simpan dan restart squid

```
service squid restart
```

Kemudian pada Loguetown, aktifkan Proxynya dengan

```
export http_proxy="http://jualbelikapal.C11.com:5000"
```

cek proxy lakukan command
```
env | grep -i jual
```

Hasilnya adalah sebagai berikut : <br>
(gambar)

Setelah di hidupkan proxynya, akses jualbelikapal.C11.com di Loguetown
<br> (gambar)

### Soal 9. 
Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

Pada node Water7 dijalankan berikut

Untuk membuat username serta password dilakukan perintah berikut :
```
htpasswd -m /etc/squid/passwd luffybelikapalC11
htpasswd -m /etc/squid/passwd zorobelikapalC11
```

Setelah itu memasukkan ke config squidnya di squid.conf :
    http_port 5000
    visible_hostname jualbelikapal.C11.com
    auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
    auth_param basic children 5
    auth_param basic realm Login
    auth_param basic credentialsttl 2 hours
    auth_param basic casesensitive on
    acl USERS proxy_auth REQUIRED
    http_access allow USERS

kemudian jalankan ```command service squid restart```


### Soal 10. 
Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00) Jawab : <br>

Buat Config di file acl.conf di Water7
```
acl time_done_1 time S 00:00-23:59
acl time_done_2 time MT 00:00-06:59
acl time_done_3 time M 11:01-23:59
acl time_done_4 time TWH 11:01-16:59
acl time_done_5 time WH 03:01-06:59
acl time_done_6 time F 03:01-16:59
acl time_done_7 time A 03:01-23:59
```

Hasilnya adalah sebagai berikut : 
<br> (gambar)

Ubah juga config di squid.conf dengan konfigurasi seperti berikut :
```
include /etc/squid/acl.conf
http_port 5000
visible_hostname jualbelikapal.C11.com

http_access deny time_done_1
http_access deny time_done_2
http_access deny time_done_3
http_access deny time_done_4
http_access deny time_done_5
http_access deny time_done_6
http_access deny time_done_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
```
Kemudian jalankan command ```service squid restart```
