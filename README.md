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
C:\Users\owi>memurai-cli # masuk ke redis cli via memurai
127.0.0.1:6379> ping # tes koneksi
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
memurai.exe "C:/Users/owi/pzn-belajar-redis-dasar/config/redis.conf" # jalanin redis server sesuai dengan isi file config, di dalam file config bisa settin beberapa db
```

- untuk linux/mac
```bash
redis-server config/redis.conf
```

# Database
## Database
- Redis memiliki konsep database seperti pada relational database mysql atau postgree
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
127.0.0.1:6379> select 0 # pilih database pertama
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
127.0.0.1:6379[19]> set test "lightspeed" # set key value
OK
127.0.0.1:6379[19]> get test # melihat value dari key
"lightspeed"
127.0.0.1:6379[19]> exists key
(integer) 0
127.0.0.1:6379[19]> exists test # cek apakah key mempunyai value
(integer) 1
127.0.0.1:6379[19]> keys test* # scan/cek seluruh keys dengan prefix test
1) "test"
127.0.0.1:6379[19]> append tes "dudung" # menambah data value ke key tes
(integer) 6
127.0.0.1:6379[19]> append test "dudung" # menambah data value ke key test
(integer) 16
127.0.0.1:6379[19]> get test # melihat value dari key
"lightspeeddudung"
127.0.0.1:6379[19]> append test " cungpret" # menambah data value ke key test
(integer) 25
127.0.0.1:6379[19]> get test # melihat value dari key
"lightspeeddudung cungpret"
127.0.0.1:6379[19]> keys tes* # scan/cek seluruh keys dengan prefix tes
1) "tes"
2) "test"
127.0.0.1:6379[19]> del tes # hapus key
(integer) 1
127.0.0.1:6379[19]> keys tes* # scan/cek seluruh keys dengan prefix tes
1) "test"
127.0.0.1:6379[19]> keys * # scan/cek seluruh keys
127.0.0.1:6379[19]> 
```

## Operasi Range Data String

| Operasi | Keterangan |
|------|------|
| setrange key offset value  | mengubah value dari offset yang ditentukan   |
| getrange key start end  | mengambil value dari range yang ditentukan   |


```bash
127.0.0.1:6379[19]> setrange kosongdua 4 "dudung" # menambah value dari key index ke 4
(integer) 12
127.0.0.1:6379[19]> get kosongdua
"dudududungon"
127.0.0.1:6379[19]> setrange kosongdua 0 "dudung nipon" # menambah value dari key index ke 0
(integer) 12
127.0.0.1:6379[19]> get kosongdua
"dudung nipon"
127.0.0.1:6379[19]> getrange kosongdua 4 9 # melihat value dari key index ke 4-9
"ng nip"
127.0.0.1:6379[19]> 
```


## Operasi Multiple Data String

| Operasi | Keterangan |
|------|------|
| mget key [key ...]  | Get the values of all the given keys   |
| mset key value [key value ...]  | Set multiple keys to multiple values   |

