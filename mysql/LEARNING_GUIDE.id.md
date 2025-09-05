# Panduan Belajar MySQL

Panduan komprehensif untuk perintah dasar dan menengah MySQL.

## 1. Terhubung ke MySQL

Pertama, akses *container* yang sedang berjalan untuk menggunakan klien baris perintah MySQL.

```bash
# Dapatkan nama atau ID container
docker ps

# Akses container (asumsi nama container adalah mysql_db)
docker exec -it mysql_db bash
```

Setelah di dalam *container*, hubungkan ke MySQL:

```bash
# Terhubung menggunakan user root
mysql -u root -p
# Anda akan diminta memasukkan password yang Anda atur di file .env
```

## 2. Operasi Database

- **Tampilkan semua database:**
  ```sql
  SHOW DATABASES;
  ```

- **Buat database baru:**
  ```sql
  CREATE DATABASE my_new_app;
  ```

- **Pilih database untuk digunakan:**
  ```sql
  USE my_new_app;
  ```

- **Hapus database:**
  ```sql
  DROP DATABASE my_new_app;
  ```

## 3. Operasi Tabel

- **Buat tabel baru:**
  *Tipe data seperti `INT`, `VARCHAR(255)`, `TEXT`, `DATE`, `TIMESTAMP` adalah umum.*
  ```sql
  CREATE TABLE users (
      id INT AUTO_INCREMENT PRIMARY KEY,
      username VARCHAR(50) NOT NULL UNIQUE,
      email VARCHAR(100) NOT NULL,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- **Tampilkan semua tabel di database saat ini:**
  ```sql
  SHOW TABLES;
  ```

- **Jelaskan struktur tabel:**
  ```sql
  DESCRIBE users;
  -- atau
  EXPLAIN users;
  ```

- **Ubah tabel (tambah, ubah, hapus kolom):**
  ```sql
  -- Tambah kolom baru
  ALTER TABLE users ADD COLUMN last_login DATE;

  -- Ubah tipe data kolom
  ALTER TABLE users MODIFY COLUMN email VARCHAR(150);

  -- Hapus kolom
  ALTER TABLE users DROP COLUMN last_login;
  ```

- **Hapus tabel:**
  ```sql
  DROP TABLE users;
  ```

## 4. Manipulasi Data (Operasi CRUD)

- **Create (Menyisipkan) data:**
  ```sql
  INSERT INTO users (username, email) VALUES ('john_doe', 'john.doe@example.com');
  INSERT INTO users (username, email) VALUES ('jane_doe', 'jane.doe@example.com');
  ```

- **Read (Membaca) data:**
  ```sql
  -- Pilih semua data dari tabel
  SELECT * FROM users;

  -- Pilih kolom spesifik
  SELECT username, email FROM users;

  -- Filter data dengan klausa WHERE
  SELECT * FROM users WHERE username = 'john_doe';

  -- Gunakan operator seperti AND, OR, LIKE
  SELECT * FROM users WHERE username LIKE 'j%';
  ```

- **Update (Memperbarui) data:**
  *Selalu gunakan klausa `WHERE` untuk menghindari pembaruan semua baris secara tidak sengaja.*
  ```sql
  UPDATE users SET email = 'john.d@new-example.com' WHERE username = 'john_doe';
  ```

- **Delete (Menghapus) data:**
  *Selalu gunakan klausa `WHERE` untuk menghindari penghapusan semua baris secara tidak sengaja.*
  ```sql
  DELETE FROM users WHERE username = 'jane_doe';
  ```

## 5. Join

Mari kita buat tabel lain untuk mendemonstrasikan *join*.

```sql
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    title VARCHAR(200),
    FOREIGN KEY (user_id) REFERENCES users(id)
);

INSERT INTO posts (user_id, title) VALUES (1, 'My first post!');
```

- **INNER JOIN (dapatkan rekaman dengan nilai yang cocok di kedua tabel):**
  ```sql
  SELECT users.username, posts.title
  FROM users
  INNER JOIN posts ON users.id = posts.user_id;
  ```

- **LEFT JOIN (dapatkan semua rekaman dari tabel kiri dan rekaman yang cocok dari kanan):**
  ```sql
  SELECT users.username, posts.title
  FROM users
  LEFT JOIN posts ON users.id = posts.user_id;
  ```

## 6. Agregasi dan Pengurutan

Perintah-perintah ini fundamental untuk menganalisis dan mengatur data.

Mari kita buat tabel `products` untuk contoh-contoh ini.
```sql
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2)
);

