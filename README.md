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
