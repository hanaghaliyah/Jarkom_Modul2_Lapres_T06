# Jarkom_Modul2_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 2 - DNS dan Web Server <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
![in progress](https://user-images.githubusercontent.com/26424136/98954663-9990e580-2530-11eb-9bae-56838510560c.jpg)
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

#### NOMOR 1 dan 2
##### Soal
Kalian diminta untuk membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw yang memiliki (2) alias http://www.semeruyyy.pw
##### Penyelesaian
Pertamax

#### NOMOR 3
##### Soal
subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO

#### NOMOR 4
##### Soal
Buatkan reverse domain untuk domain utama.

#### NOMOR 5
##### Soal
Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan <b>DNS Server Slave</b> pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website.

#### NOMOR 6
##### Soal
Selain website utama Bibah juga meminta dibuatkan <b>subdomain</b> dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO.

#### NOMOR 7
##### Soal 
Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan <b>subdomain</b> dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

#### NOMOR 8
##### Soal 
Setelah selesai membuat keseluruhan domain, kamu diminta untuk segera mengatur web server. Domain http://semeruyyy.pw memiliki DocumentRoot pada /var/www/semeruyyy.pw.
##### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available`
- Buka file <b>semerut06.pw</b> dan ubah <b>DocumentRoot</b> menjadi `/var/www/semerut06.pw`
- Aktifkan konfigurasi <b>semerut06.pw</b>
Gunakan perintah `a2ensite semerut06.pw`
- Restart Apache <br>
Gunakan perintah `service apache2 restart`

#### NOMOR 9
##### Soal 
Awalnya web dapat diakses menggunakan alamat http://semeruyyy.pw/index.php/home. Karena dirasa alamat urlnya kurang bagus, maka (9) diaktifkan mod rewrite agar urlnya menjadi http://semeruyyy.pw/home.

##### Penyelesaian
- Pindah Ke directory `/var/www/semerut06.pw`
- Buat file <b>.htaccess</b> dengan isi file
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>semerut06.pw</b> dan tambahkan 
```
<Directory /var/www/semerut06.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
</Directory>
```
- Restart apache dengan perintah `service apache2 restart`


#### NOMOR 10
##### Soal
Web http://penanjakan.semeruyyy.pw akan digunakan untuk menyimpan assets file yang memiliki DocumentRoot pada /var/www/penanjakan.semeruyyy.pw dan memiliki struktur
folder sebagai berikut:
```
/var/www/penanjakan.semeruyyy.pw    /public/javascripts
                                    /public/css
                                    /public/images
                                    /errors
```

##### Penyelesaian
- Pindah ke directory `/var/www/penanjakan.semerut06.pw`
- Buat directory-directory yang diperlukan oleh website http://penanjakan.semerut06.pw <br>
Gunakan perintah-perintah berikut ini:
```
mkdir /var/www/penanjakan.semerut06.pw/public/javascripts
mkdir /var/www/penanjakan.semerut06.pw/public/css
mkdir /var/www/penanjakan.semerut06.pw/public/images
mkdir /var/www/penanjakan.semerut06.pw/errors
```
- Kemudian tampilkan list folder yang ada di `/var/www/penanjakan.semerut06.pw` menggunakan perintah `ls`

#### NOMOR 11
##### Soal 
Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.
##### Penyelesaian
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
- Restart apache dengan perintah `service apache2 restart`

#### NOMOR 12
##### Soal 
Untuk mengatasi HTTP Error code 404, disediakan file 404.html pada folder /errors untuk mengganti error default 404 dari Apache.
##### Penyelesaian
![ongoing](https://user-images.githubusercontent.com/26424136/98954441-5df61b80-2530-11eb-99ae-ae081c19b592.jpg)

#### NOMOR 13
##### Soal
Untuk mengakses file assets javascript awalnya harus menggunakan url http://penanjakan.semeruyyy.pw/public/javascripts. Karena terlalu panjang maka dibuatkan konfigurasi virtual host agar ketika mengakses file assets menjadi http://penanjakan.semeruyyy.pw/js.
##### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>penanjakan.semerut06.pw</b> dan tambahkan
```
Alias "/js" "/var/www/penanjakan.semerut06.pw/public/javascript"
```
- Restart apache dengan perintah `service apache2 restart`

#### NOMOR 14
##### Soal
Untuk web http://gunung.semeruyyy.pw belum dapat dikonfigurasi pada web server karena menunggu pengerjaan website selesai. Sedangkan web http://naik.gunung.semeruyyy.pw
sudah bisa diakses hanya dengan menggunakan port 8888. DocumentRoot web berada pada /var/www/naik.gunung.semeruyyy.pw.
##### Penyelesaian
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>naik.gunung.semerut06.pw</b>
- Tambahkan port yang digunakan
```
<VirtualHost *:8888>
```
- Tambahkan directory tempat file website kita berada
```
DocumentRoot /var/www/naik.gunung.semerut06.pw
```
- Tambahkan port 8888 pada file <b>ports.conf</b>. File </b>ports.conf</b> berada pada directory `/etc/apache2`. Cara menambahkan port yang perlu didengar adalah dengan menuliskan
```
Listen 8888
```
- Restart apache <br>
Gunakan perintah `service apache2 restart`

#### NOMOR 15
##### Soal
Dikarenakan web http://naik.gunung.semeruyyy.pw bersifat private. Bibah meminta kamu membuat web http://naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya aman dan tidak sembarang orang bisa mengaksesnya.
##### Penyelesaian
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
- Pindah ke directory `/etc/apache2/sites-enabled` kemudian buka file <b>naik.gunung.semerut06.pw</b> dan tambahkan
```
<Directory "/var/www/naik.gunung.semerut06.pw">
     AuthType Basic
     AuthName "Restricted Content"
     AuthUserFile /etc/apache2/.htpasswd
     Require valid-user
</Directory>
```


#### NOMOR 16
##### Soal
Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama http://semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http://semeruyyy.pw.
##### Penyelesaian
Pindah ke directory `/etc/apache2` kemudian buka file <b>default</b> dan tambahkan
```
Redirect / http://semerut06.pw
```

#### NOMOR 17
##### Soal
Karena pengunjung pada /var/www/penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

##### Penyelesaian
- Pindah ke directory `/var/www`
- Buat file <b>.htaccess</b> dengan isi file
``` 
RewriteEngine on
RewriteCond %{REQUEST_URI} semeru
RewriteRule .* index.php 
```
- Pindah ke directory `/etc/apache2/sites-available` kemudian buka file <b>semerut06.pw</b> dan tambahkan
```
<Directory /var/www/semerut06.pw>
     Options +FollowSymLinks -Multiviews
     AllowOverride All
</Directory>
```
- Restart apache dengan perintah `service apache2 restart`