INSERT INTO products (name, category, price) VALUES
('Laptop', 'Electronics', 1200.00),
('Mouse', 'Electronics', 25.00),
('Keyboard', 'Electronics', 75.00),
('T-Shirt', 'Apparel', 15.00),
('Jeans', 'Apparel', 50.00);
```

### Fungsi Agregat
- `COUNT()`: Menghitung jumlah baris.
  ```sql
  SELECT COUNT(*) FROM products;
  ```
- `SUM()`: Menghitung jumlah dari sekumpulan nilai.
  ```sql
  SELECT SUM(price) FROM products WHERE category = 'Electronics';
  ```
- `AVG()`: Menghitung nilai rata-rata.
  ```sql
  SELECT AVG(price) FROM products;
  ```
- `MAX()` / `MIN()`: Mendapatkan nilai maksimum/minimum.
  ```sql
  SELECT MAX(price) AS highest_price, MIN(price) AS lowest_price FROM products;
  ```

### `GROUP BY`
Mengelompokkan baris yang memiliki nilai yang sama menjadi baris ringkasan. Sering digunakan dengan fungsi agregat.
```sql
-- Hitung jumlah produk di setiap kategori
SELECT category, COUNT(*) AS product_count
FROM products
GROUP BY category;

-- Dapatkan harga rata-rata untuk setiap kategori
SELECT category, AVG(price) AS average_price
FROM products
GROUP BY category;
```

### `ORDER BY`
Mengurutkan hasil dalam urutan menaik (`ASC`) atau menurun (`DESC`).
```sql
-- Urutkan produk berdasarkan harga, dari terendah ke tertinggi
SELECT * FROM products ORDER BY price ASC;

-- Urutkan produk berdasarkan harga, dari tertinggi ke terendah
SELECT * FROM products ORDER BY price DESC;
```

### `LIMIT`
Membatasi jumlah baris yang dikembalikan oleh kueri.
```sql
-- Dapatkan 3 produk termahal
SELECT * FROM products ORDER BY price DESC LIMIT 3;
```

## 7. Tipe Data Umum MySQL

### Tipe Numerik
- `INT`: Integer standar.
- `TINYINT`: Integer yang sangat kecil. Sering digunakan untuk boolean (`0` untuk salah, `1` untuk benar).
- `BIGINT`: Integer besar untuk angka yang sangat besar.
- `DECIMAL(p, s)`: Angka titik-tetap, sangat baik untuk mata uang. `p` adalah total digit, `s` adalah digit setelah desimal. Contoh: `DECIMAL(10, 2)`.
- `FLOAT`, `DOUBLE`: Angka titik-mengambang untuk perhitungan ilmiah.

### Tipe String
- `VARCHAR(n)`: String dengan panjang variabel dengan ukuran maksimum `n` karakter.
- `CHAR(n)`: String dengan panjang tetap `n` karakter.
- `TEXT`: Untuk teks bentuk panjang seperti posting blog atau deskripsi.
- `ENUM`: Objek string yang hanya dapat memiliki satu nilai, dipilih dari daftar nilai yang diizinkan.

### Tipe Tanggal dan Waktu
- `DATE`: Menyimpan tanggal (YYYY-MM-DD).
- `TIME`: Menyimpan waktu (HH:MM:SS).
- `DATETIME`: Menyimpan kombinasi tanggal dan waktu (YYYY-MM-DD HH:MM:SS).
- `TIMESTAMP`: Mirip dengan `DATETIME` tetapi sadar zona waktu dan memiliki jangkauan yang lebih kecil.

## 8. Pengindeksan

Indeks digunakan untuk mengambil data dari database lebih cepat.

- **Buat indeks pada kolom:**
  ```sql
  CREATE INDEX idx_username ON users(username);
  ```

- **Periksa indeks pada tabel:**
  ```sql
  SHOW INDEX FROM users;
  ```

- **Hapus indeks:**
  ```sql
  DROP INDEX idx_username ON users;
  ```

## 9. Transaksi

Transaksi adalah urutan operasi yang dilakukan sebagai satu unit kerja logis.

- **Mulai transaksi:**
  ```sql
  START TRANSACTION;
  ```

- **Contoh transaksi:**
  ```sql
  START TRANSACTION;
  INSERT INTO users (username, email) VALUES ('test_user', 'test@example.com');
  COMMIT;
  ```

- **Membatalkan transaksi:**
  ```sql
  START TRANSACTION;
  DELETE FROM users;
  ROLLBACK;
  ```

## 10. Manajemen Pengguna

- **Buat pengguna baru:**
  ```sql
  CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
  ```

- **Berikan hak istimewa kepada pengguna:**
  ```sql
  GRANT ALL PRIVILEGES ON my_new_app.* TO 'newuser'@'localhost';
  ```

- **Tampilkan hak istimewa untuk pengguna:**
  ```sql
  SHOW GRANTS FOR 'newuser'@'localhost';
  ```

- **Cabut hak istimewa:**
  ```sql
  REVOKE ALL PRIVILEGES ON my_new_app.* FROM 'newuser'@'localhost';
  ```

- **Hapus pengguna:**
  ```sql
  DROP USER 'newuser'@'localhost';
  ```
