# Panduan Belajar PostgreSQL

Panduan komprehensif untuk perintah dasar dan menengah PostgreSQL.

## 1. Terhubung ke PostgreSQL

Pertama, akses *container* yang sedang berjalan untuk menggunakan klien baris perintah `psql`.

```bash
# Dapatkan nama atau ID container
docker ps

# Akses container (asumsi nama container adalah postgres_container)
docker exec -it postgres_container bash
```

Setelah di dalam *container*, hubungkan ke PostgreSQL:

```bash
# Terhubung ke database spesifik dengan user
psql -U user -d example_db
# Anda akan diminta memasukkan password yang Anda atur di file .env
```

## 2. Perintah Meta `psql`

`psql` memiliki perintah internal yang kuat (perintah meta) yang diawali dengan garis miring terbalik (`\`).

- **Daftar semua database:** `\l`
- **Daftar semua tabel di database saat ini:** `\dt`
- **Jelaskan struktur tabel:** `\d users`
- **Daftar pengguna/peran:** `\du`
- **Keluar dari `psql`:** `\q`

## 3. Operasi Database

- **Buat database baru:**
  ```sql
  CREATE DATABASE my_new_app;
  ```

- **Terhubung ke database lain:**
  ```sql
  \c my_new_app
  ```

- **Hapus database:**
  ```sql
  DROP DATABASE my_new_app;
  ```

## 4. Operasi Tabel

- **Buat tabel baru:**
  *PostgreSQL menggunakan `SERIAL` untuk integer yang bertambah otomatis. Tipe lain seperti `INT`, `VARCHAR(255)`, `TEXT`, `DATE`, `TIMESTAMP` juga umum.*
  ```sql
  CREATE TABLE users (
      id SERIAL PRIMARY KEY,
      username VARCHAR(50) NOT NULL UNIQUE,
      email VARCHAR(100) NOT NULL,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- **Ubah tabel (tambah, ubah, hapus kolom):**
  ```sql
  -- Tambah kolom baru
  ALTER TABLE users ADD COLUMN last_login DATE;

  -- Ubah tipe data kolom
  ALTER TABLE users ALTER COLUMN email TYPE VARCHAR(150);

  -- Hapus kolom
  ALTER TABLE users DROP COLUMN last_login;
  ```

- **Hapus tabel:**
  ```sql
  DROP TABLE users;
  ```

## 5. Manipulasi Data (Operasi CRUD)

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

## 6. Join

Mari kita buat tabel lain untuk mendemonstrasikan *join*.

```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    user_id INT,
    title VARCHAR(200),
    CONSTRAINT fk_user
        FOREIGN KEY(user_id) 
        REFERENCES users(id)
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

## 7. Agregasi dan Pengurutan

Perintah-perintah ini fundamental untuk menganalisis dan mengatur data.

Mari kita buat tabel `products` untuk contoh-contoh ini.
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price NUMERIC(10, 2)
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

## 8. Tipe Data Umum PostgreSQL

### Tipe Numerik
- `INTEGER` (atau `INT`): Integer standar.
- `SMALLINT`: Integer dengan jangkauan kecil.
- `BIGINT`: Integer dengan jangkauan besar.
- `SERIAL`: Integer yang bertambah otomatis, sering digunakan untuk kunci primer. `BIGSERIAL` juga tersedia.
- `NUMERIC(p, s)`: Angka titik-tetap, ideal untuk mata uang.
- `REAL`, `DOUBLE PRECISION`: Angka titik-mengambang.

### Tipe Karakter
- `VARCHAR(n)`: String dengan panjang variabel.
- `CHAR(n)`: String dengan panjang tetap.
- `TEXT`: Untuk teks bentuk panjang tanpa batas yang telah ditentukan.

### Tipe Tanggal dan Waktu
- `DATE`: Menyimpan tanggal (YYYY-MM-DD).
- `TIME`: Menyimpan waktu (HH:MM:SS).
- `TIMESTAMP`: Menyimpan tanggal dan waktu.
- `TIMESTAMPTZ`: Timestamp yang sadar zona waktu. Ini sering menjadi pilihan yang direkomendasikan.

### Tipe Boolean
- `BOOLEAN`: Menyimpan nilai `true` atau `false`.

### Tipe Khusus
- `UUID`: Menyimpan pengidentifikasi unik universal.
- `JSON`, `JSONB`: Menyimpan data JSON. `JSONB` umumnya direkomendasikan.
- `ARRAY`: Memungkinkan kolom untuk menyimpan array nilai, mis., `TEXT[]` atau `INT[]`.

## 9. Pengindeksan

Indeks digunakan untuk mempercepat kinerja kueri.

- **Buat indeks pada kolom:**
  ```sql
  CREATE INDEX idx_users_username ON users (username);
  ```

- **Periksa indeks pada tabel:**
  Gunakan perintah meta `psql`:
  ```
  \d users
  ```

- **Hapus indeks:**
  ```sql
  DROP INDEX idx_users_username;
  ```

## 10. Transaksi

PostgreSQL memperlakukan setiap pernyataan SQL sebagai dieksekusi dalam sebuah transaksi.

- **Mulai transaksi:**
  ```sql
  BEGIN;
  ```

- **Contoh transaksi:**
  ```sql
  BEGIN;
  INSERT INTO users (username, email) VALUES ('test_user', 'test@example.com');
  COMMIT;
  ```

- **Membatalkan transaksi:**
  ```sql
  BEGIN;
  DELETE FROM users;
  ROLLBACK;
  ```

## 11. Manajemen Pengguna dan Peran

PostgreSQL mengelola pengguna sebagai "peran".

- **Buat pengguna (peran) baru:**
  ```sql
  CREATE ROLE newuser WITH LOGIN PASSWORD 'password';
  ```

- **Berikan hak istimewa ke peran:**
  ```sql
  GRANT CONNECT ON DATABASE my_new_app TO newuser;
  GRANT ALL ON users TO newuser;
  ```

- **Tampilkan hak istimewa untuk pengguna:**
  Gunakan perintah meta `psql`:
  ```
  \du newuser
  \dp users
  ```

- **Cabut hak istimewa:**
  ```sql
  REVOKE ALL ON users FROM newuser;
  ```

- **Hapus pengguna (peran):**
  ```sql
  DROP ROLE newuser;
  ```
