# Jarkom_Modul2_Lapres_T06
<b> Laporan Resmi Praktikum Jaringan Komputer </b> <br>
Modul 2 - DNS dan Web Server <br>
Kelompok T06
1. Hana Ghaliyah Azhar  (05311840000032)
2. Azmi                 (05311840000047)

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
Redirect / http://www.domain2.com
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
