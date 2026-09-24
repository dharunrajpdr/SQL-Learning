# 📘 SQL – SELECT, INSERT, UPDATE & DELETE

> 💡 **In this lesson:** Learn how to retrieve, add, modify, and remove data from a SQL table.

---

# 1️⃣ SELECT

## 🧠 What is SELECT?

`SELECT` is used to **retrieve data** from a table.

It is one of the most commonly used SQL commands.

### 📌 Syntax

```sql
SELECT column1, column2
FROM table_name;
```

To retrieve all columns:

```sql
SELECT *
FROM table_name;
```

---

## 🔹 Example Table

Let's use an `employees` table:

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 2 | Ravi | HR | 30000 |
| 3 | Kiran | IT | 50000 |
| 4 | Priya | Finance | 35000 |
| 5 | Anu | Sales | 45000 |

---

## 🔹 Example 1: Select all columns

```sql
SELECT *
FROM employees;
```

### 📤 Output

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 2 | Ravi | HR | 30000 |
| 3 | Kiran | IT | 50000 |
| 4 | Priya | Finance | 35000 |
| 5 | Anu | Sales | 45000 |

### 🧠 Meaning

```text
*
↓
All columns
```

---

## 🔹 Example 2: Select specific columns

```sql
SELECT name, salary
FROM employees;
```

### 📤 Output

| name | salary |
|------|-------:|
| Arun | 40000 |
| Ravi | 30000 |
| Kiran | 50000 |
| Priya | 35000 |
| Anu | 45000 |

---

## 🔹 Example 3: SELECT with WHERE

We can use `WHERE` to retrieve only specific rows.

```sql
SELECT *
FROM employees
WHERE department = 'IT';
```

### 📤 Output

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 3 | Kiran | IT | 50000 |

---

## 🔹 Example 4: SELECT with ORDER BY

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC;
```

### 📤 Output

| name | salary |
|------|-------:|
| Kiran | 50000 |
| Anu | 45000 |
| Arun | 40000 |
| Priya | 35000 |
| Ravi | 30000 |

---

## 🔹 Example 5: SELECT with LIMIT

```sql
SELECT *
FROM employees
LIMIT 3;
```

### 📤 Output

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 2 | Ravi | HR | 30000 |
| 3 | Kiran | IT | 50000 |

> 💡 `LIMIT` restricts the number of rows returned.

---

# 2️⃣ INSERT

## 🧠 What is INSERT?

`INSERT` is used to **add new rows** to a table.

### 📌 Syntax

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

---

## 🔹 Example 1: Insert a new employee

```sql
INSERT INTO employees (id, name, department, salary)
VALUES (6, 'Rahul', 'IT', 42000);
```

A new row is added:

| id | name | department | salary |
|---:|------|------------|-------:|
| 6 | Rahul | IT | 42000 |

---

## 🔹 Example 2: Insert multiple rows

We can insert multiple rows using one query.

```sql
INSERT INTO employees (id, name, department, salary)
VALUES
    (6, 'Rahul', 'IT', 42000),
    (7, 'Meena', 'HR', 38000),
    (8, 'Vijay', 'Sales', 41000);
```

### 📤 New Rows

| id | name | department | salary |
|---:|------|------------|-------:|
| 6 | Rahul | IT | 42000 |
| 7 | Meena | HR | 38000 |
| 8 | Vijay | Sales | 41000 |

---

## 🔹 INSERT Without Specifying All Columns

If some columns have default values or allow `NULL`, you may insert only the required columns.

```sql
INSERT INTO employees (name, department)
VALUES ('Sanjay', 'IT');
```

> ⚠️ This works only when omitted columns can accept `NULL` or have a default value.

---

## ⭐ INSERT Important Point

Always make sure the values match the column order.

```sql
INSERT INTO employees (name, department, salary)
VALUES ('Rahul', 'IT', 42000);
```

Here:

```text
name       → Rahul
department → IT
salary     → 42000
```

---

# 3️⃣ UPDATE

## 🧠 What is UPDATE?

`UPDATE` is used to **modify existing data** in a table.

### 📌 Syntax

```sql
UPDATE table_name
SET column_name = new_value
WHERE condition;
```

---

## 🔹 Example 1: Update one employee

Change Arun's salary to `45000`.

```sql
UPDATE employees
SET salary = 45000
WHERE name = 'Arun';
```

### Before

| name | salary |
|------|-------:|
| Arun | 40000 |

### After

| name | salary |
|------|-------:|
| Arun | 45000 |

---

## 🔹 Example 2: Update using ID

Using a unique ID is usually safer than using a name.

```sql
UPDATE employees
SET salary = 50000
WHERE id = 1;
```

---

## 🔹 Example 3: Update Multiple Columns

We can update multiple columns at once.

```sql
UPDATE employees
SET salary = 45000,
    department = 'IT'
WHERE id = 1;
```

---

## 🔹 Example 4: Increase Salary

We can also update a value based on its existing value.

Increase everyone's salary by `5000`.

```sql
UPDATE employees
SET salary = salary + 5000;
```

> ⚠️ This updates **every row** because there is no `WHERE` condition.

---

# ⚠️ UPDATE Without WHERE

Be careful!

```sql
UPDATE employees
SET salary = 50000;
```

This changes the salary of **every employee**.

```text
❌ No WHERE
     ↓
