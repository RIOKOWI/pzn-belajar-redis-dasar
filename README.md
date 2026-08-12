# Pengenalan Redis

## Sejarah Redis
- Redis singkatan dari Remote Dictionary Server adalah sistem basis data key-value berbasis memory
- Pertama kali rilis tahun 2009 sebagai project open source
- `https://redis.io/`
 

## Apa Itu Key-Value Database?
- Redis adalah sistem basis data berbasis key-value
- Paradigma key-value adalah paradigma dimana data disimpan dalam bentuk pair (key-value)
- Key mirip dengan primary key dari data, sedangkan value adalah isi dari datanya


contoh :

| Key | Value |
|------|------|
| K1  | AAA,BBB,CCC   |
| K2  | 1,2,3,4,5   |
| K3  | DDD,AAA   |
| K4  | AAA,2,01/01/2022   |
| K5  | 3,344,5555   |

* note : hanya bisa mengambil data dari key nya bukan dari value nya

## Apa Itu In-Memory Database?
- Redis menyimpan datanya di memory, namun kita bisa memintanya untuk menyimpan datanya secara regular permanen di disk. 
- Data di disk hanya dijadikan backup ketika redis berjalan ulang, selama redis berjalan, redis hanya akan melakukan manipulasi data ke memory

contoh :

