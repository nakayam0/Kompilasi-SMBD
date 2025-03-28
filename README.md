#  Kompilasi Laporan Praktikum Sistem Manajemen Basis Data (SMBD)
- Penulis : Arginda Mechika S.
- Mata Kuliah : Sistem Manajemen Basis Data
- Institusi : PSDKU Politeknik Negeri Malang di Kota Kediri

## Pendahuluan 
Di era digital, manajemen basis data sangat penting untuk pengelolaan informasi yang efisien. MySQL, sebagai RDBMS populer, menjadi solusi andal untuk menyimpan dan mengelola data. Laporan ini merangkum praktikum instalasi MySQL, manajemen user & role, optimasi query dengan indexing, identifikasi bottleneck, dan partisi tabel. Tujuannya adalah memahami pengelolaan database secara optimal untuk kinerja yang lebih cepat dan efisien dalam menangani data kompleks.

## 1.  Instalasi dan Konfigurasi MySQL
## Latar Belakang 
MySQL: Solusi Database untuk Semua Skala
MySQL merupakan sistem manajemen basis data relasional (RDBMS) yang powerful dan fleksibel, cocok untuk berbagai kebutuhan mulai dari aplikasi kecil hingga sistem enterprise kelas besar. Performa optimal MySQL dapat dicapai melalui:
- Instalasi yang tepat - Memastikan komponen terpasang dengan benar sesuai kebutuhan sistem
- Konfigurasi optimal - Penyesuaian parameter server untuk mendapatkan kinerja terbaik
- Pengaturan keamanan - Implementasi proteksi data yang robust sejak awal instalasi
Dengan penerapan yang benar, MySQL mampu memberikan stabilitas tinggi, kecepatan akses data, dan keamanan yang terjamin untuk mendukung operasional bisnis kritis.

## Problem yang Dihadapi
- Instalasi default memiliki parameter yang kurang optimal.
- Perubahan port MySQL dari 3306 ke 3309.
- Menyesuaikan innodb_buffer_pool_size agar lebih efisien.
- Mengubah password root untuk meningkatkan keamanan.

## Solusi dan Pengimplementasian
### Instalasi MySQL
1. Download MySQL Installer dari situs resmi.
2. Jalankan Installer dan ikuti proses instalasi.
3. Pilih Server dan Workbench untuk instalasi.
4. Konfigurasi Root Password dan simpan dengan aman.
5. Selesaikan Instalasi dan Tes Koneksi dengan MySQL Workbench atau Command Line.
### Konfigurasi Awal
#### 🔧 Mengubah Port MySQL
1. Buka file konfigurasi `my.ini` atau `my.cnf`
2. Tambahkan/modifikasi bagian `[mysqld]`:
   ```ini
   [mysqld]
   port=3309  # Port baru yang diinginkan
   ```
3. Restart service MySQL
#### 🧠 Mengatur Buffer Pool Size
Untuk server dengan RAM 8GB
  ```ini
  innodb_buffer_pool_size = 2G
```

#### 🔐 Mengubah Password Root MySQL

Untuk mengubah password user root di MySQL/MariaDB, jalankan perintah berikut:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password-baru-anda';
```
## Kesimpulan 
Konfigurasi parameter MySQL secara cermat dapat,
- Meningkatkan kecepatan eksekusi query
- Memaksimalkan utilisasi resource
- Memperkuat proteksi data
- Terutama kritikal saat diimplementasikan pada server produksi.

## 2. Manajemen User, Role, dan Privilege di MySQL
## Latar Belakang 
Manajemen user dinilai sangat penting, dikarenakan implementasi user management yang tepat memungkinkan autentikasi yang terpusat,autorisasi berbasis role (RBAC), audit trail akses data, dan pencegahan akses tidak sah (unauthorized access
## Problem yang Dihadapi
- Membuat user dengan akses terbatas.
- Memberikan role tertentu agar lebih mudah dalam manajemen hak akses.
- Menghapus atau mencabut akses user yang tidak diperlukan.
## Solusi dan Pengimplementasian
### 1. Membuat User Baru
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
### 2. Membuat Role
```
CREATE ROLE 'role_read_only';
GRANT SELECT ON database_name.* TO 'role_read_only';
```
### 3. Memberikan Role ke User
```
GRANT 'role_read_only' TO 'user1'@'localhost';
```
### 4. Menghapus User
```
DROP USER 'user1'@'localhost';
```
## Kesimpulan 
Implementasi role-based access control (RBAC) pada RDBMS menyediakan mekanisme terstruktur untuk manajemen privilege yang scalable, memungkinkan administrasi terpusat untuk ratusan user dengan kebutuhan akses berbeda.

## 3.  Optimasi Bottleneck di MySQL
## Latar Belakang 
Bottleneck pada MySQL dapat mengakibatkan : Throughput query menurun hingga 40-60%, latency meningkat eksponensial (bukan linear), resource utilization tidak optimal, identifikasi bottleneck melalui metric spesifik menjadi kunci optimasi.
## Problem yang dihadapi
- Full Table Scan karena tidak ada indeks.
- Banyak koneksi terbuka menyebabkan error Too many connections.
- Deadlock dalam transaksi.
- Ukuran InnoDB Buffer Pool yang kecil.
## Solusi dan Pengimplementasian
### Menambahkan Indeks
```
CREATE INDEX idx_customer_id ON orders(customer_id);
```
###  Menggunakan Query Cache (jika didukung oleh versi MySQL)
```
SET GLOBAL query_cache_size = 64M;
```
### Meningkatkan Buffer Pool Size
```
innodb_buffer_pool_size = 5G;
```
## Kesimpulan
Resolusi bottleneck yang tepat dapat menghasilkan:
- Peningkatan throughput query hingga 300-500%
- Penurunan latency eksekusi dari 2s menjadi 200ms
- Utilisasi CPU yang lebih efisien (dari 90% ke 60%)
- Pengurangan I/O wait time sebesar 70-80%"

## 4. Optimasi Database dengan Partisi Tabel
## Latar Belakang 
Partisi tabel membagi data menjadi bagian-bagian terpisah secara fisik berdasarkan kriteria tertentu, sehingga mempercepat query dengan membatasi pencarian hanya pada partisi yang relevan.

Keuntungan utama:
- Meningkatkan kecepatan query (partition pruning)
- Memudahkan manajemen data besar
- Mengoptimalkan I/O operations

