# 📌 SQL — CONSTRAINTS

## 1. What is a Constraint?

A **Constraint** is a rule applied to a table column to control the type of data that can be stored.

👉 Constraints help maintain **data accuracy and integrity**.

### Easy Memory Trick

```text
CONSTRAINT = Rule for data
```

---

# 2. Types of Constraints

The main SQL constraints are:

| Constraint | Purpose |
|------------|---------|
| `NOT NULL` | Prevents NULL values |
| `UNIQUE` | Prevents duplicate values |
| `PRIMARY KEY` | Uniquely identifies each row |
| `FOREIGN KEY` | Connects two tables |
| `CHECK` | Validates a condition |
| `DEFAULT` | Provides a default value |

---

# 3. NOT NULL

`NOT NULL` ensures that a column **cannot contain NULL**.

### Example

```sql
CREATE TABLE employees (
    id INT,
    name VARCHAR(50) NOT NULL
);
```

Now:

```sql
INSERT INTO employees
VALUES (1, 'Arun');
```

✅ Valid

But:

```sql
INSERT INTO employees
VALUES (2, NULL);
```

❌ Error

### Easy Memory Trick

```text
NOT NULL → Value is required
```

---

# 4. UNIQUE

`UNIQUE` ensures that values in a column are **not duplicated**.

```sql
CREATE TABLE employees (
    id INT,
    email VARCHAR(100) UNIQUE
);
```

Valid:

```text
arun@gmail.com
bala@gmail.com
```

Invalid:

```text
arun@gmail.com
arun@gmail.com ❌
```

👉 `UNIQUE` is commonly used for email, username, phone number, etc.

---

# 5. PRIMARY KEY ⭐

A `PRIMARY KEY` uniquely identifies each row in a table.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT
);
```

### Primary Key Rules

```text
PRIMARY KEY
     ↓
Unique
     +
Cannot be NULL
```

Example:

| id | name |
|----|------|
| 1 | Arun |
| 2 | Bala |
| 3 | David |

❌ Duplicate:

```sql
INSERT INTO employees
VALUES (1, 'Ravi');
```

❌ NULL:

```sql
INSERT INTO employees
VALUES (NULL, 'Ravi');
```

---

# 6. FOREIGN KEY ⭐

A `FOREIGN KEY` creates a relationship between two tables.

### Departments

```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50)
);
```

### Employees

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    dept_id INT,
    FOREIGN KEY (dept_id)
        REFERENCES departments(dept_id)
);
```

Here:

```text
departments.dept_id
        ↑
        |
employees.dept_id
```

👉 `employees.dept_id` refers to `departments.dept_id`.

---

# 7. Foreign Key Example

Departments:

| dept_id | dept_name |
|---------|-----------|
| 10 | IT |
| 20 | HR |

Employee:

```sql
INSERT INTO employees
VALUES (1, 'Arun', 10);
```

✅ Valid because department `10` exists.

But:

```sql
INSERT INTO employees
VALUES (2, 'Bala', 50);
```

❌ Invalid because department `50` does not exist.

---

# 8. CHECK ⭐

`CHECK` ensures that a condition is satisfied.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary INT CHECK (salary > 0)
);
```

Valid:

```sql
INSERT INTO employees
VALUES (1, 'Arun', 50000);
```

❌ Invalid:

```sql
INSERT INTO employees
VALUES (2, 'Bala', -1000);
```

Because:

```text
salary > 0
```

is not satisfied.

---

# 9. DEFAULT

`DEFAULT` provides a value automatically when no value is supplied.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50) DEFAULT 'IT'
);
```

Now:

```sql
INSERT INTO employees (id, name)
VALUES (1, 'Arun');
```

The department automatically becomes:

```text
IT
```

### Output

| id | name | department |
|----|------|------------|
| 1 | Arun | IT |

---

# 10. Multiple Constraints

A column can have multiple constraints.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary INT CHECK (salary > 0),
    department VARCHAR(50) DEFAULT 'IT'
);
```

Here:

```text
id         → PRIMARY KEY
name       → NOT NULL
email      → UNIQUE
salary     → CHECK
department → DEFAULT
```

---

# 11. Column-Level Constraint

A constraint can be defined directly with a column.

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);
```

👉 This is called a **column-level constraint**.

---

# 12. Table-Level Constraint

A constraint can also be defined separately after the columns.

```sql
CREATE TABLE employees (
    id INT,
    name VARCHAR(50),
    email VARCHAR(100),

    CONSTRAINT pk_employee
        PRIMARY KEY (id),

    CONSTRAINT uq_employee_email
        UNIQUE (email)
);
```

👉 This is called a **table-level constraint**.

---

# 13. Naming Constraints

We can give constraints meaningful names.

```sql
CREATE TABLE employees (
    id INT,
    salary INT,

    CONSTRAINT pk_employee
        PRIMARY KEY (id),

    CONSTRAINT chk_salary
        CHECK (salary > 0)
);
```

Here:

```text
pk_employee → Primary key constraint
chk_salary  → Check constraint
```

👉 Naming constraints makes them easier to manage later.

---

# 14. Adding a Constraint

We can add constraints to an existing table using `ALTER TABLE`.

### Add UNIQUE

```sql
ALTER TABLE employees
ADD CONSTRAINT uq_email
UNIQUE (email);
```

### Add CHECK

```sql
ALTER TABLE employees
ADD CONSTRAINT chk_salary
CHECK (salary > 0);
```

