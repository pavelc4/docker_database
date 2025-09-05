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
# You will be prompted for the password you set in your .env file.
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

## 6. Aggregation and Sorting

These commands are fundamental for analyzing and organizing data.

Let's create a `products` table for these examples.
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

### Aggregate Functions
- `COUNT()`: Counts the number of rows.
  ```sql
  SELECT COUNT(*) FROM products;
  ```
- `SUM()`: Calculates the sum of a set of values.
  ```sql
  SELECT SUM(price) FROM products WHERE category = 'Electronics';
  ```
- `AVG()`: Calculates the average value.
  ```sql
  SELECT AVG(price) FROM products;
  ```
- `MAX()` / `MIN()`: Gets the maximum/minimum value.
  ```sql
  SELECT MAX(price) AS highest_price, MIN(price) AS lowest_price FROM products;
  ```

### `GROUP BY`
Groups rows that have the same values into summary rows. It's often used with aggregate functions.
```sql
-- Count the number of products in each category
SELECT category, COUNT(*) AS product_count
FROM products
GROUP BY category;

-- Get the average price for each category
SELECT category, AVG(price) AS average_price
FROM products
GROUP BY category;
```

### `ORDER BY`
Sorts the result set in ascending (`ASC`) or descending (`DESC`) order.
```sql
-- Sort products by price, from lowest to highest
SELECT * FROM products ORDER BY price ASC;

-- Sort products by price, from highest to lowest
SELECT * FROM products ORDER BY price DESC;
```

### `LIMIT`
Constrains the number of rows returned by a query.
```sql
-- Get the top 3 most expensive products
SELECT * FROM products ORDER BY price DESC LIMIT 3;
```

## 7. Common MySQL Data Types

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

## 8. Indexing

Indexes are used to retrieve data from the database more quickly. They are special lookup tables that the database search engine can use to speed up data retrieval.

- **Create an index on a column:**
  ```sql
  CREATE INDEX idx_username ON users(username);
  ```

- **Check for indexes on a table:**
  ```sql
  SHOW INDEX FROM users;
  ```

- **Drop an index:**
  ```sql
  DROP INDEX idx_username ON users;
  ```

## 9. Transactions

Transactions are a sequence of operations performed as a single logical unit of work. All operations must succeed; otherwise, the entire transaction is rolled back.

- **Start a transaction:**
  ```sql
  START TRANSACTION;
  ```

- **Example transaction:**
  ```sql
  -- Start the transaction
  START TRANSACTION;

  -- Perform some operations
  INSERT INTO users (username, email) VALUES ('test_user', 'test@example.com');
  UPDATE posts SET title = 'New Title' WHERE user_id = 1;

  -- If everything is okay, commit the changes
  COMMIT;
  ```

- **Rolling back a transaction:**
  If something goes wrong, you can undo all changes since the transaction started.
  ```sql
  -- Start the transaction
  START TRANSACTION;

  -- Perform some operations
  DELETE FROM users;

  -- Something is wrong! Roll back the changes.
  ROLLBACK;
  ```

## 10. User Management

- **Create a new user:**
  ```sql
  CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
  ```

- **Grant privileges to a user:**
  ```sql
  -- Grant all privileges on a specific database
  GRANT ALL PRIVILEGES ON my_new_app.* TO 'newuser'@'localhost';

  -- Grant only SELECT privilege on a specific table
  GRANT SELECT ON my_new_app.users TO 'newuser'@'localhost';
  ```

- **Show grants for a user:**
  ```sql
  SHOW GRANTS FOR 'newuser'@'localhost';
  ```

- **Revoke privileges:**
  ```sql
  REVOKE ALL PRIVILEGES ON my_new_app.* FROM 'newuser'@'localhost';
  ```

- **Delete a user:**
  ```sql
  DROP USER 'newuser'@'localhost';
  ```