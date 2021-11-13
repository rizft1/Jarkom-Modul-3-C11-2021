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

### Soal 9.

### Soal 10.

### Soal 11.
Pada EniesLobby file `/etc/bind/named.conf.local`
```
zone "super.franky.C11.com" {
        type master;
        file "/etc/bind/kaizoku/super.franky.C11.com";
        // allow-transfer { 192.189.3.69; };
};
```
Selanjutnya menjalankan command `service bind9 restart`

Pada Skypie jalankan `bash command.sh`
```
apt-get install wget
apt-get install unzip
mkdir /var/www/super.franky.c11.com
wget https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/mainn
/super.franky.zip

unzip super.franky.zip
mv super.franky/error /var/www/super.franky.c11.com
mv super.franky/public /var/www/super.franky.c11.com
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/suu
per.franky.c11.com.conf
```
Juga jalankan `bash script.sh`
```
echo '<VirtualHost *:80>
                ServerAdmin webmaster@localhost
        ServerName super.franky.C11.com
        ServerAlias www.super.franky.C11.com

        DocumentRoot /var/www/super.franky.C11.com
       <Directory /var/www/html>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
</Directory>
 ErrorLog ${APACHE_LOG_DIR}/error.log
 CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/super.franky.C11.com.conf
```
Kemudian buka `/etc/apache2/sites-available` dan jalankan `a2ensite super.franky.C11.com.conf` </br>
Selanjutnya jalankan `service apache2 restart`

Ke Water7 dan tambahkan
```
include /etc/squid/acl.conf
include /etc/squid/acl-bandwith.conf
http_port 5000
visible_hostname jualbelikapal.C11.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

acl google dstdomain google.com
http_access deny google
deny_info http://super.franky.C11.com/ google

#acl BLACKLIST dstdomain google.com
#deny_info http://super.franky.c11.com/ BLACKLIST
#http_reply_access deny BLACKLIST

http_access allow USERS

http_access deny all
```

Kemudian ke Loguetown dan ketik `lynx google.com` maka akan mengarahkan ke `super.franky.c11.com`
![11](https://user-images.githubusercontent.com/57831206/141647813-a29a1d91-7be3-4847-8f1f-551c6c7dee15.JPG)

### Soal 12.

### Soal 13. 
