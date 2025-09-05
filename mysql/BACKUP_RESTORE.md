# MySQL Backup and Restore Guide

This guide explains how to back up and restore your MySQL database using `mysqldump` from within the Docker environment.

## 1. Backup a Database

To perform a backup, you can execute the `mysqldump` command from your host machine against the running container.

This command will create a `backup.sql` file in your current directory on the host.

```bash
# Command format:
# docker exec [container_name] mysqldump -u [user] -p[password] [database_name] > [backup_file.sql]

docker exec mysql_db mysqldump -u root -p1945 testing_db > backup.sql
```

*Note: There is no space between `-p` and the password.*

### Backing Up All Databases

To back up all databases managed by the server:

```bash
docker exec mysql_db mysqldump -u root -p1945 --all-databases > all_databases_backup.sql
```

## 2. Restore a Database

To restore a database from a `.sql` file, you can pipe the file into the `mysql` command inside the container.

First, make sure the database you are restoring to exists. If not, create it.

```bash
# Command format:
# docker exec -i [container_name] mysql -u [user] -p[password] [database_name] < [backup_file.sql]

docker exec -i mysql_db mysql -u root -p1945 testing_db < backup.sql
```

This command reads the `backup.sql` file from your host and executes it inside the `mysql_db` container.
