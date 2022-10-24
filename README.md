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


Source code SQLAlchemy yang berhasil dipakai untuk mengetest koneksi ke server MariaDB :

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







# SQLAlchemy




