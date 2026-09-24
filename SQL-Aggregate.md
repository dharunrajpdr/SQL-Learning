# 📊 SQL Aggregate Functions

> 💡 **In this lesson:** Learn `COUNT()`, `SUM()`, `AVG()`, `MAX()`, and `MIN()` with simple examples.

---

# 1️⃣ What are Aggregate Functions?

## 🧠 Definition

**Aggregate functions** perform a calculation on **multiple rows** and return **a single result**.

For example:

```text
10
20
30
40
50
 ↓
SUM()
 ↓
150
```

### 📌 Main Aggregate Functions

| Function | Purpose |
|----------|---------|
| `COUNT()` | 🔢 Counts rows |
| `SUM()` | ➕ Calculates total |
| `AVG()` | 📊 Calculates average |
| `MAX()` | ⬆️ Finds highest value |
| `MIN()` | ⬇️ Finds lowest value |

---

# 2️⃣ Example Table

We'll use this `employees` table for all examples.

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 2 | Ravi | HR | 30000 |
| 3 | Kiran | IT | 50000 |
| 4 | Priya | HR | 35000 |
| 5 | Anu | Sales | 45000 |

---

# 3️⃣ COUNT()

## 🧠 What is COUNT()?

`COUNT()` is used to **count rows or non-NULL values**.

### 📌 Syntax

```sql
SELECT COUNT(column_name)
FROM table_name;
```

---

## 🔹 Example 1: Count all employees

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

### 📤 Output

| total_employees |
|----------------:|
| 5 |

### 🧠 Why `COUNT(*)`?

```text
COUNT(*)
   ↓
Counts every row
```

---

## 🔹 Example 2: Count employee IDs

```sql
SELECT COUNT(id) AS total_employees
FROM employees;
```

### 📤 Output

| total_employees |
|----------------:|
| 5 |

> ⭐ `COUNT(*)` counts rows, while `COUNT(column)` counts non-NULL values in that column.

---

# 4️⃣ SUM()

## 🧠 What is SUM()?

`SUM()` calculates the **total of a numeric column**.

### 📌 Syntax

```sql
SELECT SUM(column_name)
FROM table_name;
```

---

## 🔹 Example

Find the total salary of all employees.

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

### 📤 Output

| total_salary |
|-------------:|
| 200000 |

### 🧮 Calculation

```text
40000
+ 30000
+ 50000
+ 35000
+ 45000
────────
200000
```

> ⚠️ `SUM()` is normally used with numeric columns.

---

# 5️⃣ AVG()

## 🧠 What is AVG()?

`AVG()` calculates the **average value** of a numeric column.

### 📌 Syntax

```sql
SELECT AVG(column_name)
FROM table_name;
```

---

## 🔹 Example

Find the average employee salary.

```sql
SELECT AVG(salary) AS average_salary
FROM employees;
```

### 📤 Output

| average_salary |
|---------------:|
| 40000 |

### 🧮 Calculation

```text
Total salary = 200000
Employees    = 5

Average = 200000 / 5
        = 40000
```

---

# 6️⃣ MAX()

## 🧠 What is MAX()?

`MAX()` returns the **highest value** in a column.

### 📌 Syntax

```sql
SELECT MAX(column_name)
FROM table_name;
```

---

## 🔹 Example

Find the highest salary.

```sql
SELECT MAX(salary) AS highest_salary
FROM employees;
```

### 📤 Output

| highest_salary |
|---------------:|
| 50000 |

---

# 7️⃣ MIN()

## 🧠 What is MIN()?

`MIN()` returns the **lowest value** in a column.

### 📌 Syntax

```sql
SELECT MIN(column_name)
FROM table_name;
```

---

## 🔹 Example

Find the lowest salary.

```sql
SELECT MIN(salary) AS lowest_salary
FROM employees;
```

### 📤 Output

| lowest_salary |
|--------------:|
| 30000 |

