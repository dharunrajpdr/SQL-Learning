# 📌 SQL — WINDOW FUNCTIONS

## 1. What is a Window Function?

A **Window Function** performs a calculation across a set of related rows **without combining them into a single row**.

👉 Unlike `GROUP BY`, window functions **keep every original row**.

### Basic Syntax

```sql
function_name() OVER (
    PARTITION BY column
    ORDER BY column
);
```

---

## 2. Example Table

### Employees

| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Arun  | IT         | 50000  |
| 2  | Bala  | IT         | 60000  |
| 3  | David | HR         | 40000  |
| 4  | Ravi  | HR         | 45000  |
| 5  | Kiran | IT         | 55000  |

---

# 3. ROW_NUMBER()

Assigns a **unique number** to each row.

```sql
SELECT
    name,
    department,
    salary,
    ROW_NUMBER() OVER (
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
```

### Output

| name  | department | salary | row_num |
|-------|------------|--------|---------|
| Bala  | IT         | 60000  | 1 |
| Kiran | IT         | 55000  | 2 |
| Arun  | IT         | 50000  | 3 |
| Ravi  | HR         | 45000  | 4 |
| David | HR         | 40000  | 5 |

👉 Even if two employees have the same salary, `ROW_NUMBER()` gives different numbers.

---

# 4. RANK()

Assigns the same rank to equal values.

```sql
SELECT
    name,
    salary,
    RANK() OVER (
        ORDER BY salary DESC
    ) AS rank_num
FROM employees;
```

Example:

| name | salary | rank |
|------|--------|------|
| Bala | 60000 | 1 |
| Kiran | 55000 | 2 |
| Arun | 50000 | 3 |
| Ravi | 45000 | 4 |
| David | 40000 | 5 |

If two employees have the same salary:

| salary | RANK |
|--------|------|
| 60000  | 1 |
| 60000  | 1 |
| 50000  | 3 |

👉 `RANK()` **skips** the next rank after a tie.

---

# 5. DENSE_RANK()

Works like `RANK()`, but **does not skip ranks**.

```sql
SELECT
    name,
    salary,
    DENSE_RANK() OVER (
        ORDER BY salary DESC
    ) AS dense_rank_num
FROM employees;
```

If salaries are:

| salary | RANK | DENSE_RANK |
|--------|------|------------|
| 60000  | 1 | 1 |
| 60000  | 1 | 1 |
| 50000  | 3 | 2 |

### Easy Memory Trick

```text
ROW_NUMBER  → Always unique
RANK        → Tie + skips rank
DENSE_RANK  → Tie + no skipped rank
```

---

# 6. PARTITION BY ⭐

`PARTITION BY` divides rows into groups.

👉 Think of it as creating separate windows for each group.

### Rank employees within each department

```sql
SELECT
    name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS dept_rank
FROM employees;
```

### Output

| name  | department | salary | dept_rank |
|-------|------------|--------|-----------|
| Bala  | IT | 60000 | 1 |
| Kiran | IT | 55000 | 2 |
| Arun  | IT | 50000 | 3 |
| Ravi  | HR | 45000 | 1 |
| David | HR | 40000 | 2 |

👉 Ranking **restarts for each department**.

---

# 7. SUM() as a Window Function

We can use aggregate functions as window functions.

```sql
SELECT
    name,
    department,
    salary,
    SUM(salary) OVER (
        PARTITION BY department
    ) AS dept_total_salary
FROM employees;
```

### Output

| name | department | salary | dept_total_salary |
|------|------------|--------|-------------------|
| Arun | IT | 50000 | 165000 |
| Bala | IT | 60000 | 165000 |
| Kiran | IT | 55000 | 165000 |
| David | HR | 40000 | 85000 |
| Ravi | HR | 45000 | 85000 |

👉 Every employee keeps their row while seeing the department total.

---

# 8. AVG() as a Window Function

Find each employee's salary and their department's average salary.

```sql
SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS dept_avg_salary
FROM employees;
```

---

# 9. COUNT() as a Window Function

Count employees in each department.

```sql
SELECT
    name,
    department,
    COUNT(*) OVER (
        PARTITION BY department
    ) AS dept_employee_count
FROM employees;
```

---

# 10. LAG()

`LAG()` gets the value from a **previous row**.

```sql
SELECT
    name,
    salary,
    LAG(salary) OVER (
        ORDER BY id
    ) AS previous_salary
FROM employees;
```

Example:

| name | salary | previous_salary |
|------|--------|-----------------|
| Arun | 50000 | NULL |
| Bala | 60000 | 50000 |
| David | 40000 | 60000 |
| Ravi | 45000 | 40000 |

👉 Useful for comparing the current row with the previous row.

---

# 11. LEAD()

`LEAD()` gets the value from the **next row**.

