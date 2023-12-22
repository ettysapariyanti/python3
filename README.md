# python3
Kumpulan Source Code Python 3


source code yang berhasil untuk input data ke database MySQL menggunakan module MySQLdb :


```python

import MySQLdb


koneksiKeDatabase = MySQLdb.connect("1.1.3.2","steven","kucing","latihan")
    
kursor = koneksiKeDatabase.cursor()


kueri = 'insert into komputer(kodedatabarang,tanggalpendataan,kodebarang) values ("kdb01","2022-06-27","02132");'

try:

    kursor.execute(kueri)
    
    koneksiKeDatabase.commit()
    
except (MySQLdb.Error, MySQLdb.Warning) as e:

    print (e)
    
    koneksiKeDatabase.rollback()
    
finally:

    koneksiKeDatabase.close()


```



contoh source code untuk di server Ubuntu yg sering mengalami masalah koneksi ke MySQL alias kesulitan untuk instal module di pip, yg berguna untuk terhubung ke server MySQL:

```python

import mysql.connector

databaseku = mysql.connector.connect(host="999.tcp.ap.ngrok.io",user="******",password="**********",auth_plugin="mysql_native_password",database="*********",port="19921")

print(databaseku)



```




source code ini berhasil untuk menampilkan isi data di MariaDB yg telah di query ke tampilan Jinja2 :



```python



#!/usr/bin/python


# source code untuk melanjutkan mengambil data dari mariadb dan menampilkannya di Jinja2


from jinja2 import Template

import mariadb

import sys


try:


    koneksi = mariadb.connect(user="steven",password="kucing",host="1.1.3.7",port=3306,database="latihan")
    

except mariadb.Error as e:

    print(f"Gagal Terhubung ke Server Mariadb: {e}")
    
    sys.exit(1)

    
kursor = koneksi.cursor()


kursor.execute("SELECT kodedatabarang,tanggalpendataan,kodebarang FROM datakomputer") 
    

for (kodedatabarang, tanggalpendataan, kodebarang) in kursor:

    kodeDataBarang2=kodedatabarang
    


data = {"kodeDataBarang3" : kodeDataBarang2}

template = "Isi Kolom Kode Data Barang : {{kodeDataBarang3}}"


tm = Template(template)


pesan = tm.render(data)


print(pesan)



```







# Source code SQLAlchemy yang berhasil dipakai untuk mengetest koneksi ke server MariaDB :



# SQLAlchemy


``` python

import mariadb

import sys

import sqlalchemy

from sqlalchemy.ext.declarative import declarative_base



# mengikuti tutorial di sini: https://stackoverflow.com/questions/32929318/is-there-a-way-to-test-an-sqlalchemy-connection

# ini berguna untuk test koneksi ke server MariaDB menggunakan SQLAlchemy





#Mengkoneksikan menggunakan MariaDB Konektor untuk python

engine = sqlalchemy.create_engine("mariadb+mariadbconnector://steven:kucing@1.1.3.4:3306/latihan")


engine.connect()


metadata = sqlalchemy.MetaData(bind=engine)

metadata.reflect(only=['datakomputer'])

print(metadata.tables)



```


Di bawah ini adalah source code untuk menampilkan data hasil query dari MariaDB untuk di cetak ke dalam file txt dengan menggunakan Jinja sebagai template engine 

``` python

import mariadb

import sys

import json

from jinja2 import Environment, FileSystemLoader


try:

    koneksi = mariadb.connect(user="steven",password="kucing",host="76t9cnaz3rytqrps1ey3gsysz3sp93xeto49ejtkpi3nrsjf5amy.loki",port=3306,database="saham")
    
    
except mariadb.Error as e:


    print(f"Gagal Terkoneksi Ke : {e}")
    
    sys.exit(1)
    
    
    
kursor = koneksi.cursor()

kursor.execute("SELECT kodedata,tanggalpendataan,bagiansubbagian,kodebarang,merekprinter FROM daftartintaprinter")

hasil = kursor.fetchall()


for data in hasil:

    kodedata = data[0]
    
    tanggalpendataan = data[1].strftime("%Y-%m-%d")
    
    bagiansubbagian = data[2]
    
    kodebarang = data[3]
    
    merekprinter = data[4]
    
    
f = lambda s: f"dict({ ','.join( f'{k}={k}' for k in s.split(',') ) })"

kamusHasil = eval(f('kodedata,tanggalpendataan,bagiansubbagian,kodebarang,merekprinter'))


# print (kamusHasil)


environment = Environment(loader=FileSystemLoader("templates/"))

template = environment.get_template("message2.txt")


content = template.render(kamusHasil)

filename = f"file1.txt"


with open(filename, mode="w", encoding="utf-8") as message:
    
     message.write(content)
     
     print(f"... wrote{filename}")


koneksi.commit()

koneksi.close()


```

