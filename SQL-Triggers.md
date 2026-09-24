# 📌 SQL — TRIGGERS

## 1. What is a Trigger?

A **Trigger** is a SQL program that automatically executes when a specific event happens on a table.

👉 You don't manually call a trigger.

### Easy Memory Trick

```text
EVENT happens
     ↓
TRIGGER automatically runs
```

---

# 2. Why Use Triggers?

Triggers are commonly used for:

- Automatically updating data
- Maintaining audit/history records
- Validating data
- Tracking changes
- Automatically performing related operations

---

# 3. Trigger Events

Common events are:

```text
INSERT
UPDATE
DELETE
```

Example:

```text
Employee inserted
       ↓
Trigger runs automatically
       ↓
Record added to audit table
```

---

# 4. BEFORE Trigger

A `BEFORE` trigger executes **before** the operation happens.

Example:

```sql
CREATE TRIGGER before_employee_insert
BEFORE INSERT ON employees
FOR EACH ROW
SET NEW.created_at = CURRENT_TIMESTAMP;
```

👉 Before inserting an employee, the trigger automatically sets `created_at`.

---

# 5. AFTER Trigger

An `AFTER` trigger executes **after** the operation happens.

Example:

```sql
CREATE TRIGGER after_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
INSERT INTO employee_log(employee_id, action)
VALUES (NEW.id, 'Employee Added');
```

Flow:

```text
INSERT employee
      ↓
Employee inserted
      ↓
AFTER trigger runs
      ↓
Log created
```

---

# 6. NEW and OLD

Triggers often use `NEW` and `OLD`.

### NEW

Represents the **new value**.

Used mainly with:

```text
INSERT
UPDATE
```

Example:

```sql
NEW.salary
```

---

### OLD

Represents the **old value**.

Used mainly with:

```text
UPDATE
DELETE
```

Example:

```sql
OLD.salary
```

### Easy Memory Trick

```text
NEW → New value
OLD → Previous value
```

---

# 7. Example — Audit Log

Suppose we have:

### employees

| id | name | salary |
|----|------|--------|
| 101 | Arun | 40000 |
| 102 | Ravi | 50000 |

### employee_log

| employee_id | action |
|-------------|--------|
| 101 | Employee Added |

Create trigger:

```sql
CREATE TRIGGER employee_insert_log
AFTER INSERT ON employees
FOR EACH ROW
INSERT INTO employee_log(employee_id, action)
VALUES (NEW.id, 'Employee Added');
```

Now execute:

```sql
INSERT INTO employees(id, name, salary)
VALUES (103, 'Dharun', 60000);
```

The trigger automatically executes:

```sql
INSERT INTO employee_log(employee_id, action)
VALUES (103, 'Employee Added');
```

---

# 8. UPDATE Trigger

A trigger can track changes to existing data.

Example:

```sql
CREATE TRIGGER salary_update_log
AFTER UPDATE ON employees
FOR EACH ROW
INSERT INTO employee_log(employee_id, action)
VALUES (NEW.id, 'Employee Updated');
```

If we execute:

```sql
UPDATE employees
SET salary = 70000
WHERE id = 103;
```

The trigger automatically records:

```text
103 | Employee Updated
```

---

# 9. OLD vs NEW Example

Suppose:

```text
Old salary = 60000
New salary = 70000
```

Inside an update trigger:

```sql
OLD.salary
```

returns:

```text
60000
```

and

```sql
NEW.salary
```

returns:

```text
70000
```

We can use both to track changes.

---

# 10. DELETE Trigger

A delete trigger can store information before a record disappears.

Example:

```sql
CREATE TRIGGER employee_delete_log
AFTER DELETE ON employees
FOR EACH ROW
INSERT INTO employee_log(employee_id, action)
VALUES (OLD.id, 'Employee Deleted');
```

Then:

```sql
DELETE FROM employees
WHERE id = 103;
```

The trigger can record:

```text
103 | Employee Deleted
```

