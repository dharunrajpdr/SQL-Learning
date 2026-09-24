# 📌 SQL — INDEXES

## 1. What is an Index?

An **Index** is a database object used to **speed up data retrieval** from a table.

👉 Think of an index like the **index page of a book**.

Instead of checking every page, you can directly find the required topic.

### Easy Memory Trick

```text
WITHOUT INDEX → Search many rows
WITH INDEX    → Find data faster
```

---

# 2. Why Do We Need Indexes?

Suppose a table contains **1 million employees**.

```sql
SELECT *
FROM employees
WHERE email = 'arun@gmail.com';
```

Without a suitable index, the database may need to check many rows.

If `email` has an index:

```sql
CREATE INDEX idx_employee_email
ON employees(email);
```

The database can use the index to locate matching rows more efficiently.

---

# 3. Creating an Index

### Syntax

```sql
CREATE INDEX index_name
ON table_name(column_name);
```

### Example

```sql
CREATE INDEX idx_employee_name
ON employees(name);
```

Now the database has an index on the `name` column.

---

# 4. Index on Multiple Columns

We can create a **composite index** using multiple columns.

```sql
CREATE INDEX idx_dept_salary
ON employees(department, salary);
```

This is called a:

> **Composite Index / Multi-column Index**

---

# 5. Unique Index

A unique index prevents duplicate values in the indexed column.

```sql
CREATE UNIQUE INDEX idx_employee_email
ON employees(email);
```

Now duplicate email values are not allowed.

Example:

```text
arun@gmail.com   ✅
bala@gmail.com   ✅
arun@gmail.com   ❌
```

⚠️ Exact behavior regarding `NULL` values can vary by database system.

---

# 6. Index and PRIMARY KEY

When we create a primary key:

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT
);
```

Most database systems automatically create an index associated with the primary key.

So we normally don't need to manually create another index on the same primary-key column.

---

# 7. Index and UNIQUE Constraint

Example:

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);
```

A `UNIQUE` constraint is typically supported using a unique index internally, depending on the database system.

---

# 8. Dropping an Index

If an index is no longer required, we can remove it.

### MySQL

```sql
DROP INDEX idx_employee_name
ON employees;
```

### PostgreSQL

```sql
DROP INDEX idx_employee_name;
```

👉 Syntax can vary between database systems.

---

# 9. When Should We Use Indexes?

Indexes are especially useful for columns frequently used in:

### WHERE

```sql
SELECT *
FROM employees
WHERE email = 'arun@gmail.com';
```

### JOIN

```sql
SELECT *
FROM employees e
JOIN departments d
ON e.dept_id = d.dept_id;
```

### ORDER BY

```sql
SELECT *
FROM employees
ORDER BY salary;
```

### Searching

```sql
SELECT *
FROM employees
WHERE name = 'Arun';
```

---

# 10. Disadvantages of Indexes ⚠️

Indexes improve read performance, but they also have costs.

### INSERT

```sql
INSERT INTO employees ...
```

### UPDATE

```sql
UPDATE employees
SET salary = 50000;
```

### DELETE

```sql
DELETE FROM employees
WHERE id = 10;
```

When table data changes, related indexes may also need to be updated.

Therefore:

```text
More Indexes
     ↓
Faster Reads
     +
More Storage
     +
Potentially Slower Writes
```

---

# 11. Index vs No Index

### Without Index

```text
Database
   ↓
Check Row 1
   ↓
Check Row 2
   ↓
Check Row 3
   ↓
...
   ↓
Find matching row
```

### With Index

```text
Query
  ↓
Index
  ↓
Find matching location
  ↓
Retrieve required row
```

👉 The exact execution depends on the database optimizer and query.

---

# 12. Composite Index ⭐

Consider:

```sql
CREATE INDEX idx_dept_salary
ON employees(department, salary);
```

The **column order matters**.

This index is generally most useful when queries filter using the leading column:

```sql
WHERE department = 'IT'
```

or:

```sql
WHERE department = 'IT'
AND salary > 50000
```

But a query using only:

```sql
WHERE salary > 50000
```

may not be able to use this composite index as effectively.

### Easy Memory Trick

```text
INDEX(department, salary)

First  → department
Second → salary
```

👉 Remember the **leftmost/leading column** concept.

---

# 13. Index Example

Suppose:

```sql
CREATE INDEX idx_salary
ON employees(salary);
```

Query:

```sql
SELECT name
FROM employees
WHERE salary = 60000;
```

The database may choose the salary index to find matching rows.

⚠️ The database optimizer decides whether using the index is actually beneficial.

---

# 14. Index Does NOT Always Make Queries Faster