### Add FOREIGN KEY

```sql
ALTER TABLE employees
ADD CONSTRAINT fk_department
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id);
```

---

# 15. Dropping a Constraint

Syntax varies slightly by database.

Example:

```sql
ALTER TABLE employees
DROP CONSTRAINT chk_salary;
```

For MySQL, a foreign key is commonly removed using:

```sql
ALTER TABLE employees
DROP FOREIGN KEY fk_department;
```

👉 Always check the syntax for the specific database system.

---

# 16. PRIMARY KEY vs UNIQUE ⭐

| Feature | PRIMARY KEY | UNIQUE |
|---------|-------------|--------|
| Duplicate values | ❌ | ❌ |
| NULL allowed | ❌ | DBMS-dependent |
| Number per table | Usually one | Multiple |
| Identifies row | ✅ | Not necessarily |
| Used for relationships | Commonly | Can also be referenced in suitable DBMS designs |

### Easy Memory Trick

```text
PRIMARY KEY → Main identity of the row

UNIQUE       → No duplicate values
```

---

# 17. PRIMARY KEY vs FOREIGN KEY

```text
PRIMARY KEY
     ↓
Identifies a row in its own table

FOREIGN KEY
     ↓
Refers to a key in another table
```

Example:

```text
Departments
-----------
dept_id ← PRIMARY KEY

Employees
---------
dept_id ← FOREIGN KEY
```

---

# 18. NOT NULL vs DEFAULT

### NOT NULL

Requires a value:

```sql
name VARCHAR(50) NOT NULL
```

### DEFAULT

Provides a value if one isn't supplied:

```sql
department VARCHAR(50) DEFAULT 'IT'
```

They can also be used together:

```sql
department VARCHAR(50)
DEFAULT 'IT'
NOT NULL
```

---

# 19. Constraints Example ⭐

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary INT CHECK (salary > 0),
    department VARCHAR(50) DEFAULT 'IT'
);
```

### Constraint Summary

```text
id
 ↓
PRIMARY KEY
 ↓
Unique + Not NULL

name
 ↓
NOT NULL
 ↓
Value required

email
 ↓
UNIQUE
 ↓
No duplicates

salary
 ↓
CHECK
 ↓
Must satisfy condition

department
 ↓
DEFAULT
 ↓
Automatic value if omitted
```

---

# 20. Referential Integrity ⭐

Foreign keys help maintain **referential integrity**.

Example:

```text
Department
10 → IT
20 → HR

Employee
101 → 10
102 → 20
```

The employee's `dept_id` should refer to a valid department key, subject to the foreign-key rules.

This prevents invalid relationships such as:

```text
Employee → dept_id = 99
```

when department `99` doesn't exist.

---

# 21. ON DELETE and ON UPDATE

Foreign keys can define what happens when the referenced row changes.

### CASCADE

```sql
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id)
ON DELETE CASCADE;
```

If a department is deleted, related employee rows may also be deleted.

### SET NULL

```sql
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id)
ON DELETE SET NULL;
```

The employee's `dept_id` becomes `NULL` when the referenced department is deleted, assuming the column allows `NULL`.

### Common Actions

| Action | Meaning |
|--------|---------|
| `CASCADE` | Apply the change to related rows |
| `SET NULL` | Set foreign key to NULL |
| `RESTRICT` | Prevent the operation |
| `NO ACTION` | Depends on DBMS and constraint checking rules |

⚠️ Exact behavior and supported options vary by database.

---

# 🎯 Quick Revision

```text
NOT NULL
→ Value required

UNIQUE
→ No duplicate values

PRIMARY KEY
→ Unique row identity

FOREIGN KEY
→ Relationship between tables

CHECK
→ Condition must be satisfied

DEFAULT
→ Automatic value
```

---

# 💡 Interview One-Liners

### What is a Constraint?

> A constraint is a rule applied to table data to maintain data integrity and consistency.

### What is a Primary Key?

> A primary key uniquely identifies each row and cannot contain NULL values.

### What is a Foreign Key?

> A foreign key is a column or set of columns that references a key in another table.

### Difference between PRIMARY KEY and UNIQUE?

> A primary key identifies the row and cannot be NULL, while a UNIQUE constraint prevents duplicate values and can have multiple constraints in a table.

### What is NOT NULL?

> NOT NULL ensures that a column must have a value.

### What is CHECK?

> CHECK ensures that inserted or updated values satisfy a specified condition.

### What is DEFAULT?

> DEFAULT provides a predefined value when a value is not explicitly supplied.

---

# 📝 Practice Questions

### Q1.
Create an `employees` table with:

```text
id       → PRIMARY KEY
name     → NOT NULL
email    → UNIQUE
salary   → CHECK > 0
city     → DEFAULT 'Karur'
```

### Q2.
Create a `departments` table and connect it with `employees` using a FOREIGN KEY.

### Q3.
Add a UNIQUE constraint to an existing column.

### Q4.
Add a CHECK constraint to ensure salary is greater than `10000`.

### Q5.
Explain the difference between PRIMARY KEY and FOREIGN KEY.

### Q6.
What happens when `ON DELETE CASCADE` is used?

---

# ⭐ Key Point

```text
CONSTRAINTS = Rules for maintaining correct data

NOT NULL    → Required
UNIQUE      → No duplicates
PRIMARY KEY → Row identity
FOREIGN KEY  → Table relationship
CHECK       → Validation
DEFAULT     → Automatic value
```
