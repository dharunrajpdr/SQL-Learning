# 📘 SQL Clauses – GROUP BY, ORDER BY, LIMIT & OFFSET

> 💡 **In this lesson:** Learn how to group, sort, limit, and skip SQL query results.

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

# 2️⃣ ORDER BY

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
| Arun | 40000 |
| Priya | 35000 |
| Anu | 45000 |
| Kiran | 50000 |

### ⬆️ Correct Ascending Order

| name | salary |
|------|-------:|
| Ravi | 30000 |
| Arun | 40000 |
| Priya | 35000 |
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

# 3️⃣ LIMIT

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

Usually, `LIMIT` is combined with `ORDER BY`.

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

# 4️⃣ OFFSET

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

### 📤 Example Output

| id | name |
|---:|------|
| 3 | Kiran |
| 4 | Priya |

> ⭐ **Easy Rule:**  
> `OFFSET` = **Skip** rows  
> `LIMIT` = **Take** rows

---

# 5️⃣ LIMIT + OFFSET

## 📄 Pagination

`LIMIT` and `OFFSET` are commonly used for **pagination**.

For example, suppose we display **2 employees per page**.

---

### 📄 Page 1

```sql
SELECT *
FROM employees
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
LIMIT 2 OFFSET 4;
```

---

# 6️⃣ GROUP BY + ORDER BY

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

### 🔄 Query Flow

```text
GROUP BY
   ↓
Create department groups
   ↓
COUNT()
   ↓
Count employees in each group
   ↓
ORDER BY DESC
   ↓
Sort by employee count
```

---

# 7️⃣ GROUP BY + ORDER BY + LIMIT

We can find the department with the highest number of employees.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
ORDER BY employee_count DESC
LIMIT 1;
```

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
ORDER BY DESC
   ↓
Highest count first
   ↓
LIMIT 1
   ↓
Take the first result
```

---

# 8️⃣ ORDER BY + LIMIT + OFFSET

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

# 🧠 Quick Revision

| Clause | Meaning | Easy Word |
|--------|---------|-----------|
| `GROUP BY` | Groups similar rows | 🧩 GROUP |
| `ORDER BY` | Sorts the result | 🔃 SORT |
| `LIMIT` | Restricts rows | 🎯 TAKE |
| `OFFSET` | Skips rows | ⏭️ SKIP |

### 💡 Easy Memory Trick

```text
GROUP BY → GROUP 🧩
ORDER BY → SORT 🔃
LIMIT    → TAKE 🎯
OFFSET   → SKIP ⏭️
```

---

# 🚀 One Important Combined Query

```sql
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
ORDER BY total DESC
LIMIT 2 OFFSET 1;
```

### 🧠 Understand it step-by-step

```text
GROUP BY
   ↓
Create department groups

COUNT()
   ↓
Count employees in each department

ORDER BY
   ↓
Sort departments by count

OFFSET 1
   ↓
Skip the first result

LIMIT 2
   ↓
Take the next 2 results
```

---

# 🎯 Interview One-Liners

### GROUP BY
> `GROUP BY` is used to group rows having the same values, usually with aggregate functions.

### ORDER BY
> `ORDER BY` is used to sort query results in ascending or descending order.

### LIMIT
> `LIMIT` restricts the number of rows returned by a query.

### OFFSET
> `OFFSET` skips a specified number of rows before returning the result.

---

## ⭐ Final Cheat Sheet

```text
GROUP BY → Group rows
ORDER BY → Sort rows
LIMIT    → Take rows
OFFSET   → Skip rows
```

### 🔥 Most Common Patterns

```sql
-- Count each group
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
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
LIMIT 10 OFFSET 20;
```

```sql
-- Top groups
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department
ORDER BY total DESC
LIMIT 3;
```
