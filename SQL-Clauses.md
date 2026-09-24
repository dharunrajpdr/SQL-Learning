# 📘 SQL Clauses – GROUP BY, HAVING, ORDER BY, LIMIT & OFFSET

> 💡 **In this lesson:** Learn how to group, filter groups, sort, limit, and skip SQL query results.

---

# 1️⃣ GROUP BY

## 🧠 What is GROUP BY?

`GROUP BY` is used to **group rows that have the same value** in one or more columns.

It is commonly used with **aggregate functions**:

| Function | Purpose |
|----------|---------|
| `COUNT()` | 🔢 Counts rows |
| `SUM()` | ➕ Calculates total |
| `AVG()` | 📊 Calculates average |
| `MAX()` | ⬆️ Finds maximum |
| `MIN()` | ⬇️ Finds minimum |

### 📌 Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name;
```

---

## 📋 Example Table

Suppose we have an `employees` table:

| id | name | department | salary |
|---:|------|------------|-------:|
| 1 | Arun | IT | 40000 |
| 2 | Ravi | HR | 30000 |
| 3 | Kiran | IT | 50000 |
| 4 | Priya | HR | 35000 |
| 5 | Anu | Sales | 45000 |

---

## 🔹 Example 1: Count Employees in Each Department

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |
| Sales | 1 |

### 🔍 How it works

```text
IT    → Arun, Kiran   → 2 employees
HR    → Ravi, Priya   → 2 employees
Sales → Anu           → 1 employee
```

---

## 🔹 Example 2: Find Average Salary of Each Department

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

### 📤 Output

| department | average_salary |
|------------|---------------:|
| IT | 45000 |
| HR | 32500 |
| Sales | 45000 |

> ⭐ **Key Point:**  
> `GROUP BY` creates groups, and aggregate functions perform calculations on each group.

---

# 2️⃣ HAVING

## 🧠 What is HAVING?

`HAVING` is used to **filter groups after `GROUP BY`**.

👉 `WHERE` filters **individual rows**.

👉 `HAVING` filters **groups**.

### 💡 Easy Memory Trick

```text
WHERE  → Filter rows
HAVING → Filter groups
```

---

## 📌 Syntax

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

---

## 🔹 Example 1: Departments with More Than 1 Employee

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |

`Sales` is not included because it has only 1 employee.

---

## 🔹 Example 2: Departments with Average Salary Greater Than 40000

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 40000;
```

### 📤 Output

| department | average_salary |
|------------|---------------:|
| IT | 45000 |
| Sales | 45000 |

---

## 🔹 Example 3: GROUP BY + WHERE + HAVING

We can use both `WHERE` and `HAVING`.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING AVG(salary) > 40000;
```

### 🧠 Query Flow

```text
WHERE
  ↓
Filter individual rows
  ↓
GROUP BY
  ↓
Create groups
  ↓
AVG()
  ↓
Calculate average
  ↓
HAVING
  ↓
Filter groups
```

---

# 3️⃣ WHERE vs HAVING ⭐

| WHERE | HAVING |
|-------|--------|
| Filters rows | Filters groups |
| Applied before `GROUP BY` | Applied after `GROUP BY` |
| Usually used for individual row conditions | Usually used with aggregate conditions |
| Cannot normally use aggregate functions directly | Commonly used with aggregate functions |

### Example

```sql
-- WHERE → Filter rows
SELECT *
FROM employees
WHERE salary > 40000;
```

```sql
-- HAVING → Filter groups
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 40000;
```

### 🧠 Remember

```text
WHERE  → Before GROUP BY
HAVING → After GROUP BY
```

---

# 4️⃣ ORDER BY

## 🧠 What is ORDER BY?

`ORDER BY` is used to **sort the result**.

We can sort in two ways:

- ⬆️ `ASC` → Ascending
- ⬇️ `DESC` → Descending

### 📌 Syntax

```sql
SELECT column_name
FROM table_name
ORDER BY column_name ASC;
```

> 💡 `ASC` is the default, so `ORDER BY salary` also means ascending order.

---

## 🔹 Example 1: Sort Employees by Salary

```sql
SELECT name, salary
FROM employees
ORDER BY salary ASC;
```

### 📤 Output

| name | salary |
|------|-------:|
| Ravi | 30000 |
| Priya | 35000 |
| Arun | 40000 |
| Anu | 45000 |
| Kiran | 50000 |

---

## 🔹 Example 2: Descending Order

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

## 🔹 Sorting Using Multiple Columns

```sql
SELECT *
FROM employees
ORDER BY department ASC, salary DESC;
```

### 🧠 Meaning

```text
1️⃣ First → Sort by department
2️⃣ Then → If department is same,
           sort salary from HIGH → LOW
