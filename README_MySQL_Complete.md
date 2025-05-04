# 📘 MySQL Cheatsheet (Basic + Advanced)

A complete reference for basic and advanced MySQL operations. Useful for developers, database admins, and learners.

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

# 🚀 Advanced MySQL Topics

---

## 🔍 Views

### Create a View
```sql
CREATE VIEW active_users AS
SELECT id, name, email
FROM users
WHERE status = 'active';
```

### Use a View
```sql
SELECT * FROM active_users;
```

### Update Through a View (only if allowed)
```sql
UPDATE active_users SET name = 'Omka' WHERE id = 1;
```

### Drop a View
```sql
DROP VIEW active_users;
```

---

## 🔁 Transactions

```sql
START TRANSACTION;

-- SQL operations
UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

-- Save changes
COMMIT;

-- Or revert if something goes wrong
ROLLBACK;
```

> Use transactions when multiple queries must succeed or fail together.

---

## ⚙️ Stored Procedures

```sql
DELIMITER //
CREATE PROCEDURE GetUserById(IN userId INT)
BEGIN
    SELECT * FROM users WHERE id = userId;
END //
DELIMITER ;

-- Call the Procedure
CALL GetUserById(5);

-- Drop the Procedure
DROP PROCEDURE GetUserById;
```

---

## 🧠 Functions

```sql
DELIMITER //
CREATE FUNCTION getTotalOrders(uid INT) RETURNS INT
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total FROM orders WHERE user_id = uid;
    RETURN total;
END //
DELIMITER ;

-- Use Function
SELECT getTotalOrders(3);
```

---

## 🔐 Triggers

```sql
-- Create Trigger
CREATE TRIGGER before_user_insert
BEFORE INSERT ON users
FOR EACH ROW
SET NEW.created_at = NOW();

-- Drop Trigger
DROP TRIGGER before_user_insert;
```

---

## 📦 Temporary Tables

```sql
CREATE TEMPORARY TABLE temp_users AS
SELECT id, name FROM users WHERE status = 'inactive';

-- Use the temporary table
SELECT * FROM temp_users;
```

---

## ✅ Summary

- **Views** simplify complex queries and improve reusability.
- **Transactions** ensure data integrity across multiple operations.
- **Stored Procedures** help modularize and reuse logic.
- **Functions** return single values and can be used inside SQL.
- **Triggers** automatically respond to table events.
- **Temporary Tables** store session-specific intermediate data.

---

> 📂 Created by Omka Waghchaure — [Your GitHub link]
