# Jarkom_Modul2_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 2 - DNS dan Web Server <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

### NOMOR 1 dan 2
##### Soal
Kalian diminta untuk membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw yang memiliki (2) alias http://www.semeruyyy.pw
##### Penyelesaian

### NOMOR 3
##### Soal
subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO

### NOMOR 4
##### Soal
Buatkan reverse domain untuk domain utama.

### NOMOR 5
##### Soal
Untuk mengantisipasi server dicuri/rusak, Bibah minta dibuatkan <b>DNS Server Slave</b> pada MOJOKERTO agar Bibah tidak terganggu menikmati keindahan Semeru pada Website.

### NOMOR 6
##### Soal
Selain website utama Bibah juga meminta dibuatkan <b>subdomain</b> dengan alamat http://gunung.semeruyyy.pw yang didelegasikan pada server MOJOKERTO dan mengarah ke IP Server PROBOLINGGO.

### NOMOR 7
##### Soal 
Bibah juga ingin memberi petunjuk mendaki gunung semeru kepada anggota komunitas sehingga dia meminta dibuatkan <b>subdomain</b> dengan nama http://naik.gunung.semeruyyy.pw, domain ini diarahkan ke IP Server PROBOLINGGO.

### NOMOR 15
##### Soal
Bibah meminta kamu membuat web http://naik.gunung.semeruyyy.pw agar diberi autentikasi password dengan username “semeru” dan password “kuynaikgunung” supaya aman dan tidak sembarang orang bisa mengaksesnya.
##### Penyelesaian


### NOMOR 16
##### Soal
Saat Bibah mengunjungi IP PROBOLINGGO, yang muncul bukan web utama http://semeruyyy.pw melainkan laman default Apache yang bertuliskan “It works!”. Karena dirasa kurang profesional, maka setiap Bibah mengunjungi IP PROBOLINGGO akan dialihkan secara otomatis ke http://semeruyyy.pw.
##### Penyelesaian
Di file default
tambah 
```
Redirect / http://semerut06.pw
```

### NOMOR 17
##### Soal
Karena pengunjung pada /var/www/penanjakan.semeruyyy.pw/public/images sangat banyak maka semua request gambar yang memiliki substring “semeru” akan diarahkan menuju semeru.jpg.

##### Penyelesaian
Buat file .htaccess <br>
``` 
RewriteEngine on
RewriteCond %{REQUEST_URI} foobar
RewriteRule .* index.php 
```
