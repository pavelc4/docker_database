# PostgreSQL Backup and Restore Guide

This guide explains how to back up and restore your PostgreSQL database using `pg_dump` and `pg_restore`/`psql` from within the Docker environment.

## 1. Backup a Database

To perform a backup, you can execute the `pg_dump` command from your host machine against the running container. `pg_dump` creates a compressed archive file by default which is often preferred.

This command will create a `backup.tar` file in your current directory on the host.

```bash
# Command format:
# docker exec -t [container_name] pg_dump -U [user] -d [database_name] -F t > [backup_file.tar]

docker exec -t postgres_container pg_dump -U aether -d example_db -F t > backup.tar
```

- `-F t` specifies the format as a tar archive.

### Plain-Text SQL Backup

If you prefer a plain `.sql` file:

```bash
docker exec -t postgres_container pg_dump -U aether -d example_db > backup.sql
```

## 2. Restore a Database

How you restore depends on the backup format.

### Restoring from a Plain-Text `.sql` File

You can pipe the `.sql` file directly into the `psql` client.

```bash
# Command format:
# docker exec -i [container_name] psql -U [user] -d [database_name] < [backup_file.sql]

cat backup.sql | docker exec -i postgres_container psql -U aether -d example_db
```

### Restoring from a `.tar` Archive File

For compressed archives, you must use `pg_restore`. You first need to copy the backup file into the container.

```bash
# 1. Copy the backup file into the container
docker cp backup.tar postgres_container:/tmp/backup.tar

# 2. Execute pg_restore inside the container
# The --create flag will re-create the database before restoring.
# The -d postgres is used to connect to the maintenance db to perform the create.
docker exec -it postgres_container pg_restore -U aether -d postgres --create --verbose /tmp/backup.tar
```
