# Panduan Backup dan Restore MySQL

Panduan ini menjelaskan cara mem-backup dan me-restore database MySQL Anda menggunakan `mysqldump` dari dalam lingkungan Docker.

## 1. Backup Database

Untuk melakukan backup, Anda dapat menjalankan perintah `mysqldump` dari mesin host Anda terhadap *container* yang sedang berjalan.

Perintah ini akan membuat file `backup.sql` di direktori Anda saat ini di host.

```bash
# Format perintah:
# docker exec [nama_container] mysqldump -u [user] -p[password] [nama_database] > [file_backup.sql]

docker exec mysql_db mysqldump -u root -p${MYSQL_ROOT_PASSWORD} testing_db > backup.sql
```

*Catatan: Tidak ada spasi antara `-p` dan password.*

### Backup Semua Database

Untuk mem-backup semua database yang dikelola oleh server:

```bash
docker exec mysql_db mysqldump -u root -p${MYSQL_ROOT_PASSWORD} --all-databases > all_databases_backup.sql
```

## 2. Restore Database

Untuk me-restore database dari file `.sql`, Anda dapat menyalurkan file tersebut ke perintah `mysql` di dalam *container*.

Pertama, pastikan database tujuan restore sudah ada. Jika tidak, buatlah terlebih dahulu.

```bash
# Format perintah:
# docker exec -i [nama_container] mysql -u [user] -p[password] [nama_database] < [file_backup.sql]

docker exec -i mysql_db mysql -u root -p${MYSQL_ROOT_PASSWORD} testing_db < backup.sql
```

Perintah ini membaca file `backup.sql` dari host Anda dan menjalankannya di dalam *container* `mysql_db`.
