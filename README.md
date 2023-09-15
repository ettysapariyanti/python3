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





