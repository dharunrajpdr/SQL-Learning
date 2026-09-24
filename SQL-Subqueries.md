# 📌 SQL — SUBQUERIES

## 1. What is a Subquery?

A **Subquery** is a query written **inside another SQL query**.

👉 The inner query executes first, and its result is used by the outer query.

### Basic Structure

```sql
SELECT column
FROM table
WHERE column = (
    SELECT column
    FROM table
    WHERE condition
);
```

---

## 2. Simple Example

### 👨‍💼 Employees

| id | name  | salary |
|----|-------|--------|
| 1  | Arun  | 30000  |
| 2  | Bala  | 50000  |
| 3  | David | 40000  |
| 4  | Ravi  | 60000  |

### Find the employee who earns the highest salary

```sql
SELECT name, salary
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

### How it works

**Step 1 — Inner query:**

```sql
SELECT MAX(salary)
FROM employees;
```

Output:

| MAX(salary) |
|-------------|
| 60000       |

**Step 2 — Outer query becomes:**

```sql
SELECT name, salary
FROM employees
WHERE salary = 60000;
```

Output:

| name | salary |
|------|--------|
| Ravi | 60000  |

---

# 3. Subquery with WHERE

Find employees whose salary is greater than the average salary.

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

### Inner query

```sql
SELECT AVG(salary)
FROM employees;
```

Average = `45000`

### Output

| name | salary |
|------|--------|
| Bala | 50000  |
| Ravi | 60000  |

---

# 4. Subquery with IN

Use `IN` when the subquery can return **multiple values**.

### Departments

| dept_id | dept_name |
|---------|-----------|
| 1       | IT        |
| 2       | HR        |
| 3       | Sales     |

### Employees

| name  | dept_id |
|-------|---------|
| Arun  | 1       |
| Bala  | 2       |
| David | 1       |
| Ravi  | 3       |

Find employees working in departments whose name starts with `I` or `S`.

```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id
    FROM departments
    WHERE dept_name IN ('IT', 'Sales')
);
```

### Output

| name  |
|-------|
| Arun  |
| David |
| Ravi  |

---

# 5. Subquery with NOT IN

Find employees who are **not** working in the IT department.

```sql
SELECT name
FROM employees
WHERE dept_id NOT IN (
    SELECT dept_id
    FROM departments
    WHERE dept_name = 'IT'
);
```

---

# 6. Subquery with FROM

A subquery can also be used as a **temporary table**.

```sql
SELECT name, salary
FROM (
    SELECT name, salary
    FROM employees
    WHERE salary > 40000
) AS high_salary;
```

### Output

| name  | salary |
|-------|--------|
| Bala  | 50000  |
| David | 40000  |
| Ravi  | 60000  |

> `AS high_salary` gives a name (alias) to the subquery result.

---

# 7. Subquery with SELECT

A subquery can also appear inside the `SELECT` list.

```sql
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM employees) AS avg_salary
FROM employees;
```

### Output

| name  | salary | avg_salary |
|-------|--------|------------|
| Arun  | 30000  | 45000      |
| Bala  | 50000  | 45000      |
| David | 40000  | 45000      |
| Ravi  | 60000  | 45000      |

---

# 8. Correlated Subquery ⭐

A **Correlated Subquery** depends on the outer query.

Example:

Find employees whose salary is greater than the average salary of their own department.

```sql
SELECT e.name, e.salary, e.dept_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.dept_id = e.dept_id
);
```

👉 The inner query uses `e.dept_id` from the outer query.

### Easy Difference

**Normal Subquery:**

```text
Inner Query → executes independently
            ↓
Outer Query
```

**Correlated Subquery:**

```text
Outer Query
     ↓
Inner Query
     ↓
Depends on outer row
     ↓
Next outer row
     ↓
Inner Query again
```

---

# 9. Subquery with EXISTS ⭐

`EXISTS` checks whether the subquery returns **at least one row**.

Example:

Find departments that have at least one employee.

```sql
SELECT d.dept_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.dept_id = d.dept_id
);
```

👉 If the subquery finds even one matching employee, `EXISTS` becomes `TRUE`.

---

# 10. Types of Subqueries

| Type | Meaning |
|------|---------|
| Single-row subquery | Returns one value/row |
| Multi-row subquery | Returns multiple rows |
| Correlated subquery | Depends on outer query |
| Subquery in WHERE | Used for filtering |
| Subquery in FROM | Acts like a temporary table |
| Subquery in SELECT | Produces a value for each row |

---

# 11. Important Operators

| Operator | Used When |
|----------|-----------|
| `=` | Subquery returns one value |
| `>` | Compare with one value |
| `<` | Compare with one value |
| `IN` | Multiple possible values |
| `NOT IN` | Exclude multiple values |
| `EXISTS` | Check whether rows exist |
| `NOT EXISTS` | Check whether rows don't exist |

---

# 12. Subquery vs JOIN

### Subquery

```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id
    FROM departments
    WHERE dept_name = 'IT'
);
```

### JOIN

```sql
SELECT e.name
FROM employees e
JOIN departments d
ON e.dept_id = d.dept_id
WHERE d.dept_name = 'IT';
```

👉 Both can sometimes solve the same problem.

### Easy Memory Trick

```text
SUBQUERY → Query inside Query
JOIN     → Combine Tables
```

---

# 13. Common Mistake ❌

If the subquery returns multiple rows, don't use `=`.

❌ Wrong:

```sql
SELECT name
FROM employees
WHERE dept_id = (
    SELECT dept_id
    FROM departments
);
```

If the inner query returns multiple `dept_id` values, this causes an error.

✅ Use `IN`:

```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id
    FROM departments
);
```

---

# 🎯 Quick Revision

```text
Subquery
   ↓
Query inside another query

WHERE → filtering
FROM  → temporary result/table
SELECT → calculated value
IN → multiple values
EXISTS → check if rows exist
Correlated → depends on outer query
```

---

# 💡 Interview One-Liners

### What is a subquery?

> A subquery is a query written inside another SQL query.

### What is a correlated subquery?

> A correlated subquery depends on values from the outer query and is evaluated for each outer row.

### What is EXISTS?

> EXISTS checks whether the subquery returns at least one row.

### `IN` vs `EXISTS`?

> `IN` compares a value against a set of values, while `EXISTS` checks whether matching rows exist.

### Can a subquery be used in FROM?

> Yes. A subquery in the FROM clause acts as a derived table and usually requires an alias.

---

# 📝 Practice Questions

### Q1. Find employees earning more than the average salary.

### Q2. Find the employee with the maximum salary.

### Q3. Find employees belonging to the IT department using a subquery.

### Q4. Find departments that have at least one employee using `EXISTS`.

### Q5. Find employees whose salary is greater than their department's average salary.

---