All rows affected
```

### ⭐ Best Practice

First check the rows:

```sql
SELECT *
FROM employees
WHERE id = 1;
```

Then update:

```sql
UPDATE employees
SET salary = 45000
WHERE id = 1;
```

---

# 4️⃣ DELETE

## 🧠 What is DELETE?

`DELETE` is used to **remove rows** from a table.

### 📌 Syntax

```sql
DELETE FROM table_name
WHERE condition;
```

---

## 🔹 Example 1: Delete one employee

Delete employee with `id = 5`.

```sql
DELETE FROM employees
WHERE id = 5;
```

The employee's row will be removed.

---

## 🔹 Example 2: Delete based on a condition

Delete employees from the HR department.

```sql
DELETE FROM employees
WHERE department = 'HR';
```

All rows matching the condition will be deleted.

---

# ⚠️ DELETE Without WHERE

Be very careful!

```sql
DELETE FROM employees;
```

This removes **all rows** from the table.

```text
DELETE FROM employees
        ↓
    No WHERE
        ↓
ALL ROWS DELETED ❌
```

> 🚨 **Interview/Practical Tip:** Always verify the `WHERE` condition before running `DELETE`.

---

# 🔥 Combining SELECT with UPDATE & DELETE

`SELECT` is useful for checking the data before modifying it.

### Step 1️⃣ Check the row

```sql
SELECT *
FROM employees
WHERE id = 1;
```

### Step 2️⃣ Update the row

```sql
UPDATE employees
SET salary = 45000
WHERE id = 1;
```

### Step 3️⃣ Verify the update

```sql
SELECT *
FROM employees
WHERE id = 1;
```

The same idea can be used before `DELETE`.

```sql
SELECT *
FROM employees
WHERE id = 5;
```

Then:

```sql
DELETE FROM employees
WHERE id = 5;
```

---

# 🧠 Quick Revision

| Command | Purpose | Easy Word |
|---------|---------|-----------|
| `SELECT` | Retrieve data | READ 👀 |
| `INSERT` | Add new rows | ADD ➕ |
| `UPDATE` | Modify existing rows | CHANGE ✏️ |
| `DELETE` | Remove rows | REMOVE 🗑️ |

---

# 🎯 Easy Memory Trick

```text
SELECT → READ 👀

INSERT → ADD ➕

UPDATE → CHANGE ✏️

DELETE → REMOVE 🗑️
```

---

# ⭐ Interview One-Liners

### SELECT

> `SELECT` is used to retrieve data from one or more tables.

### INSERT

> `INSERT` is used to add new rows into a table.

### UPDATE

> `UPDATE` is used to modify existing rows in a table.

### DELETE

> `DELETE` is used to remove rows from a table based on a condition.

---

# 🚀 Important Difference

## DELETE vs TRUNCATE vs DROP

These commands are often asked together in interviews.

| Command | Removes | WHERE Allowed? | Table Structure |
|---------|---------|----------------|-----------------|
| `DELETE` | Selected rows or all rows | ✅ Yes | Remains |
| `TRUNCATE` | All rows | ❌ No | Remains |
| `DROP` | Entire table | ❌ No | Removed |

---

## 🔹 DELETE

```sql
DELETE FROM employees
WHERE id = 5;
```

Removes selected rows.

---

## 🔹 TRUNCATE

```sql
TRUNCATE TABLE employees;
```

Removes all rows but keeps the table structure.

---

## 🔹 DROP

```sql
DROP TABLE employees;
```

Removes the table itself.

```text
DELETE
  ↓
Remove selected rows 🗑️

TRUNCATE
  ↓
Remove all rows 🧹

DROP
  ↓
Remove table 💥
```

---

# 📝 Practice Questions

### Q1️⃣ SELECT

Display all employees.

---

### Q2️⃣ SELECT

Display only:

```text
name
salary
```

---

### Q3️⃣ SELECT + WHERE

Find employees working in the `IT` department.

---

### Q4️⃣ SELECT + ORDER BY

Display employees in descending order of salary.

---

### Q5️⃣ INSERT

Insert this employee:

```text
id = 6
name = Rahul
department = IT
salary = 42000
```

---

### Q6️⃣ INSERT

Insert multiple employees:

```text
7, Meena, HR, 38000
8, Vijay, Sales, 41000
```

---

### Q7️⃣ UPDATE

Change Arun's salary to `50000`.

---

### Q8️⃣ UPDATE

Change employee `id = 2` department to `IT`.

---

### Q9️⃣ UPDATE

Increase the salary of all IT employees by `5000`.

---

### Q🔟 DELETE

Delete the employee whose `id = 5`.

---

### Q1️⃣1️⃣ DELETE

Delete all employees from the `Sales` department.

---

### Q1️⃣2️⃣ 🔥 Combined

Write queries to:

```text
1. Display employees from IT
2. Increase their salary by 5000
3. Display the updated data
```

---

# 🏆 Final Cheat Sheet

```text
SELECT
→ Retrieve data
→ READ 👀

INSERT
→ Add new rows
→ ADD ➕

UPDATE
→ Modify existing rows
→ CHANGE ✏️

DELETE
→ Remove rows
→ REMOVE 🗑️
```

---

> ⭐ **Remember:** `UPDATE` or `DELETE` without a `WHERE` clause can affect every row in the table.
