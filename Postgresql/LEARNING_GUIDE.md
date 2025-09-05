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
psql -U user -d example_db
# You will be prompted for the password you set in your .env file.
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

Let's create another table to demonstrate joins.

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

## 7. Aggregation and Sorting

These commands are fundamental for analyzing and organizing data.

Let's create a `products` table for these examples.
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

## 8. Common PostgreSQL Data Types

### Numeric Types
- `INTEGER` (or `INT`): Standard integer.
- `SMALLINT`: A small-range integer.
- `BIGINT`: A large-range integer.
- `SERIAL`: An auto-incrementing integer, often used for primary keys. `BIGSERIAL` is also available.
- `NUMERIC(p, s)`: A fixed-point number, ideal for currency where precision is critical. `p` is total digits, `s` is digits after the decimal.
- `REAL`, `DOUBLE PRECISION`: Floating-point numbers.

### Character Types
- `VARCHAR(n)`: A variable-length string with a maximum size of `n` characters.
- `CHAR(n)`: A fixed-length string of `n` characters.
- `TEXT`: For long-form text with no predefined limit.

### Date and Time Types
- `DATE`: Stores a date (YYYY-MM-DD).
- `TIME`: Stores a time of day (HH:MM:SS).
- `TIMESTAMP`: Stores both date and time.
- `TIMESTAMPTZ`: A timezone-aware timestamp. This is often the recommended choice for web applications.

### Boolean Type
- `BOOLEAN`: Stores `true` or `false` values.

### Special Types
- `UUID`: Stores a universally unique identifier.
- `JSON`, `JSONB`: Stores JSON data. `JSONB` is stored in a decomposed binary format which is faster to process but slightly slower to input. It also supports indexing. `JSONB` is generally recommended.
- `ARRAY`: Allows a column to store an array of values, e.g., `TEXT[]` or `INT[]`.

## 9. Indexing

Indexes are used to speed up the performance of queries. They are crucial for optimizing data retrieval on large tables.

- **Create an index on a column:**
  ```sql
  CREATE INDEX idx_users_username ON users (username);
  ```

- **Check for indexes on a table:**
  Use the `psql` meta-commands:
  ```
  \d users
  ```

- **Drop an index:**
  ```sql
  DROP INDEX idx_users_username;
  ```

## 10. Transactions

PostgreSQL treats every SQL statement as being executed within a transaction. `BEGIN` and `COMMIT` are used to group multiple statements into a single transaction.

- **Start a transaction:**
  ```sql
  BEGIN;
  ```

- **Example transaction:**
  ```sql
  BEGIN;
  INSERT INTO users (username, email) VALUES ('test_user', 'test@example.com');
  UPDATE posts SET title = 'New Title' WHERE user_id = 1;
  COMMIT;
  ```

- **Rolling back a transaction:**
  If you make a mistake, you can roll back the changes before committing.
  ```sql
  BEGIN;
  DELETE FROM users;
  ROLLBACK;
  ```

## 11. User and Role Management

PostgreSQL manages users as "roles". A role can be a user or a group.

- **Create a new user (role):**
  ```sql
  CREATE ROLE newuser WITH LOGIN PASSWORD 'password';
  ```

- **Grant privileges to a role:**
  ```sql
  -- Grant connect access to a database
  GRANT CONNECT ON DATABASE my_new_app TO newuser;

  -- Grant all privileges on a specific table
  GRANT ALL ON users TO newuser;

  -- Grant only SELECT on all tables in a schema
  GRANT SELECT ON ALL TABLES IN SCHEMA public TO newuser;
  ```

- **Show grants for a user:**
  Use `psql` meta-commands:
  ```
  \du newuser
  \dp users
  ```

- **Revoke privileges:**
  ```sql
  REVOKE ALL ON users FROM newuser;
  ```

- **Delete a user (role):**
  ```sql
  DROP ROLE newuser;
  ```