👉 For `DELETE`, we use `OLD` because the row is being removed.

---

# 11. BEFORE vs AFTER

| BEFORE | AFTER |
|--------|-------|
| Runs before operation | Runs after operation |
| Can validate/modify incoming data | Useful for logging |
| Executes before INSERT/UPDATE/DELETE | Executes after INSERT/UPDATE/DELETE |

### Easy Memory Trick

```text
BEFORE → Prepare / Validate
AFTER  → Record / React
```

---

# 12. Trigger Syntax

A common MySQL-style syntax is:

```sql
CREATE TRIGGER trigger_name
BEFORE | AFTER INSERT | UPDATE | DELETE
ON table_name
FOR EACH ROW
BEGIN
    -- SQL statements
END;
```

⚠️ Exact trigger syntax differs between database systems.

---

# 13. Drop a Trigger

To remove a trigger:

```sql
DROP TRIGGER trigger_name;
```

Example:

```sql
DROP TRIGGER employee_insert_log;
```

---

# 14. Important Trigger Types

Triggers can be classified by:

### Timing

```text
BEFORE
AFTER
```

### Event

```text
INSERT
UPDATE
DELETE
```

So you may see combinations such as:

```text
BEFORE INSERT
AFTER INSERT

BEFORE UPDATE
AFTER UPDATE

BEFORE DELETE
AFTER DELETE
```

---

# 15. Trigger vs Stored Procedure ⭐

| Trigger | Stored Procedure |
|---------|------------------|
| Runs automatically | Usually called explicitly |
| Associated with a table/event | Stored in database |
| Executes when event occurs | Executes when called |
| Useful for automatic actions | Useful for reusable operations |

### Example

```text
TRIGGER
INSERT happens
     ↓
Automatically runs
```

```text
PROCEDURE
CALL procedure_name()
     ↓
Procedure runs
```

---

# 16. Advantages

✅ Automatic execution  
✅ Useful for audit logging  
✅ Helps maintain data consistency  
✅ Can automate repetitive database operations  

---

# 17. Disadvantages

❌ Can make database behavior harder to understand  
❌ Debugging can become difficult  
❌ Too many triggers can affect performance  
❌ Hidden automatic operations may surprise developers  

👉 Use triggers carefully.

---

# 🎯 Quick Revision

```text
TRIGGER
   ↓
Automatically executes
   ↓
When an event occurs
   ↓
INSERT / UPDATE / DELETE
```

```text
BEFORE → Runs before operation
AFTER  → Runs after operation

NEW → New value
OLD → Previous value
```

---

# 💡 Interview One-Liners

### What is a Trigger?

> A trigger is a database object that automatically executes when a specified event occurs on a table.

### When are triggers used?

> Triggers are commonly used for auditing, validation, maintaining consistency, and automatically performing related operations.

### What is the difference between BEFORE and AFTER triggers?

> A BEFORE trigger executes before the database operation, while an AFTER trigger executes after it.

### What is NEW in a trigger?

> NEW represents the new row values involved in an INSERT or UPDATE operation.

### What is OLD in a trigger?

> OLD represents the previous row values involved in an UPDATE or DELETE operation.

### Trigger vs Stored Procedure?

> A trigger runs automatically when an event occurs, while a stored procedure is normally executed explicitly.

---

# 📝 Practice Questions

### Q1.
Create a trigger that logs whenever a new employee is inserted.

### Q2.
Create an update trigger that records salary changes.

### Q3.
Which keyword represents the previous value?

```text
A) NEW
B) OLD
C) PREVIOUS
D) BEFORE
```

### Q4.
Which trigger runs after an INSERT?

```text
A) BEFORE INSERT
B) AFTER INSERT
C) BEFORE UPDATE
D) AFTER DELETE
```

### Q5.
What is the main difference between a trigger and a stored procedure?

---

# ⭐ Key Point

```text
Trigger = Automatic SQL action

INSERT / UPDATE / DELETE
          ↓
       TRIGGER
          ↓
    SQL automatically runs
```