```

> ⭐ **Key Point:**  
> `ORDER BY` controls the **order in which rows appear**.

---

# 5️⃣ LIMIT

## 🧠 What is LIMIT?

`LIMIT` is used to **restrict the number of rows returned**.

### 📌 Syntax

```sql
SELECT *
FROM table_name
LIMIT number;
```

---

## 🔹 Example

```sql
SELECT *
FROM employees
LIMIT 3;
```

👉 Only **3 rows** will be returned.

---

## 🔹 Example: Get Top 3 Highest Salaries

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

### 📤 Output

| name | salary |
|------|-------:|
| Kiran | 50000 |
| Anu | 45000 |
| Arun | 40000 |

### 🧠 Remember

```text
ORDER BY → Decides which rows come first
LIMIT    → Decides how many rows to take
```

> ⭐ **Interview Tip:**  
> `ORDER BY + LIMIT` is commonly used to find **Top N records**.

---

# 6️⃣ OFFSET

## 🧠 What is OFFSET?

`OFFSET` is used to **skip a specific number of rows** before returning the result.

### 📌 Syntax

```sql
SELECT *
FROM table_name
LIMIT number
OFFSET number;
```

---

## 🔹 Example

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 2 OFFSET 2;
```

### 🧠 Meaning

```text
OFFSET 2
   ↓
Skip first 2 rows

LIMIT 2
   ↓
Take next 2 rows
```

### 📤 Output

| id | name |
|---:|------|
| 3 | Kiran |
| 4 | Priya |

> ⭐ **Easy Rule:**  
> `OFFSET` = **Skip** rows  
> `LIMIT` = **Take** rows

---

# 7️⃣ LIMIT + OFFSET

## 📄 Pagination

`LIMIT` and `OFFSET` are commonly used for **pagination**.

For example, suppose we display **2 employees per page**.

---

### 📄 Page 1

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 2 OFFSET 0;
```

```text
Rows → 1, 2
```

---

### 📄 Page 2

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 2 OFFSET 2;
```

```text
Rows → 3, 4
```

---

### 📄 Page 3

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 2 OFFSET 4;
```

```text
Rows → 5
```

### 📊 Pagination

```text
Page 1 → [1] [2]
Page 2 → [3] [4]
Page 3 → [5]
```

---

## 🧮 Pagination Formula

```text
OFFSET = (page_number - 1) × page_size
```

### Example

```text
page_number = 3
page_size   = 2
```

Therefore:

```text
OFFSET = (3 - 1) × 2
       = 4
```

Query:

```sql
SELECT *
FROM employees
ORDER BY id
LIMIT 2 OFFSET 4;
```

---

# 8️⃣ GROUP BY + HAVING + ORDER BY

We can combine all three.

## 🔹 Example

Find departments having more than 1 employee and sort them by employee count.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY employee_count DESC;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |

### 🔄 Query Flow

```text
GROUP BY
   ↓
Create department groups
   ↓
COUNT()
   ↓
Count employees
   ↓
HAVING
   ↓
Keep groups with count > 1
   ↓
ORDER BY
   ↓
Sort the remaining groups
```

---

# 9️⃣ GROUP BY + ORDER BY

We can combine `GROUP BY` and `ORDER BY`.

## 🔹 Example

Find the number of employees in each department and sort by employee count.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
ORDER BY employee_count DESC;
```

### 📤 Output

| department | employee_count |
|------------|---------------:|
| IT | 2 |
| HR | 2 |
| Sales | 1 |

---

# 🔟 GROUP BY + HAVING + ORDER BY + LIMIT

We can find the department with the highest number of employees among departments having more than 1 employee.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY employee_count DESC
LIMIT 1;
```

### 🔄 Query Flow

```text
GROUP BY
   ↓