---

# 8️⃣ Using Multiple Aggregate Functions

We can use multiple aggregate functions in one query.

### 🔹 Example

```sql
SELECT
    COUNT(*) AS total_employees,
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```

### 📤 Output

| total_employees | total_salary | average_salary | highest_salary | lowest_salary |
|----------------:|-------------:|---------------:|---------------:|--------------:|
| 5 | 200000 | 40000 | 50000 | 30000 |

### 🧠 One Query → Multiple Calculations

```text
COUNT() → How many?
SUM()   → How much total?
AVG()   → What is the average?
MAX()   → What is the highest?
MIN()   → What is the lowest?
```

---

# 9️⃣ Aggregate Functions with WHERE

We can use `WHERE` to filter rows before calculating.

### 🔹 Example

Find the average salary of IT employees.

```sql
SELECT AVG(salary) AS average_salary
FROM employees
WHERE department = 'IT';
```

### 📤 Output

| average_salary |
|---------------:|
| 45000 |

Because:

```text
IT employees:

Arun  → 40000
Kiran → 50000

Average = (40000 + 50000) / 2
        = 45000
```

### 🔄 Query Flow

```text
employees
    ↓
WHERE department = 'IT'
    ↓
IT employees only
    ↓
AVG(salary)
    ↓
45000
```

---

# 🔟 Aggregate Functions + GROUP BY

This is one of the **most important combinations in SQL**.

### 🔹 Example

Find the average salary of each department.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

### 📤 Output

| department | average_salary |
|------------|---------------:|
| IT | 45000 |
| HR | 32500 |
| Sales | 45000 |

### 🧠 What happens?

```text
GROUP BY department
        ↓
Create groups
        ↓
IT → 40000, 50000
HR → 30000, 35000
Sales → 45000
        ↓
AVG()
        ↓
Calculate average for each group
```

---

# 1️⃣1️⃣ COUNT() + GROUP BY

### 🔹 Example

Find the number of employees in each department.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |
| Sales | 1 |

---

# 1️⃣2️⃣ SUM() + GROUP BY

### 🔹 Example

Find the total salary paid by each department.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

### 📤 Output

| department | total_salary |
|------------|-------------:|
| IT | 90000 |
| HR | 65000 |
| Sales | 45000 |

---

# 1️⃣3️⃣ MAX() + GROUP BY

### 🔹 Example

Find the highest salary in each department.

```sql
SELECT
    department,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department;
```

### 📤 Output

| department | highest_salary |
|------------|---------------:|
| IT | 50000 |
| HR | 35000 |
| Sales | 45000 |

---

# 1️⃣4️⃣ MIN() + GROUP BY

### 🔹 Example

Find the lowest salary in each department.

```sql
SELECT
    department,
    MIN(salary) AS lowest_salary
FROM employees
GROUP BY department;
```

### 📤 Output

| department | lowest_salary |
|------------|--------------:|
| IT | 40000 |
| HR | 30000 |
| Sales | 45000 |

---

# 1️⃣5️⃣ Aggregate Functions + HAVING

`HAVING` is used to **filter groups after GROUP BY**.

### 🔹 Example

Find departments having more than 1 employee.

```sql
SELECT
    department,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |

### 🧠 WHERE vs HAVING

```text
WHERE
  ↓
Filters individual rows

GROUP BY
  ↓
Creates groups

HAVING
  ↓
Filters groups
```

### Example

```sql
SELECT department, COUNT(*) AS total
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING COUNT(*) >= 2;
```

> ⭐ **Interview Tip:**  
> `WHERE` filters rows **before grouping**.  
> `HAVING` filters groups **after grouping**.

---

# 1️⃣6️⃣ Aggregate Functions + ORDER BY

We can sort aggregate results.

### 🔹 Example

Find the total salary of each department and sort from highest to lowest.

```sql
SELECT
    department,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC;