[IN MEMORY DATABASE](Arch/IN-MEMORY-DB.excalidraw.png)
<br>
[KENAPA PILIH REDIS DARIPADA YANG LAIN ?](https://db-engines.com/en/ranking/key-value+store)


# Kapan Butuh Redis?

## Kapan Butuh Redis?
- Saat kita membuat aplikasi, tidak langsung wajib menggunakan Redis
- Redis menggunakan memory sebagai media penyimpanan utama, otomatis harga memory lebih mahal dibandingkan disk
- Untuk menggunakan Redis, kita perlu lihat kasusnya secara detail

contoh :

[SLOW DB](Arch/SLOW-DB.excalidraw.png)
<br>
[SLOW APPS](Arch/SLOW-APPS.excalidraw.png)
<br>
[HEAVY REQUEST](Arch/HEAVY-REQUEST.excalidraw.png)
<br>
[DELAYED JOB](Arch/DELAYED-JOB.excalidraw.png)

## Dan masih banyak lainnya
- Rata-rata redis digunakan untuk mempercepat aplikasi yang lambat
- Dan juga redis biasa digunakan untuk caching, menyimpan data secara sementara

# How to Run Redis in memurai

```bash
C:\Users\owi>memurai-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

# Configuration

## Configuration File

- Saat menjalankan redis, redis tidak butuh file konfigurasi
- Namun jika tidak menggunakan file konfigurasi, redis akan berjalan menggunakan konfigurasi default
- Ada baiknya kita membuat file konfigurasi agar pengaturannya bisa diubah
- `https://github.com/redis/redis/blob/7.0/redis.conf`

## Menjalankan redis server sesuai file config
- untuk windows
```bash
memurai.exe "C:/Users/owi/pzn-belajar-redis-dasar/config/redis.conf"
```

- untuk linux/mac
```bash
redis-server config/redis.conf
```

# Database
## Database
- Redis memiliki konsep database seperti pada relational database mysql atau postgre
- Di redis kita bisa membuat database dan menggunakan database nya
- Namun sedikit berbeda, jika di relational database kita bisa membuat database dengan menggunakan nama database, di redis kita hanya bisa menggunakan angka sebagai database
- Secara default database di redis adalah 0 (nol)
- Kita bisa menggunakan database sejumlah maksimal sesuai dengan konfigurasi yang kita gunakan di file konfigurasi

* note : cari aja variable databases di file `config\redis.conf` kalau ingin di ubah ubah, setelah di ubah server harus di stop lalu start ulang

## Operasi Database

| Operasi Database | Keterangan |
|------|------|
| select database  | memilih atau pindah database   |

contoh :

```bash
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> select 2
OK
127.0.0.1:6379[2]> select 3
OK
127.0.0.1:6379[3]> select 4
OK
```

# Strings
## Struktur Data Redis
- Redis sebenarnya mendukung struktur data yang banyak, seperti String, List, Set, dan lain-lain
- Namun yang paling sering digunakan adalah struktur data String
- Struktur data lainnya akan kita bahas di kelas terpisah, yaitu kelas Redis Data Structure

## Operasi Data String

| Operasi | Keterangan |
|------|------|
| set key value  | mengubah string value dari key   |
| get key  | mendapatkan value menggunakan key   |
| exists key  | mengecek apakah key memiliki value   |
| del key [key ...]  | menghapus menggunakan key   |
| append key value  | menambah data value ke key   |
| keys pattern  | mencari key menggunakan patterns   | 

* note: keys pattern tidak di rekomendasikan karna bisa lambat kalau di suruh scan semua data

```bash
127.0.0.1:6379> select 19
OK
127.0.0.1:6379[19]> set test "lightspeed"
OK
127.0.0.1:6379[19]> get test
"lightspeed"
127.0.0.1:6379[19]> exists key
(integer) 0
127.0.0.1:6379[19]> exists test
(integer) 1
127.0.0.1:6379[19]> keys test*
1) "test"
127.0.0.1:6379[19]> append tes "dudung"
(integer) 6
127.0.0.1:6379[19]> append test "dudung"
(integer) 16
127.0.0.1:6379[19]> get test
"lightspeeddudung"
127.0.0.1:6379[19]> append test " cungpret"
(integer) 25
127.0.0.1:6379[19]> get test
"lightspeeddudung cungpret"
127.0.0.1:6379[19]> keys pattern tes*
(error) ERR wrong number of arguments for 'keys' command
127.0.0.1:6379[19]> keys tes*
1) "tes"
2) "test"
127.0.0.1:6379[19]> del tes
(integer) 1
127.0.0.1:6379[19]> keys tes*
1) "test"
127.0.0.1:6379[19]> keys *
1) "test"
127.0.0.1:6379[19]> 
```

## Operasi Range Data String

| Operasi | Keterangan |
|------|------|
| setrange key offset value  | mengubah value dari offset yang ditentukan   |
| getrange key start end  | mengambil value dari range yang ditentukan   |


```bash
127.0.0.1:6379[19]> setrange kosongdua 4 "dudung"
(integer) 12
127.0.0.1:6379[19]> get kosongdua
"dudududungon"
127.0.0.1:6379[19]> setrange kosongdua 0 "dudung nipon"
(integer) 12
127.0.0.1:6379[19]> get kosongdua
"dudung nipon"
127.0.0.1:6379[19]> getrange kosongdua 4 9
"ng nip"
127.0.0.1:6379[19]> 
```


## Operasi Multiple Data String

| Operasi | Keterangan |
|------|------|
| mget key [key ...]  | Get the values of all the given keys   |
| mset key value [key value ...]  | Set multiple keys to multiple values   |

```bash
127.0.0.1:6379[19]> mget test kosongdua
1) "dadah"
2) "jokowi"
127.0.0.1:6379[19]> mset dudung "100" dodong "200"
OK
127.0.0.1:6379[19]> keys *
1) "dudung"
2) "kosongdua"
3) "[kosongdua"
4) "dodong"
5) "tes"
6) "test"
127.0.0.1:6379[19]> mget dudung dodong test kosongdua
1) "100"
2) "200"
3) "dadah"
4) "jokowi"
127.0.0.1:6379[19]> 
```

# Expiration
## Expiration

- Secara default saat kita menyimpan data ke redis, redis akan menyimpannya secara permanen sampai kita menghapusnya
- Kadang kita mendapatkan kasus ingin menghapus data di redis secara otomatis dalam waktu tertentu
- Misal kita menyimpan data cache di redis selama 10 menit, setelah 10 menit kita akan query ulang ke database untuk mendapatkan data terbaru
- Hal ini bisa dilakukan di redis, redis memiliki fitur expiration secara otomatis pada data yang kita simpan di redis

## Operasi Expiration Data String

| Operasi | Keterangan |
|------|------|
| expire key seconds  | Set a key's time to live in seconds   |
| setex key seconds value  | Set the value and expiration of a key   |
| ttl key  | Get the time to live for a key   |