Create department groups

COUNT()
   ↓
Count employees

HAVING
   ↓
Filter groups

ORDER BY
   ↓
Sort groups

LIMIT
   ↓
Take the first result
```

---

# 1️⃣1️⃣ ORDER BY + LIMIT + OFFSET

These three are very useful together.

### 🔹 Example

Get the **3rd and 4th highest-paid employees**:

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC
LIMIT 2 OFFSET 2;
```

### 🔄 Query Flow

```text
ORDER BY salary DESC
        ↓
Highest salary first
        ↓
OFFSET 2
        ↓
Skip first 2 employees
        ↓
LIMIT 2
        ↓
Take next 2 employees
```

### 📤 Result

| Position | Employee | Salary |
|---------:|----------|-------:|
| 3 | Arun | 40000 |
| 4 | Priya | 35000 |

---

# 1️⃣2️⃣ SQL Query Execution Order ⭐⭐⭐

This is very important for interviews.

Although we write SQL like this:

```sql
SELECT department, COUNT(*) AS total
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY total DESC
LIMIT 2;
```

The logical processing order is approximately:

```text
1️⃣ FROM
      ↓
2️⃣ WHERE
      ↓
3️⃣ GROUP BY
      ↓
4️⃣ HAVING
      ↓
5️⃣ SELECT
      ↓
6️⃣ ORDER BY
      ↓
7️⃣ LIMIT
```

### 🧠 Easy Memory

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

> ⭐ **Interview Tip:**  
> `WHERE` filters rows before grouping, while `HAVING` filters groups after grouping.

---

# 🧠 Quick Revision

| Clause | Meaning | Easy Word |
|--------|---------|-----------|
| `GROUP BY` | Groups similar rows | 🧩 GROUP |
| `HAVING` | Filters groups | 🔍 FILTER |
| `ORDER BY` | Sorts the result | 🔃 SORT |
| `LIMIT` | Restricts rows | 🎯 TAKE |
| `OFFSET` | Skips rows | ⏭️ SKIP |

### 💡 Easy Memory Trick

```text
GROUP BY → GROUP 🧩
HAVING   → FILTER 🔍
ORDER BY → SORT 🔃
LIMIT    → TAKE 🎯
OFFSET   → SKIP ⏭️
```

---

# 🎯 Interview One-Liners

### GROUP BY

> `GROUP BY` is used to group rows having the same values, usually with aggregate functions.

### HAVING

> `HAVING` is used to filter groups after `GROUP BY`.

### WHERE vs HAVING

> `WHERE` filters individual rows, while `HAVING` filters groups.

### ORDER BY

> `ORDER BY` is used to sort query results in ascending or descending order.

### LIMIT

> `LIMIT` restricts the number of rows returned by a query.

### OFFSET

> `OFFSET` skips a specified number of rows before returning the result.

---

# 📝 Practice Questions

### Q1.
Find the number of employees in each department.

### Q2.
Find departments having more than 2 employees.

### Q3.
Find departments whose average salary is greater than 40000.

### Q4.
Find the top 3 highest-paid employees.

### Q5.
Find the 3rd and 4th highest-paid employees.

### Q6.
Find the department with the highest number of employees.

### Q7.
Find the top 2 departments based on average salary.

### Q8.
Explain the difference between `WHERE` and `HAVING`.

---

# ⭐ Final Cheat Sheet

```text
GROUP BY → Group rows
HAVING   → Filter groups
ORDER BY → Sort rows/results
LIMIT    → Take rows
OFFSET   → Skip rows
```

### 🔥 Most Common Patterns

```sql
-- GROUP BY
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

```sql
-- GROUP BY + HAVING
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

```sql
-- Top 3 salaries
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

```sql
-- Pagination
SELECT *
FROM employees
ORDER BY id
LIMIT 10 OFFSET 20;
```

```sql
-- Top groups
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY total DESC
LIMIT 3;
```

> 🧠 **Remember:**  
> **WHERE → Rows**  
> **GROUP BY → Groups**  
> **HAVING → Filter Groups**  
> **ORDER BY → Sort**  
> **LIMIT → Take**  
> **OFFSET → Skip**