```

### 📤 Output

| department | total_salary |
|------------|-------------:|
| IT | 90000 |
| HR | 65000 |
| Sales | 45000 |

---

# 1️⃣7️⃣ Aggregate Functions + GROUP BY + HAVING + ORDER BY

Now let's combine everything.

### 🔥 Example

Find departments where the average salary is greater than `35000`, and sort by average salary.

```sql
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 35000
ORDER BY average_salary DESC;
```

### 📤 Output

| department | average_salary |
|------------|---------------:|
| IT | 45000 |
| Sales | 45000 |

### 🔄 Query Flow

```text
FROM
 ↓
Get employees
 ↓
GROUP BY
 ↓
Create department groups
 ↓
AVG()
 ↓
Calculate average salary
 ↓
HAVING
 ↓
Keep average salary > 35000
 ↓
ORDER BY
 ↓
Sort result
```

---

# 1️⃣8️⃣ COUNT(*) vs COUNT(column)

This is an important interview question ⭐

### `COUNT(*)`

Counts **all rows**.

```sql
SELECT COUNT(*)
FROM employees;
```

### `COUNT(column)`

Counts only rows where that column is **NOT NULL**.

```sql
SELECT COUNT(department)
FROM employees;
```

### Example

Suppose:

| id | name | department |
|---:|------|------------|
| 1 | Arun | IT |
| 2 | Ravi | HR |
| 3 | Kiran | NULL |

Then:

```sql
COUNT(*)
```

returns:

```text
3
```

But:

```sql
COUNT(department)
```

returns:

```text
2
```

Because `COUNT(department)` ignores the `NULL` value.

---

# 🧠 Aggregate Functions Cheat Sheet

| Function | Meaning | Example |
|----------|---------|---------|
| `COUNT()` | 🔢 Count | Number of employees |
| `SUM()` | ➕ Total | Total salary |
| `AVG()` | 📊 Average | Average salary |
| `MAX()` | ⬆️ Highest | Highest salary |
| `MIN()` | ⬇️ Lowest | Lowest salary |

---

# 🎯 Easy Memory Trick

```text
COUNT → HOW MANY? 🔢

SUM   → HOW MUCH TOTAL? ➕

AVG   → WHAT IS THE AVERAGE? 📊

MAX   → WHAT IS THE HIGHEST? ⬆️

MIN   → WHAT IS THE LOWEST? ⬇️
```

---

# ⭐ Important Query Patterns

### Count all employees

```sql
SELECT COUNT(*)
FROM employees;
```

### Total salary

```sql
SELECT SUM(salary)
FROM employees;
```

### Average salary

```sql
SELECT AVG(salary)
FROM employees;
```

### Highest salary

```sql
SELECT MAX(salary)
FROM employees;
```

### Lowest salary

```sql
SELECT MIN(salary)
FROM employees;
```

### Average salary by department

```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```

### Departments with more than 1 employee

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

### Top 2 departments by total salary

```sql
SELECT department, SUM(salary) AS total_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC
LIMIT 2;
```

---

# 🚀 SQL Query Execution Order

This is very important for interviews.

A simplified logical order is:

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT
```

### 🧠 Example

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING AVG(salary) > 35000
ORDER BY avg_salary DESC
LIMIT 2;
```

Think:

```text
FROM      → Get table
WHERE     → Filter rows
GROUP BY  → Create groups
HAVING    → Filter groups
SELECT    → Select result
ORDER BY  → Sort result
LIMIT     → Take required rows
```

---

# 🎯 Interview One-Liners

### COUNT()

> `COUNT()` returns the number of rows or non-NULL values in a column.

### SUM()

> `SUM()` calculates the total of a numeric column.

### AVG()

> `AVG()` calculates the average of a numeric column.

### MAX()

> `MAX()` returns the highest value from a column.

### MIN()

> `MIN()` returns the lowest value from a column.

---
