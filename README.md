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
