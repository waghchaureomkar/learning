# 📘 MySQL Cheatsheet

A quick reference guide for basic and intermediate MySQL commands.

---

## 🗃️ Database Operations

```sql
CREATE DATABASE dbname;
SHOW DATABASES;
USE dbname;
DROP DATABASE dbname;
```

---

## 📁 Table Operations

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

SHOW TABLES;
DESCRIBE users;
DROP TABLE users;
```

---

## ✏️ CRUD Operations

### 🔹 Insert
```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### 🔹 Read
```sql
SELECT * FROM users;
SELECT name, email FROM users;
SELECT * FROM users WHERE id = 1;
```

### 🔹 Update
```sql
UPDATE users SET name = 'Jane Doe' WHERE id = 1;
```

### 🔹 Delete
```sql
DELETE FROM users WHERE id = 1;
```

---

## 🔍 Filtering Data

```sql
SELECT * FROM users WHERE name = 'John';
SELECT * FROM users WHERE id > 5;
SELECT * FROM users WHERE id BETWEEN 5 AND 10;
SELECT * FROM users WHERE name IN ('John', 'Jane');
SELECT * FROM users WHERE name LIKE 'J%';
SELECT * FROM users WHERE email IS NOT NULL;
```

---

## 📊 Sorting and Limiting

```sql
SELECT * FROM users ORDER BY created_at DESC;
SELECT * FROM users LIMIT 5;
SELECT * FROM users LIMIT 5 OFFSET 10;
```

---

## 🔗 Joins

```sql
-- INNER JOIN
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;

-- LEFT JOIN
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

-- RIGHT JOIN
SELECT users.name, orders.amount
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

---

## 🧮 Aggregate Functions

```sql
SELECT COUNT(*) FROM users;
SELECT SUM(amount) FROM orders;
SELECT AVG(amount) FROM orders;
SELECT MIN(amount), MAX(amount) FROM orders;
```

---

## 🧱 Group By & Having

```sql
SELECT user_id, COUNT(*) FROM orders GROUP BY user_id;

SELECT user_id, COUNT(*) AS total
FROM orders
GROUP BY user_id
HAVING total > 5;
```

---

## 🔧 Indexes

```sql
CREATE INDEX idx_name ON users(name);
DROP INDEX idx_name ON users;
```

---

## 🧾 Misc

```sql
RENAME TABLE old_name TO new_name;

ALTER TABLE users ADD age INT;
ALTER TABLE users MODIFY email VARCHAR(255);
ALTER TABLE users DROP COLUMN age;
```

---

## 📌 Notes

- Use `LIMIT` for pagination.
- Always back up before running `DROP` or `ALTER` commands.
- Use `EXPLAIN` to understand query performance.

---

> 📂 Created by Omka Waghchaure — [Your GitHub link]