di bawah ini source code untuk templatenya:

```

{# templates/message2.txt #}


Kode Data : {{ kodedata }}

Tanggal Pendataan : {{ tanggalpendataan }}

Bagian / Sub Bagian : {{ bagiansubbagian }}

Kode Barang : {{ kodebarang }}


```

Di bawah ini source code yang menginspirasi untuk bisa mencetak hasil query nya ke halaman web

```python

from flask import Flask, render_template

aplikasi = Flask(__name__)


nilaiTertinggi = 100

@aplikasi.route('/')


# nilaiTertinggi = 100


def halamanUtama():

    nilaiKata = {
    
        "nilaiTertinggi": nilaiTertinggi,
    
    
    }
    
    return render_template("web4.html", **nilaiKata)
    
if __name__ == '__main__':

    aplikasi.run(host='0.0.0.0', port=8543, debug=True)


```

di bawah ini adalah template HTML nya:

``` html

<!DOCTYPE html>

<html>

	<head>
	
	
	
	
	
	</head>
	
	
	<body>
	
		<p>Nilai Tertinggi = {{ nilaiTertinggi }}</p>
	
	
	</body>



</html>



```


Contoh sederhana dari menghasil RESTFUL API, dengan menggunakan Flask dan MariaDB :

```python

# melanjutkan pembuatan program yg bisa menampilkan JSON di web

# datanya berasal dari MariaDB.

# kali ini menggabungkan dictionary seperti sebelumnya.

# dan mengatasi masalah data yang berupa tanggal ketika  di konversi ke JSON


import mariadb


import sys


import json


from flask import Flask, render_template


aplikasi = Flask(__name__)


@aplikasi.route('/kodebarang')


def dataPrinter():

    try:

        koneksi = mariadb.connect(user="steven",password="kucing",host="dk7ji791eetus5yei7h9m3emcfskjoqqh9u47jhgrdon9opk7gty.loki",port=3306,database="saham")
        
    except mariadb.Error as e:

        print(f"Gagal Terkoneksi Ke : {e}")
        
        sys.exit(1)
        
    # return "Berhasil Terkoneksi"
        
        
        
    class create_dict(dict):

        def __init__(self):
        
            self = dict()
            
            
        def add(self, key, value):
        
            self[key] = value
                
                
    mydict = create_dict()
            
    kursor = koneksi.cursor()
            
    kursor.execute("SELECT kodedata,tanggalpendataan FROM daftartintaprinter")
            
    hasil = kursor.fetchall()
    
    
    return hasil
            
            
            
    for row in hasil:
                        
        mydict.add(row[0],({"kodedata":row[0],"tanggalpendataan":row[1].strftime("%Y-%m-%d")}))
                    
        tampilkan_JSON = json.dumps(mydict, indent=2, sort_keys=True)
                    
                
        return tampilkan_JSON
            
            
    
            
if __name__ == '__main__':

    aplikasi.run(host="dk7ji791eetus5yei7h9m3emcfskjoqqh9u47jhgrdon9opk7gty.loki", port=5432, debug=True)


```

Selanjutnya kita akan belajar bagaimana mengkonsumsi data berformat JSON yang dihasilkan oleh Restful API. Untuk kemudian data tersebut ditampilkan ke dalam halaman web.


Sekarang kita mulai menuliskan membuat aplikasi web menggunakan Flask dan Blueprints.

Selanjutnya kita akan mengeksekusi file pythonnya langsung dalam bentuk image docker.



## Cara Untuk Menampilkan Hasi Fetchall dari Mariadb

```python

from jinja2 import Template, Environment, FileSystemLoader

from flask import Flask,render_template,request,json,jsonify

import mariadb

import sys


aplikasi = Flask(__name__)

@aplikasi.route('/')

def formUtama():

    return render_template("halaman1.html")
    
    
    
@aplikasi.route('/tampilData',methods=['POST'])

def tampilkanData():

    if request.method=='POST':
    
        koneksi = mariadb.connect(user="steven",password="kucing",host="1.1.3.11",port=3306,database="saham")
        
        kursor = koneksi.cursor(dictionary=True)
        
        kursor.execute("SELECT kodedata,tanggalpendataan,kodebarang FROM daftartintaprinter2")
        
        hasil = kursor.fetchall()
        
        print(hasil)   
        
        return render_template("halaman2.html", data1 = hasil)
        
        
        koneksi.commit()
        
        koneksi.close()
    
    
if __name__ == '__main__':

    aplikasi.run(host='0.0.0.0',port=8543,debug=True)




```

