# PostgreSQL Learning Guide

A comprehensive guide to basic and intermediate PostgreSQL commands.

## 1. Connecting to PostgreSQL

First, access the running container to use the `psql` command-line client.

```bash
# Get the container name or ID
docker ps

# Access the container (assuming container name is postgres_container)
docker exec -it postgres_container bash
```

Once inside the container, connect to PostgreSQL:

```bash
# Connect to a specific database with a user
psql -U aether -d example_db
# You will be prompted for the password (1945)
```

## 2. `psql` Meta-Commands

`psql` has powerful internal commands (meta-commands) that start with a backslash (`\`).

- **List all databases:** `\l`
- **List all tables in the current database:** `\dt`
- **Describe a table structure:** `\d users`
- **List users/roles:** `\du`
- **Quit `psql`:** `\q`

## 3. Database Operations

- **Create a new database:**
  ```sql
  CREATE DATABASE my_new_app;
  ```

- **Connect to a different database:**
  ```sql
  \c my_new_app
  ```

- **Delete a database:**
  ```sql
  DROP DATABASE my_new_app;
  ```

## 4. Table Operations

- **Create a new table:**
  *PostgreSQL uses `SERIAL` for auto-incrementing integers. Other types like `INT`, `VARCHAR(255)`, `TEXT`, `DATE`, `TIMESTAMP` are also common.*
  ```sql
  CREATE TABLE users (
      id SERIAL PRIMARY KEY,
      username VARCHAR(50) NOT NULL UNIQUE,
      email VARCHAR(100) NOT NULL,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- **Modify a table (add, modify, drop a column):**
  ```sql
  -- Add a new column
  ALTER TABLE users ADD COLUMN last_login DATE;

  -- Modify a column's data type
  ALTER TABLE users ALTER COLUMN email TYPE VARCHAR(150);

  -- Drop a column
  ALTER TABLE users DROP COLUMN last_login;
  ```

- **Delete a table:**
  ```sql
  DROP TABLE users;
  ```

## 5. Data Manipulation (CRUD Operations)

- **Create (Insert) data:**
  ```sql
  INSERT INTO users (username, email) VALUES ('john_doe', 'john.doe@example.com');
  INSERT INTO users (username, email) VALUES ('jane_doe', 'jane.doe@example.com');
  ```

- **Read (Select) data:**
  ```sql
  -- Select all data from a table
  SELECT * FROM users;

  -- Select specific columns
  SELECT username, email FROM users;

  -- Filter data with WHERE clause
  SELECT * FROM users WHERE username = 'john_doe';

  -- Use operators like AND, OR, LIKE
  SELECT * FROM users WHERE username LIKE 'j%';
  ```

- **Update data:**
  *Always use a `WHERE` clause to avoid updating all rows by mistake.*
  ```sql
  UPDATE users SET email = 'john.d@new-example.com' WHERE username = 'john_doe';
  ```

- **Delete data:**
  *Always use a `WHERE` clause to avoid deleting all rows by mistake.*
  ```sql
  DELETE FROM users WHERE username = 'jane_doe';
  ```

## 6. Joins

Let\'s create another table to demonstrate joins.

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

- **INNER JOIN (get records with matching values in both tables):**
  ```sql
  SELECT users.username, posts.title
  FROM users
  INNER JOIN posts ON users.id = posts.user_id;
  ```

- **LEFT JOIN (get all records from the left table and matched records from the right):**
  ```sql
  SELECT users.username, posts.title
  FROM users
  LEFT JOIN posts ON users.id = posts.user_id;
  ```

