# Panduan Backup dan Restore PostgreSQL

Panduan ini menjelaskan cara mem-backup dan me-restore database PostgreSQL Anda menggunakan `pg_dump` dan `pg_restore`/`psql` dari dalam lingkungan Docker.

## 1. Backup Database

Untuk melakukan backup, Anda dapat menjalankan perintah `pg_dump` dari mesin host Anda terhadap *container* yang sedang berjalan. `pg_dump` secara default membuat file arsip terkompresi yang seringkali lebih disukai.

Perintah ini akan membuat file `backup.tar` di direktori Anda saat ini di host.

```bash
# Format perintah:
# docker exec -t [nama_container] pg_dump -U [user] -d [nama_database] -F t > [file_backup.tar]

docker exec -t postgres_container pg_dump -U ${POSTGRES_USER} -d ${POSTGRES_DB} -F t > backup.tar
```

- `-F t` menentukan format sebagai arsip tar.

### Backup SQL Teks Biasa

Jika Anda lebih suka file `.sql` biasa:

```bash
docker exec -t postgres_container pg_dump -U ${POSTGRES_USER} -d ${POSTGRES_DB} > backup.sql
```

## 2. Restore Database

Cara Anda me-restore tergantung pada format backup.

### Restore dari File `.sql` Teks Biasa

Anda dapat menyalurkan file `.sql` langsung ke klien `psql`.

```bash
# Format perintah:
# docker exec -i [nama_container] psql -U [user] -d [nama_database] < [file_backup.sql]

cat backup.sql | docker exec -i postgres_container psql -U ${POSTGRES_USER} -d ${POSTGRES_DB}
```

### Restore dari File Arsip `.tar`

Untuk arsip terkompresi, Anda harus menggunakan `pg_restore`. Anda pertama-tama perlu menyalin file backup ke dalam *container*.

```bash
# 1. Salin file backup ke dalam container
docker cp backup.tar postgres_container:/tmp/backup.tar

# 2. Jalankan pg_restore di dalam container
# Flag --create akan membuat ulang database sebelum me-restore.
# -d postgres digunakan untuk terhubung ke db pemeliharaan untuk melakukan pembuatan.
docker exec -it postgres_container pg_restore -U ${POSTGRES_USER} -d postgres --create --verbose /tmp/backup.tar
```
