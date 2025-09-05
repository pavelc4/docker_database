# MySQL Learning Guide

A comprehensive guide to basic and intermediate MySQL commands.

## 1. Connecting to MySQL

First, access the running container to use the MySQL command-line client.

```bash
# Get the container name or ID
docker ps

# Access the container (assuming container name is mysql_db)
docker exec -it mysql_db bash
```

Once inside the container, connect to MySQL:

```bash
# Connect using the root user
mysql -u root -p
# You will be prompted for the password (1945)
```

## 2. Database Operations

- **Show all databases:**
  ```sql
  SHOW DATABASES;
  ```

- **Create a new database:**
  ```sql
  CREATE DATABASE my_new_app;
  ```

- **Select a database to use:**
  ```sql
  USE my_new_app;
  ```

- **Delete a database:**
  ```sql
  DROP DATABASE my_new_app;
  ```

## 3. Table Operations

- **Create a new table:**
  *Data types like `INT`, `VARCHAR(255)`, `TEXT`, `DATE`, `TIMESTAMP` are common.*
  ```sql
  CREATE TABLE users (
      id INT AUTO_INCREMENT PRIMARY KEY,
      username VARCHAR(50) NOT NULL UNIQUE,
      email VARCHAR(100) NOT NULL,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

- **Show all tables in the current database:**
  ```sql
  SHOW TABLES;
  ```

- **Describe a table structure:**
  ```sql
  DESCRIBE users;
  -- or
  EXPLAIN users;
  ```

- **Modify a table (add, modify, drop a column):**
  ```sql
  -- Add a new column
  ALTER TABLE users ADD COLUMN last_login DATE;

  -- Modify a column's data type
  ALTER TABLE users MODIFY COLUMN email VARCHAR(150);

  -- Drop a column
  ALTER TABLE users DROP COLUMN last_login;
  ```

- **Delete a table:**
  ```sql
  DROP TABLE users;
  ```

## 4. Data Manipulation (CRUD Operations)

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

## 5. Joins

Let's create another table to demonstrate joins.

```sql
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    title VARCHAR(200),
    FOREIGN KEY (user_id) REFERENCES users(id)
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

## 6. Common MySQL Data Types

### Numeric Types
- `INT`: Standard integer.
- `TINYINT`: A very small integer. Often used for booleans (`0` for false, `1` for true).
- `BIGINT`: A large integer for very big numbers.
- `DECIMAL(p, s)`: A fixed-point number, excellent for currency. `p` is total digits, `s` is digits after the decimal. Example: `DECIMAL(10, 2)`.
- `FLOAT`, `DOUBLE`: Floating-point numbers for scientific calculations.

### String Types
- `VARCHAR(n)`: A variable-length string with a maximum size of `n` characters. Use this for most text fields.
- `CHAR(n)`: A fixed-length string of `n` characters. If the input is shorter, it's padded with spaces.
- `TEXT`: For long-form text like blog posts or descriptions.
- `ENUM`: A string object that can have only one value, chosen from a list of allowed values specified at table creation.

### Date and Time Types
- `DATE`: Stores a date (YYYY-MM-DD).
- `TIME`: Stores a time (HH:MM:SS).
- `DATETIME`: Stores a date and time combination (YYYY-MM-DD HH:MM:SS).
- `TIMESTAMP`: Similar to `DATETIME` but is timezone-aware and has a smaller range. It's often used for tracking creation or modification times.