```bash
127.0.0.1:6379[19]> mget test kosongdua # cek isi value lebih dari 1 key
1) "dadah"
2) "jokowi"
127.0.0.1:6379[19]> mset dudung "100" dodong "200" # set lebih dari 1 key value
OK
127.0.0.1:6379[19]> keys * # scan/cek seluruh keys
1) "dudung"
2) "kosongdua"
3) "[kosongdua"
4) "dodong"
5) "tes"
6) "test"
127.0.0.1:6379[19]> mget dudung dodong test kosongdua # cek isi value lebih dari 1 key
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

```bash
127.0.0.1:6379[19]> expire rio 10 # set waktu expired key selama 10 detik
(integer) 1
127.0.0.1:6379[19]> ttl rio # cek waktu expired yang tersisa
(integer) 3
127.0.0.1:6379[19]> ttl rio
(integer) 1
127.0.0.1:6379[19]> ttl rio
(integer) 0
127.0.0.1:6379[19]> setex rio 10 "parung" # set waktu expired value dari key selama 10 detik
OK
127.0.0.1:6379[19]> ttl rio # cek waktu expired yang tersisa
(integer) 8
127.0.0.1:6379[19]> ttl rio
(integer) 6
127.0.0.1:6379[19]> ttl rio
(integer) 5
127.0.0.1:6379[19]> ttl rio
(integer) 5
127.0.0.1:6379[19]> ttl rio
(integer) 4
127.0.0.1:6379[19]> ttl rio
(integer) 4
127.0.0.1:6379[19]> ttl rio
(integer) 3
127.0.0.1:6379[19]> ttl rio
(integer) 3
127.0.0.1:6379[19]> ttl rio
(integer) 3
127.0.0.1:6379[19]> ttl rio
(integer) 2
127.0.0.1:6379[19]> ttl rio
(integer) 2
127.0.0.1:6379[19]> ttl rio
(integer) 2
127.0.0.1:6379[19]> ttl rio
(integer) 1
127.0.0.1:6379[19]> ttl rio
(integer) 1
127.0.0.1:6379[19]> ttl rio
(integer) 1
127.0.0.1:6379[19]> ttl rio
(integer) 1
127.0.0.1:6379[19]> get rio
(nil)
```

* note : set expire di perlukan agar memory/RAM tidak di bebani oleh data data yang di perlukan


# Increment & Decrement
## Increment & Decrement

- Operasi Increment & Decrement sekilas sangat mudah dilakukan, hanya tinggal mengupdate data yang di redis dengan data baru (data lama ditambah 1)
- Namun jika operasi dilakukan secara paralel dan dalam waktu yang sangat cepat, hal ini bisa memungkinkan race condition
- Untungnya redis memiliki operasi untuk melakukan increment dan decrement

## Race Condition
```js
// contoh race condition
var value = await redis.get("key"); // key value 10
value = Number(value) + 1;
await redis.set("key", value);
// jadi saat ada dua orang increment di detik yang sama code tersebut menyebabkan race condition, seharusnya per orang menyimpan value sebelas alhasil kedua orang tersebut menyimpan value 11 yang sama
```

## Operasi Increment & Decrement

| Operasi | Keterangan |
|------|------|
| incr key  | Increment the integer value of a key by one   |
| decr key seconds value  | Decrement the integer value of a key by one   |
| incrby key increment  | Increment the integer value of a key by the given amount   |
| decrby key decrement  | Decrement the integer value of a key by the given number   |

* note : isi key nya harus berupa angka/int jangan teks/string

```bash
127.0.0.1:6379[19]> incr counter # tambah/increment key value integer
(integer) 1
127.0.0.1:6379[19]> incr counter
(integer) 2
127.0.0.1:6379[19]> incr counter
(integer) 3
127.0.0.1:6379[19]> get counter # melihat value dari key
"3"
127.0.0.1:6379[19]> decr counter # mengurangi/decrement value dari key
(integer) 2
127.0.0.1:6379[19]> decr counter
(integer) 1
127.0.0.1:6379[19]> decr counter
(integer) 0
127.0.0.1:6379[19]> incrby counter 5 # menambah/increment secara bulk value dari key
(integer) 5
127.0.0.1:6379[19]> get counter # melihat isi value dari key
"5"
127.0.0.1:6379[19]> decrby counter 5 # mengurangi/decrement secara bulk value dari key
(integer) 0
127.0.0.1:6379[19]> 
```

# Flush
## Flush

- Kadang kita butuh mengosongkan seluruh data di redis, misal ketika terjadi kesalahan kode sehingga menyebabkan data di redis salah
- Menghapus data di redis satu-satu menggunakan operasi delete bukanlah hal yang bijak
- Redis memiliki fitur untuk menghapus seluruh data di database redis, yaitu operasi flush

## Operasi Flush

| Operasi | Keterangan |
|------|------|
| flushdb  | Remove all keys from the current database  |
| flushall  | Remove all keys from all databases  |

* note : konsep kalo di laravel php artisan migrate:fresh/php artisan optimize:clear

```bash
127.0.0.1:6379[19]> flushdb # menghapus semua key dari db yang di gunakan
OK
127.0.0.1:6379[19]> get rio
(nil)
127.0.0.1:6379[19]> keys *
(empty array)
127.0.0.1:6379[19]> set cungpret "halomo"
OK
127.0.0.1:6379[19]> keys *
1) "cungpret"
127.0.0.1:6379[19]> set papuy "ororo"
OK
127.0.0.1:6379[19]> keys *
1) "papuy"
2) "cungpret" 
127.0.0.1:6379[19]> flushall # menghapus semua key dari semua db yang tersedia
OK
127.0.0.1:6379[19]> keys *
(empty array)
127.0.0.1:6379[19]> 
```

# Pipeline
## Pipeline
- Perintah yang dikirim dari client ke server redis menggunakan Request/Response protocol
- Artinya tiap request yang dikirim ke server redis, maka redis akan membalasnya secara langsung
- Kadang ada kebutuhan kita mengirim data ke redis dalam jumlah besar, misal ketika ada kasus memindahkan data dari database mysql ke redis
- Jika kita mengirim satu per satu datanya, maka akan butuh waktu lama untuk selesai
- Redis mendukung operasi bulk via pipeline, dimana kita bisa mengirim beberapa perintah sekaligus dalam satu request
- Namun perlu diketahui, server redis tidak akan membalas tiap perintah yang dikirim via pipeline

## Operasi Pipeline Menggunakan Redis Cli
- linux
```bash
redis-cli -h host -p port -n database --pipe < input-file
redis-cli -h localhost -p 6379 -n 1 --pipe < input-file.txt
```

- powershell
```bash
Get-Content .\input-file.txt | redis-cli -h localhost -p 6379 -n 0 --pipe
Get-Content .\input-file.txt | memurai-cli -h localhost -p 6379 -n 0 --pipe # jika pakai memurai
```

| Bagian | Fungsi |
|--------|--------|
| `Get-Content` | Membaca isi file |
| `\|` | Mengirim output ke command berikutnya |
| `memurai-cli` | Redis Client |
| `-h localhost` | Host Redis |
| `-p 6379` | Port Redis |
| `-n 0` | Menggunakan Database 0 |
| `--pipe` | Mengirim seluruh command secara streaming (pipeline) ke Redis |

- response 
```bash
PS C:\Users\owi\pzn-belajar-redis-dasar> Get-Content .\input-file.txt | memurai-cli -h localhost -p 6379 -n 0 --pipe
All data transferred. Waiting for the last reply...
Last reply received from server.
errors: 0, replies: 4
```

## Result
```bash
PS C:\Users\owi\pzn-belajar-redis-dasar> memurai-cli                                                       
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> keys *
1) "rui"
2) "gibran"
3) "rio"
4) "jkw"
127.0.0.1:6379> get rio
"Rio Achyar"
127.0.0.1:6379> get jkw
"Joko Widodo"
127.0.0.1:6379> 
```

# Transaction
- Seperti pada database relational, redis juga mendukung transaction
- Proses transaction adalah proses dimana kita mengirimkan beberapa perintah, dan perintah tersebut akan dianggap sukses jika semua perintah sukses, jika gagal maka semua perintah harus dibatalkan

## Operasi Transaction


| Operasi | Keterangan |
|------|------|
| multi  | Mark the start of a transaction block   |
| exec  | Execute all commands issued after MULTI   |
| discard  | Discard all commands issued after MULTI   |


- example 
```bash
127.0.0.1:6379> multi # memulai transaction
OK
127.0.0.1:6379(TX)> set rm "ronaldo" # set key & value
QUEUED
127.0.0.1:6379(TX)> set brc "neymar" # set key & value
QUEUED
127.0.0.1:6379(TX)> set united "rooney" # set key & value
QUEUED
127.0.0.1:6379(TX)> exec # commit transaction
1) OK # success resspone
2) OK
3) OK
127.0.0.1:6379> keys * # scan/cek semua keys
1) "brc"
2) "rm"
3) "united"
127.0.0.1:6379> get brc # cek isi key
"neymar"
127.0.0.1:6379> multi # memulai transaction
OK
127.0.0.1:6379(TX)> set c1 "komeng" # set key & value
QUEUED
127.0.0.1:6379(TX)> set c2 "adul" # set key & value
QUEUED
127.0.0.1:6379(TX)> set c3 "bedugul" # set key & value
QUEUED
127.0.0.1:6379(TX)> discard # rollback/batalkan transaction
OK
127.0.0.1:6379> keys *
1) "brc"
2) "rm"
3) "united"
127.0.0.1:6379> 
```

* note : semua command yang sudah di dahului command multi akan masuk ke antrean


# Monitor
## Monitor

- Kadang ada kasus kita ingin mendebug aplikasi saat berkomunikasi dengan redis
- Redis memiliki fitur monitor, yaitu fitur untuk memonitor semua request yang masuk ke redis server
- Dengan fitur ini kita bisa mudah mendebug jika ternyata ada perintah yang salah yang dikirim oleh aplikasi kita ke redis server

## Operasi Monitor

| Operasi | Keterangan |
|------|------|
| monitor  | Listen for all requests received by the server in real time  |

- example

### Client 1
```bash
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> get rio
(nil)
127.0.0.1:6379> get rm
"ronaldo"
127.0.0.1:6379> mget rm brc united
1) "ronaldo"
2) "neymar"
3) "rooney"
127.0.0.1:6379> 
```

### Client 2 (MONITOR)
```bash
PS C:\Users\owi\pzn-belajar-redis-dasar> memurai-cli 
127.0.0.1:6379> monitor
OK
1786606642.250807 [0 127.0.0.1:49960] "ping"
1786606656.245558 [0 127.0.0.1:49960] "get" "rio"
1786606661.499804 [0 127.0.0.1:49960] "get" "rm"
1786606743.274778 [0 127.0.0.1:49960] "mget" "rm" "brc" "united"
```


# Server Information
- Kadang kita butuh mendapatkan informasi dan statistik redis server
- Seperti jumlah memory yang sudah terpakai, konfigurasi dan lain-lain
- Redis memiliki fitur ini, sehingga kita sangat mudah untuk mendapat informasi server dan memonitor nya


## Operasi Server Information

| Operasi | Keterangan |
|------|------|
| info  | Get information and statistics about the server  |
| config get <key>  | Get the value of a configuration parameter from redis.conf  |

# Client Connection

- Redis menyimpan semua informasi client di server
- Hal ini memudahkan kita untuk melihat daftar client, dan juga mengecek jika ada anomali, seperti terlalu banyak koneksi client ke redis

| Operasi | Keterangan |
|------|------|
| info  | Get information and statistics about the server  |
| config get <key>  | Get the value of a configuration parameter from redis.conf  |