Ini source code yang berhasil untuk menampilkan data dari hasil kueri fetchall, yang berupa dictionary:

```python

from jinja2 import Template, Environment, FileSystemLoader

from flask import Flask,render_template,request,json,jsonify


import mariadb

import sys


aplikasi = Flask(__name__)

@aplikasi.route('/')

def formUtama():

    return render_template("halaman1.html")
    
    
@aplikasi.route('/tampilData',methods=['POST'])

def tampilkanData():

    if request.method=='POST':
    
        koneksi = mariadb.connect(user="steven",password="kucing",host="1.1.3.11",port=3306,database="saham")
        
        kursor = koneksi.cursor(dictionary=True)
        
        kursor.execute("SELECT kodedata,tanggalpendataan,kodebarang FROM daftartintaprinter2")
        
        hasil = kursor.fetchall()
        
        # print(hasil)
        
        # print (hasil[0])
        
        for row in hasil:
        
            print(row["kodedata"])
        
        
        return render_template("halaman2.html")
        
        koneksi.commit()
        
        koneksi.close()
        
        
        
if __name__=='__main__':

    aplikasi.run(host='0.0.0.0',port=8543,debug=True)

```

Ini source code yang bisa membaca hasil dictionary secara satu per satu : 

```python

from jinja2 import Template, Environment, FileSystemLoader

from flask import Flask,render_template,request,json,jsonify


import mariadb

import sys


aplikasi = Flask(__name__)

@aplikasi.route('/')

def formUtama():

    return render_template("halaman1.html")
    
    
@aplikasi.route('/tampilData',methods=['POST'])

def tampilkanData():

    if request.method=='POST':
    
        koneksi = mariadb.connect(user="steven",password="kucing",host="1.1.3.11",port=3306,database="saham")
        
        kursor = koneksi.cursor(dictionary=True)
        
        kursor.execute("SELECT kodedata,tanggalpendataan,kodebarang FROM daftartintaprinter2")
        
        hasil = kursor.fetchall()
        
        print(hasil[3]["kodebarang"])
        
        
        koneksi.commit()
        
        koneksi.close()
        
        
        return render_template("halaman2.html")
        
        
if __name__=='__main__':

    aplikasi.run(host='0.0.0.0',port=8543,debug=True)
    

```


### Source Code Untuk Menampilkan Data hasil Fetchall dari Mariadb ke halaman web, dengan data bisa dipilih per baris dan per item.

Source code untuk python yang melakukan query ke Maridb dan melakukan pemilahan data untuk ditampilkan ke halaman web:

```python

from jinja2 import Template, Environment, FileSystemLoader

from flask import Flask, render_template, request, json, jsonify


import mariadb

import sys


aplikasi = Flask(__name__)

@aplikasi.route('/')

def formUtama():

    return render_template("halaman1.html")

@aplikasi.route('/tampilData',methods=['POST'])

def tampilkanData():

    if request.method=='POST':
    
        koneksi = mariadb.connect(user="steven",password="kucing",host="1.1.3.9",port=3306,database="saham")
        
        kursor= koneksi.cursor(dictionary=True)
        
        kursor.execute("SELECT kodedata,tanggalpendataan,kodebarang,NIP,namabarang,kodebagian,namadivisi,merekprinter,serialprinter,macaddress,jenistinta,namapengguna FROM daftartintaprinter2")
        
        hasil = kursor.fetchall()
        
        # di bawah ini untuk menampilkan baris data yang urutan 1
        
        kodedata1 = hasil[0]["kodedata"]
        
        tanggalpendataan1 = hasil[0]["tanggalpendataan"]
        
        kodebarang1 = hasil[0]["kodebarang"]
        
        nip1 = hasil[0]["NIP"]
        
        namabarang1 = hasil[0]["namabarang"]
        
        kodebagian1 = hasil[0]["kodebagian"]
        
        namadivisi1 = hasil[0]["namadivisi"]
        
        merekprinter1 = hasil[0]["merekprinter"]
        
        serialprinter1 = hasil[0]["serialprinter"]
        
        macaddress1 = hasil[0]["macaddress"]
        
        jenistinta1 = hasil[0]["jenistinta"]
        
        namapengguna1 = hasil[0]["namapengguna"]
        
        
        # di bawah ini untuk menampilkan baris data yang urutan 2
        
        kodedata2 = hasil[1]["kodedata"]
        
        tanggalpendataan2 = hasil[1]["tanggalpendataan"]
        
        kodebarang2 = hasil[1]["kodebarang"]
        
        nip2 = hasil[1]["NIP"]
        
        namabarang2 = hasil[1]["namabarang"]
        
        kodebagian2 = hasil[1]["kodebagian"]
        
        namadivisi2 = hasil[1]["namadivisi"]
        
        merekprinter2 = hasil[1]["merekprinter"]
        
        serialprinter2 = hasil[1]["serialprinter"]
        
        macaddress2 = hasil[1]["macaddress"]
        
        jenistinta2 = hasil[1]["jenistinta"]
        
        namapengguna2 = hasil[1]["namapengguna"]
        
        
        
        
        
        koneksi.commit()
        
        koneksi.close()
        
        
        return render_template("halaman3.html", kodeData1 = kodedata1, tanggalPendataan1 = tanggalpendataan1, kodeBarang1 = kodebarang1, NIP1 = nip1, namaBarang1 = namabarang1, kodeBagian1 = kodebagian1, namaDivisi1 = namadivisi1, merekPrinter1 = merekprinter1, serialPrinter1 = serialprinter1, macAddress1 = macaddress1, jenisTinta1 = jenistinta1, namaPengguna1 = namapengguna1, kodeData2 = kodedata2, tanggalPendataan2 = tanggalpendataan2, kodeBarang2 = kodebarang2, NIP2 = nip2, namaBarang2 = namabarang2, kodeBagian2 = kodebagian2, namaDivisi2 = namadivisi2, merekPrinter2 = merekprinter2, serialPrinter2 = serialprinter2, macAddress2 = macaddress2, jenisTinta2 = jenistinta2, namaPengguna2 = namapengguna2 )

if __name__ == '__main__':

    aplikasi.run(host='0.0.0.0',port=8543,debug=True)




```

