# Jarkom_Modul2_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 2 - DNS dan Web Server <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### NOMOR 1 dan 2
#### Soal
Kalian diminta untuk membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw yang memiliki (2) alias http://www.semeruyyy.pw
#### Penyelesaian
- Buka `nano /etc/bind/named.conf.local` pada <b>UML Malang</b>
- Kemudian membuat konfigurasi domain semerut06.pw dengan syntax
```
zone "semerut06.pw" {
	type master;
	file "/etc/bind/semeru/semerut06.pw";
};
```
- Buat folder <b>semeru</b> di `/etc/bind 
- Copy file <b>db.local</b> ke <b>semerut06.pw</b> dengan perintah `cp /etc/bind/db.local /etc/bind/jarkom/jarkom2020.com`
- Kemudian `nano /etc/bind/semeru/semerut06.pw` dan diubah
```
@	IN	NS	semerut06.pw
@	IN	A	10.151.73.178	; IP Malang
www	IN	CNAME	semerut06.pw	; Alias
```
- Restart dengan perintah `service bind9 restart`
#### Testing
Nomor 1 <br>
<img width="365" alt="1" src="https://user-images.githubusercontent.com/26424136/99098755-062ce280-260c-11eb-921a-cb5bede76949.PNG"> <br>
Nomor 2 <br>
<img width="363" alt="2" src="https://user-images.githubusercontent.com/26424136/99098766-088f3c80-260c-11eb-80fc-61c82ab6a9c6.PNG">

### NOMOR 3
#### Soal
subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
#### Penyelesaian
- Buka file dengan `nano /etc/bind/semeru/semerut06.pw`
- Tambah subdomain <b>penanjakan.semerut06.pw</b> dan arahkan IP nya ke IP Server Probolinggo
```
penanjakan	IN	A	10.151.73.180	; IP Probolinggo
```
- Restart dengan perintah `service bind9 restart`
#### Testing
<img width="365" alt="3" src="https://user-images.githubusercontent.com/26424136/99102805-2f9c3d00-2611-11eb-8854-cf76e07cfb34.PNG">

### NOMOR 4
#### Soal
Buatkan reverse domain untuk domain utama.
#### Penyelesaian
- Edit file `nano /etc/bind/named.conf.local`
- Tambahkan syntax berikut
```
zone "73.151.10.in-addr.arpa" {
    type master;
    file "/etc/bind/semeru/73.151.10.in-addr.arpa";
};
```
- Copy file <b>db.local</b> ke <b>73.151.10.in-addr.arpa</b> dengan perintah `cp /etc/bind/db.local /etc/bind/jarkom/73.151.10.in-addr.arpa`
- Kemudian `nano /etc/bind/semeru/73.151.10.in-addr.arpa` dan diubah
```
178	IN	PTR	semerut06.pw
```
- Restart bind9, `service bind9 restart`

#### Testing
<img width="365" alt="4" src="https://user-images.githubusercontent.com/26424136/99102814-33c85a80-2611-11eb-8188-3fc2b617a1ec.PNG">

### NOMOR 5
#### Soal
Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan <b>DNS Server Slave</b> pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website.
#### Penyelesaian
- Ubah konfigurasi pada <b>Server Malang</b> dengan `nano /etc/bind/named.conf.local`
```
zone "semerut06.pw" {
    type master;
    notify yes;
    also-notify { 10.151.73.179; }; //IP Mojokerto
    allow-transfer { 10.151.73.179; }; //IP Mojokerto
    file "/etc/bind/semeru/semerut06.pw";
};
```
- Restart bind9, `service bind9 restart`
- Kemudian ke <b>Server Mojokerto</b> dan update terlebih dahulu `apt-get update` dan install bind9 jika belum
- Ubah konfigurasi pada <b>Server Mojokerto</b> dengan `nano /etc/bind/named.conf.local`
```
zone "jarkom2020.com" {
    type slave;
    masters { 10.151.73.178; }; //IP Malang
    file "/var/lib/bind/semerut06.pw";
};
```
- Restart bind9, `service bind9 restart`
#### Testing
<img width="729" alt="5" src="https://user-images.githubusercontent.com/26424136/99102817-3460f100-2611-11eb-927b-40f81ae39468.PNG">

### NOMOR 6
#### Soal
Selain website utama Bibah juga meminta dibuatkan <b>subdomain</b> dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO.
#### Penyelesaian
- Ubah konfigurasi pada <b>Server Malang</b> dengan `nano /etc/bind/named.conf.local`
```
ns1	IN	A	10.151.73.179	; IP Mojokerto
gunung	IN	A	ns1
```
- Kemudian `nano /etc/bind/named.conf.options` pada <b>Server Malang</b> dan bagian <b>dnssec-validation auto;</b> dijadikan komen dan tambahkan <b>allow-query{any;};</b>
- Buka `nano /etc/bind/named.conf.local` pada <b>UML Malang</b> dan ubah syntaxnya
```
zone "semerut06.pw" {
    type master;
    allow-transfer { 10.151.73.179; }; //IP Mojokerto
    file "/etc/bind/semeru/semerut06.pw";
};
```
- Restart bind9, `service bind9 restart`
- Kemudian `nano /etc/bind/named.conf.options` pada <b>Server Mojokerto</b> dan bagian <b>dnssec-validation auto;</b> dijadikan komen dan tambahkan <b>allow-query{any;};</b>
- Buka `nano /etc/bind/named.conf.local` pada <b>UML Mojokerto</b> dan ubah syntaxnya
```
zone "semerut06.pw" {
    type master;
    allow-transfer { any; };
    file "/etc/bind/delegasi/gunung.semerut06.pw";
};
```
- Kemudian buat folder <b>delegasi</b>
- Dan copy file <b>db.local</b> ke <b>gunung.semerut06.pw</b> dengan perintah `cp /etc/bind/db.local /etc/bind/delegasi/gunung.semerut06.pw`
- Ubah isinya `/etc/bind/delegasi/gunung.semerut06.pw`
```
@	IN	NS	gunung.semerut06.pw
@	IN	A	10.151.73.180	; IP Probolinggo
```
- Restart bind9, `service bind9 restart`
#### Testing
<img width="364" alt="6" src="https://user-images.githubusercontent.com/26424136/99102821-362ab480-2611-11eb-8649-b797e6efec5a.PNG">

### NOMOR 7
#### Soal 
Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan <b>subdomain</b> dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.
#### Penyelesaian
- `nano /etc/bind/delegasi/gunung.semerut06.pw` pada <b>Server Mojokerto</b>
- Tambahkan subdomain
```
naik	IN	A	10.151.73.180	; IP Probolinggo
```
- Restart bind9, `service bind9 restart`
#### Testing
<img width="366" alt="7" src="https://user-images.githubusercontent.com/26424136/99102825-36c34b00-2611-11eb-878c-7d26ee09f860.PNG">

### NOMOR 8
#### Soal 
Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server. Domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw.
#### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available`
- Buka file <b>semerut06.pw</b> dan ubah <b>DocumentRoot</b> menjadi `/var/www/semerut06.pw` <br>
<img width="364" alt="8b" src="https://user-images.githubusercontent.com/26424136/99148348-c1f81b80-26b9-11eb-914f-6043cbd82507.PNG"> <br>
- Aktifkan konfigurasi <b>semerut06.pw</b>
Gunakan perintah `a2ensite semerut06.pw`
- Restart Apache <br>
Gunakan perintah `service apache2 restart`
#### Testing
![8](https://user-images.githubusercontent.com/26424136/99102829-375be180-2611-11eb-85cd-46e5cdfdd6e9.png)

### NOMOR 9
#### Soal 
Awalnya web dapat diakses menggunakan alamat http://semeruyyy.pw/index.php/home. Karena dirasa alamat urlnya kurang bagus, maka (9) diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.

#### Penyelesaian
- Pindah Ke directory `/var/www/semerut06.pw`
- Buat file <b>.htaccess</b> dengan isi file
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```
<img width="365" alt="9b" src="https://user-images.githubusercontent.com/26424136/99148351-c3c1df00-26b9-11eb-961c-7eed45318d81.PNG"> <br>
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>semerut06.pw</b> dan tambahkan 
```
<Directory /var/www/semerut06.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
</Directory>
```
<img width="365" alt="9c" src="https://user-images.githubusercontent.com/26424136/99148353-c45a7580-26b9-11eb-9451-3e30759ffc16.PNG"> <br>
- Restart apache dengan perintah `service apache2 restart`
#### Testing
![9](https://user-images.githubusercontent.com/26424136/99102851-3e82ef80-2611-11eb-812b-b443177594c1.png)


### NOMOR 10
#### Soal
Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut:
```
/var/www/penanjakan.semeruyyy.pw    /public/javascripts
                                    /public/css
                                    /public/images
                                    /errors
```

#### Penyelesaian
- Pindah ke directory `/var/www/penanjakan.semerut06.pw`
- Buat directory-directory yang diperlukan oleh website http://penanjakan.semerut06.pw <br>
Gunakan perintah-perintah berikut ini:
```
mkdir /var/www/penanjakan.semerut06.pw/public/javascripts
mkdir /var/www/penanjakan.semerut06.pw/public/css
mkdir /var/www/penanjakan.semerut06.pw/public/images
mkdir /var/www/penanjakan.semerut06.pw/errors
```
- Kemudian tampilkan list folder yang ada di `/var/www/penanjakan.semerut06.pw` menggunakan perintah `ls` <br>
<img width="367" alt="10 b" src="https://user-images.githubusercontent.com/26424136/99148173-e43d6980-26b8-11eb-9c84-1c1dda853e14.PNG">

#### Testing
![10](https://user-images.githubusercontent.com/26424136/99102857-417de000-2611-11eb-9d45-53aa06ddcea6.png)

### NOMOR 11
#### Soal 
Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.
#### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>penanjakan.semerut06.pw</b> dan tambahkan
```
<Directory /var/www/penanjakan.semerut06.pw/public>
     Options +Indexes
</Directory>
<Directory /var/www/penanjakan.semerut06.pw/public/javascripts>
     Options -Indexes
</Directory>
<Directory /var/www/penanjakan.semerut06.pw/public/css>
     Options -Indexes
</Directory>
<Directory /var/www/penanjakan.semerut06.pw/public/images>
     Options -Indexes
</Directory>
```
<img width="362" alt="11b" src="https://user-images.githubusercontent.com/26424136/99148085-52cdf780-26b8-11eb-8afe-4401848da87f.PNG"> <br>
- Restart apache dengan perintah `service apache2 restart`

#### Testing
![11](https://user-images.githubusercontent.com/26424136/99102858-42167680-2611-11eb-8fc7-b1650f2711e9.png)

### NOMOR 12
#### Soal 
Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.
#### Penyelesaian
Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>penanjakan.semerut06.pw</b> dan tambahkan
```
ErrorDocument 404 /errors/404.html
```
<img width="363" alt="12b" src="https://user-images.githubusercontent.com/26424136/99148028-0aaed500-26b8-11eb-9a24-cc5619ae13da.PNG">

#### Testing
![12](https://user-images.githubusercontent.com/26424136/99102861-42af0d00-2611-11eb-9a20-42b5b0c1f626.png)

### NOMOR 13
#### Soal
Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semeruyyy.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semeruyyy.pw/js.
#### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>penanjakan.semerut06.pw</b> dan tambahkan
```
Alias "/js" "/var/www/penanjakan.semerut06.pw/public/javascript"
```
<img width="366" alt="13 b" src="https://user-images.githubusercontent.com/26424136/99147987-bb68a480-26b7-11eb-8524-f37b05ee079b.PNG"> <br>
- Restart apache dengan perintah `service apache2 restart`

#### Testing
Halaman dengan alamat http://penanjakan.semeruyyy.pw/js ketika diakses menampilkan <b>403 Forbidden</b> dikarenakan `folder yang berada di dalam /public tidak dibolehkan directory listing.` <br>
![13](https://user-images.githubusercontent.com/26424136/99102862-4347a380-2611-11eb-9521-d524efef502f.png)

### NOMOR 14
#### Soal
Untuk web http://gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai. Sedangkan web http://naik.gunung.semeruyyy.pw
sudah bisa diakses hanya dengan menggunakan port 8888. DocumentRoot web berada pada /var/www/naik.gunung.semeruyyy.pw.
#### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>naik.gunung.semerut06.pw</b>
- Tambahkan port yang digunakan
```
<VirtualHost *:8888>
```
- Tambahkan directory tempat file website kita berada
```
DocumentRoot /var/www/naik.gunung.semerut06.pw
```
<img width="365" alt="14 a" src="https://user-images.githubusercontent.com/26424136/99147948-6d53a100-26b7-11eb-8cd8-3326d62fc82c.PNG"><br>
- Tambahkan port 8888 pada file <b>ports.conf</b>. File </b>ports.conf</b> berada pada directory `/etc/apache2`. Cara menambahkan port yang perlu didengar adalah dengan menuliskan
```
Listen 8888
```
<img width="364" alt="14 b" src="https://user-images.githubusercontent.com/26424136/99147951-6f1d6480-26b7-11eb-91bd-9161348dc824.PNG"> <br>
- Restart apache <br>
Gunakan perintah `service apache2 restart`

#### Testing
![no 14 15](https://user-images.githubusercontent.com/26424136/99102871-45116700-2611-11eb-9caf-cf2adb8c5a42.png)

### NOMOR 15
#### Soal
Dikarenakan web http://naik.gunung.semeruyyy.pw bersifat private. Bibah meminta kamu membuat web http://naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya aman dan tidak sembarang orang bisa mengaksesnya.
#### Penyelesaian
- Pindah ke directory `/etc/apache2` kemudian buka file <b>naik.gunung.semerut06.pw</b>
- Install apache2 utils
```
apt-get update
apt-get install apache2-utils
```
- Setting usernamenya di `/etc/apache2`
```
htpasswd -c /etc/apache2/.htpasswd semeru
```
- Lalu akan muncul permintaan untuk mengisi password
```
kuynaikgunung
```
<img width="366" alt="15b" src="https://user-images.githubusercontent.com/26424136/99147838-9e7fa180-26b6-11eb-8270-350818177df3.PNG"> <br>
- Pindah ke directory `/etc/apache2/sites-enabled` kemudian buka file <b>naik.gunung.semerut06.pw</b> dan tambahkan
```
<Directory "/var/www/naik.gunung.semerut06.pw">
     AuthType Basic
     AuthName "Restricted Content"
     AuthUserFile /etc/apache2/.htpasswd
     Require valid-user
</Directory>
```
<img width="367" alt="15" src="https://user-images.githubusercontent.com/26424136/99147801-5d878d00-26b6-11eb-9cf7-593986a3fafa.PNG">

#### Testing
Masukkan username `semeru` dan password `kuynaikgunung` <br>
![no 14 15](https://user-images.githubusercontent.com/26424136/99102871-45116700-2611-11eb-9caf-cf2adb8c5a42.png)
Login berhasil dan website bisa diakses <br>
![no 14 15 b](https://user-images.githubusercontent.com/26424136/99102864-43e03a00-2611-11eb-87e7-b517c3b37791.png)

### NOMOR 16
#### Soal
Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama http://semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http://semeruyyy.pw.
#### Penyelesaian
Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>default</b> dan tambahkan
```
Redirect / http://semerut06.pw
```
<img width="364" alt="16c" src="https://user-images.githubusercontent.com/26424136/99147728-ca4e5780-26b5-11eb-8fb5-e284fdc83018.PNG"> <br>

#### Testing
![16a](https://user-images.githubusercontent.com/26424136/99147613-f7e6d100-26b4-11eb-9b19-366216a48384.png)
![8](https://user-images.githubusercontent.com/26424136/99102829-375be180-2611-11eb-85cd-46e5cdfdd6e9.png)

### NOMOR 17
#### Soal
Karena pengunjung pada /var/www/penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

#### Penyelesaian
- Pindah ke directory `/var/www/penanjakan.semerut06.pw`
- Buat file <b>.htaccess</b> dengan isi file
``` 
RewriteEngine on
RewriteRule ^(.*)semeru(.*)$ /public/images/semeru.jpg 
```
<img width="365" alt="no 17" src="https://user-images.githubusercontent.com/26424136/99147539-72fbb780-26b4-11eb-98aa-d71c39ae9e3c.PNG"> <br>
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>penanjakan.semerut06.pw</b> dan tambahkan
```
<Directory /var/www/penanjakan.semerut06.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
</Directory>
```
<img width="365" alt="no 17b" src="https://user-images.githubusercontent.com/26424136/99147677-6fb4fb80-26b5-11eb-8adb-19520119567a.PNG"> <br>
- Restart apache dengan perintah `service apache2 restart`
#### Testing
![17](https://user-images.githubusercontent.com/26424136/99147536-70995d80-26b4-11eb-9b18-f9ef7e1401c0.png)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<b>Semangat uyy !!! || TUHAN BERSAMA MAHASISWA SEMESTER 5</b>
