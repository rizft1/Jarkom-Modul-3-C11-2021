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
![2-6 switch 1](https://user-images.githubusercontent.com/57831206/141648640-913df4d5-4c66-4ab6-a890-17c732fe62ba.JPG)

dan menambahkan baris ini pada subnet 192.189.3.0
```
        default-lease-time 720;
        max-lease-time 7200;
```
![2-6 switch 3](https://user-images.githubusercontent.com/57831206/141648642-c776c457-b011-4740-8204-f09665b9fa5a.JPG)

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
![7](https://user-images.githubusercontent.com/57831206/141648644-7965b1a0-09c2-42ff-bc1f-cca4143def85.JPG)

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

Setelah di hidupkan proxynya, akses jualbelikapal.C11.com di Loguetown <br> 
![8](https://user-images.githubusercontent.com/57831206/141648653-3648c786-b742-4ae9-9eee-5bcf0667149c.JPG)

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

Jika mengakses di luar jam yang ditentukan hasilnya adalah sebagai berikut : <br> 
![10](https://user-images.githubusercontent.com/57831206/141648662-3827a820-4643-474d-8bba-fa24fd233693.JPG)

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

Ke Water7 kemudian menuju ke `/etc/squid/squid.conf` dan tambahkan 
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
Juga tambahkan nameserver EniesLobby `nameserver 192.189.2.2` pada file `resolv.conf`

Kemudian ke Loguetown dan ketik `lynx google.com` maka akan mengarahkan ke `super.franky.c11.com`
![11](https://user-images.githubusercontent.com/57831206/141647813-a29a1d91-7be3-4847-8f1f-551c6c7dee15.JPG)

### Soal 12.
Masuk ke Water7 dan pada `/etc/squid/acl-bandwidth.conf` tambahkan
```
acl luffy url_regex -i \.png$
    acl luffy url_regex -i \.jpg$

delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 allow luffy
delay_access 1 deny zoro
delay_access 1 allow download
delay_access 1 deny all
```
![12](https://user-images.githubusercontent.com/57831206/141648249-4658459f-cc03-4cc8-bebe-6643e974a8fe.JPG)

Kembali ke Loguetown dan lakukan `lynx google.com` yang mengarah ke `super.franky.C11.com`. </br>
Ketik username `luffybelikapalc11` dan passwordnya `luffy_c11`. </br>
Selanjutnya lakukan download image, dan akan muncul kecepatan download data
![12-2](https://user-images.githubusercontent.com/57831206/141648251-2a62872e-e518-49dc-a1b7-8a528e5d8cf4.JPG)

### Soal 13.
Masuk ke Water7 dan pada `/etc/squid/acl-bandwidth.conf` tambahkan
```
delay_class 2 1
delay_parameters 2 -1/-1
delay_access 2 allow zoro
delay_access 2 deny luffy
delay_access 2 deny all
```

Kembali lagi ke Loguetown dan lakukan `lynx google.com` yang mengarah ke `super.franky.C11.com`. </br>
Ketik username `zorobelikapalc11` dan passwordnya `zoro_c11`. </br>
Masuk ke `bukanfrankytapirandom.99689` lalu enter
![13-1](https://user-images.githubusercontent.com/57831206/141648472-270c6d9e-9a07-4488-84cd-c0fa793c3859.JPG)

Maka akan terlihat hasilnya dengan cepat seperti di bawah ini
![13](https://user-images.githubusercontent.com/57831206/141648476-21f3ecfb-f8e0-40ce-bac5-309f9253eb06.JPG)