Di bawah ini source code untuk halaman tombol yang berguna memicu source code di atas:

```html


<!DOCTYPE html>

<html>

	<head>
	
	
	
	
	
	
	
	</head>
	
	<body>
	
	
		<form action="http://1.1.3.9:8543/tampilData" method="POST" id="formPrinter" name="formPrinter">
		
			<input type="submit" Value="Tampilkan Data Dari Mariadb">
		
		</form>
	
	</body>

```

Ini source code halaman HTML untuk menampilkan semua data yang di dapat dari Mariadb secara satu per satu:

```html

<!DOCTYPE html>

<html>

	<head>
	
	
	
	
	</head>
	
	
	
	<body>
	
		<p>Kode Data 1 : {{ kodeData1 }} </p>
		
		<p>Tanggal Pendataan 1 : {{ tanggalPendataan1 }} </p>
		
		<p>Kode Barang 1 : {{ kodeBarang1 }} </p>
		
		<p>NIP 1 : {{ NIP1 }}</p>
		
		<p>Nama Barang 1 : {{ namaBarang1 }}</p>
		
		<p>Kode Bagian 1 : {{ kodeBagian1 }}</p>
		
		<p>Nama Divisi 1 : {{namaDivisi1}}</p>
		
		<p>Merek Printer 1 : {{merekPrinter1}}</p>
		
		<p>Serial Printer 1 : {{serialPrinter1}}</p>
		
		<p>Mac Address 1 : {{macAddress1}}</p>
		
		<p>Jenis Tinta 1 : {{jenisTinta1}}</p>
		
		<p>Nama Pengguna 1 : {{namaPengguna1}}</p>
		
		<br>
		
		<br>
		
		
		<h3>Di Bawah ini adalah data untuk item tinta baris 2</h3>
		
		<br>
		
		<br>
		
		<p>Kode Data 2 : {{ kodeData2 }}</p>
		
		<p>Tanggal Pendataan 2 : {{ tanggalPendataan2 }} </p>
		
		<p>Kode Barang 2 : {{ kodeBarang2 }} </p>
		
		<p>NIP 2 : {{ NIP2 }}</p>
		
		<p>Nama Barang 2 : {{ namaBarang2 }}</p>
		
		<p>Kode Bagian 2 : {{ kodeBagian2 }}</p>
		
		<p>Nama Divisi 2 : {{namaDivisi2}}</p>
		
		<p>Merek Printer 2 : {{merekPrinter2}}</p>
		
		<p>Serial Printer 2 : {{serialPrinter2}}</p>
		
		<p>Mac Address 2 : {{macAddress2}}</p>
		
		<p>Jenis Tinta 2 : {{jenisTinta2}}</p>
		
		<p>Nama Pengguna 2 : {{namaPengguna2}}</p>
		
		
		
	
	</body>


</html>




```