```sql
SELECT
    name,
    salary,
    LEAD(salary) OVER (
        ORDER BY id
    ) AS next_salary
FROM employees;
```

### Easy Memory Trick

```text
LAG  → Look backward
LEAD → Look forward
```

---

# 12. FIRST_VALUE()

Returns the first value in the window.

```sql
SELECT
    name,
    department,
    salary,
    FIRST_VALUE(salary) OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS highest_salary
FROM employees;
```

---

# 13. LAST_VALUE()

Returns the last value in the window.

```sql
SELECT
    name,
    department,
    salary,
    LAST_VALUE(salary) OVER (
        PARTITION BY department
        ORDER BY salary
        ROWS BETWEEN UNBOUNDED PRECEDING
        AND UNBOUNDED FOLLOWING
    ) AS lowest_salary
FROM employees;
```

👉 `LAST_VALUE()` often needs an explicit window frame to get the expected result.

---

# 14. Window Function vs GROUP BY ⭐

### GROUP BY

```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```

Output:

| department | avg_salary |
|------------|------------|
| IT | 55000 |
| HR | 42500 |

👉 Rows are **grouped**.

### Window Function

```sql
SELECT
    name,
    department,
    salary,
    AVG(salary) OVER (
        PARTITION BY department
    ) AS avg_salary
FROM employees;
```

Output:

| name | department | salary | avg_salary |
|------|------------|--------|------------|
| Arun | IT | 50000 | 55000 |
| Bala | IT | 60000 | 55000 |
| Kiran | IT | 55000 | 55000 |
| David | HR | 40000 | 42500 |
| Ravi | HR | 45000 | 42500 |

👉 Original rows are **preserved**.

### Easy Memory Trick

```text
GROUP BY
→ Combine rows

WINDOW FUNCTION
→ Keep rows + calculate
```

---

# 15. Find Top 2 Employees in Each Department ⭐

This is a very common interview problem.

```sql
SELECT *
FROM (
    SELECT
        name,
        department,
        salary,
        ROW_NUMBER() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS rn
    FROM employees
) AS ranked
WHERE rn <= 2;
```

👉 First, employees are numbered inside each department.

👉 Then the outer query selects only ranks `1` and `2`.

---

# 16. Running Total

Calculate a running salary total.

```sql
SELECT
    name,
    salary,
    SUM(salary) OVER (
        ORDER BY id
    ) AS running_total
FROM employees;
```

Example:

| name | salary | running_total |
|------|--------|---------------|
| Arun | 50000 | 50000 |
| Bala | 60000 | 110000 |
| David | 40000 | 150000 |
| Ravi | 45000 | 195000 |

---

# 17. Important Window Functions

| Function | Purpose |
|----------|---------|
| `ROW_NUMBER()` | Unique row number |
| `RANK()` | Rank with gaps |
| `DENSE_RANK()` | Rank without gaps |
| `LAG()` | Previous row |
| `LEAD()` | Next row |
| `FIRST_VALUE()` | First value |
| `LAST_VALUE()` | Last value |
| `SUM() OVER()` | Window total |
| `AVG() OVER()` | Window average |
| `COUNT() OVER()` | Window count |
| `MAX() OVER()` | Window maximum |
| `MIN() OVER()` | Window minimum |

---

# 🎯 Quick Revision

```text
WINDOW FUNCTION
       ↓
Calculates across related rows
       ↓
Keeps original rows
       ↓
OVER()
       ↓
PARTITION BY → divide into groups
ORDER BY     → control order
```

### Most Important Syntax

```sql
function() OVER (
    PARTITION BY column
    ORDER BY column
);
```

---

# 💡 Interview One-Liners

### What is a Window Function?

> A window function performs calculations across related rows while keeping the individual rows in the result.

### What is PARTITION BY?

> PARTITION BY divides the result into groups without combining the rows.

### Difference between RANK and DENSE_RANK?

> RANK skips ranks after ties, while DENSE_RANK does not skip ranks.

### Difference between ROW_NUMBER and RANK?

> ROW_NUMBER gives every row a unique number, while RANK gives the same rank to tied values.

### What does LAG do?

> LAG returns a value from a previous row.

### What does LEAD do?

> LEAD returns a value from a following row.

---

# 📝 Practice Questions

### Q1.
Find the salary rank of every employee.

### Q2.
Find the highest-paid employee in each department.

### Q3.
Find the top 2 highest-paid employees in each department.

### Q4.
Find the average salary of each department alongside every employee.

### Q5.
Find the difference between an employee's salary and the previous employee's salary using `LAG()`.

### Q6.
Calculate the running total of salaries.

---

# ⭐ Key Point

Remember these 3 first:

```text
ROW_NUMBER() → Unique numbering
RANK()       → Ranking with gaps
DENSE_RANK() → Ranking without gaps
```

And remember:

```text
GROUP BY → Reduces rows
WINDOW   → Keeps rows
```
