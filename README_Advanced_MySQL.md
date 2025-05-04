# 📘 MySQL Cheatsheet – Advanced Topics

This guide covers advanced MySQL topics including views, transactions, stored procedures, functions, triggers, and temporary tables.

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

### Basic Usage
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

### Create a Stored Procedure
```sql
DELIMITER //
CREATE PROCEDURE GetUserById(IN userId INT)
BEGIN
    SELECT * FROM users WHERE id = userId;
END //
DELIMITER ;
```

### Call the Procedure
```sql
CALL GetUserById(5);
```

### Drop a Procedure
```sql
DROP PROCEDURE GetUserById;
```

---

## 🧠 Functions

### Create a Custom Function
```sql
DELIMITER //
CREATE FUNCTION getTotalOrders(uid INT) RETURNS INT
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total FROM orders WHERE user_id = uid;
    RETURN total;
END //
DELIMITER ;
```

### Use the Function
```sql
SELECT getTotalOrders(3);
```

---

## 🔐 Triggers

### Create a Trigger
```sql
CREATE TRIGGER before_user_insert
BEFORE INSERT ON users
FOR EACH ROW
SET NEW.created_at = NOW();
```

### Drop a Trigger
```sql
DROP TRIGGER before_user_insert;
```

> Triggers automatically perform actions before or after insert/update/delete events.

---

## 📦 Temporary Tables

### Create and Use
```sql
CREATE TEMPORARY TABLE temp_users AS
SELECT id, name FROM users WHERE status = 'inactive';

SELECT * FROM temp_users;
```

> Temporary tables exist only for the current session and are dropped automatically when the session ends.

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