An index is not automatically beneficial for every query.

For example, if a table has only a few rows, scanning the entire table may be cheaper.

Also, columns with very few distinct values may not always benefit much from a normal index.

Example:

```text
gender
------
M
F
M
F
M
F
```

There are very few distinct values.

👉 Index usefulness depends on the **data, query, table size, and database optimizer**.

---

# 15. Index vs Primary Key

| Feature | Index | Primary Key |
|---------|-------|-------------|
| Purpose | Improve retrieval | Identify each row |
| Can have duplicates | Usually yes | ❌ No |
| Can be NULL | Depends on definition | ❌ No |
| Multiple per table | ✅ Yes | ❌ Usually one primary key |
| Automatically indexed | Not necessarily | Typically yes |

---

# 16. Index vs UNIQUE Index

### Normal Index

```sql
CREATE INDEX idx_name
ON employees(name);
```

👉 Mainly helps with data retrieval.

Duplicate names are allowed.

```text
Arun
Arun
Bala
```

### Unique Index

```sql
CREATE UNIQUE INDEX idx_email
ON employees(email);
```

👉 Helps with retrieval and enforces uniqueness.

```text
arun@gmail.com   ✅
bala@gmail.com   ✅
arun@gmail.com   ❌
```

---

# 17. Types of Indexes

Common index concepts include:

| Type | Meaning |
|------|---------|
| Single-column index | Index on one column |
| Composite index | Index on multiple columns |
| Unique index | Prevents duplicate indexed values |
| Primary key index | Index associated with primary key |
| Clustered index | Determines/organizes physical row storage in some DBMSs |
| Non-clustered index | Separate index structure pointing to table rows |

⚠️ **Clustered vs non-clustered indexes are database-specific concepts.**  
For example, PostgreSQL's indexing/storage model differs from SQL Server's.

---

# 18. EXPLAIN and Indexes ⭐

We can inspect how a database plans to execute a query.

Example in MySQL:

```sql
EXPLAIN
SELECT *
FROM employees
WHERE email = 'arun@gmail.com';
```

Example in PostgreSQL:

```sql
EXPLAIN
SELECT *
FROM employees
WHERE email = 'arun@gmail.com';
```

👉 `EXPLAIN` helps us understand whether an index or another access method may be used.

---

# 19. Important Rule ⚠️

Don't create indexes on **every column**.

Instead, consider columns frequently used for:

```text
WHERE
JOIN
ORDER BY
GROUP BY
UNIQUE constraints
```

and verify the actual workload and query plans.

---

# 20. Query Flow

```text
SQL Query
    ↓
Database Optimizer
    ↓
Choose execution plan
    ↓
Index Scan / Table Scan / Other Method
    ↓
Return Result
```

👉 The optimizer decides whether an index should be used.

---

# 🎯 Quick Revision

```text
INDEX
  ↓
Improves data retrieval
  ↓
Faster reads in suitable queries
  ↓
Uses extra storage
  ↓
Can add overhead to INSERT/UPDATE/DELETE
```

### Important Commands

```sql
CREATE INDEX
CREATE UNIQUE INDEX
DROP INDEX
EXPLAIN
```

---

# 💡 Interview One-Liners

### What is an Index?

> An index is a database structure used to improve the speed of data retrieval.

### Why do we use indexes?

> We use indexes to make suitable search, filtering, joining, and sorting operations more efficient.

### What is a Composite Index?

> A composite index is an index created on multiple columns.

### Does an index always improve performance?

> No. The database optimizer decides whether using an index is beneficial for a particular query.

### What is a disadvantage of indexes?

> Indexes require additional storage and can add overhead to insert, update, and delete operations.

### What is a Unique Index?

> A unique index prevents duplicate values in the indexed key while also supporting efficient lookups.

### What is EXPLAIN?

> EXPLAIN shows the database's planned execution strategy for a query and helps analyze index usage and performance.

---

# 📝 Practice Questions

### Q1.
Create an index on the `name` column.

### Q2.
Create a unique index on the `email` column.

### Q3.
Create a composite index on `department` and `salary`.

### Q4.
Write a query to remove an index.

### Q5.
Use `EXPLAIN` to analyze a query that searches employees by email.

### Q6.
Why should we not create indexes on every column?

---

# ⭐ Key Point

```text
INDEX = Faster Data Retrieval

CREATE INDEX       → Create index
UNIQUE INDEX       → Prevent duplicate indexed values
COMPOSITE INDEX    → Multiple columns
DROP INDEX         → Remove index
EXPLAIN            → Analyze query plan
```

But remember:

> **More indexes ≠ Always better performance.**
