# 📌 SQL — CTE (COMMON TABLE EXPRESSIONS)

## 1. What is a CTE?

**CTE** stands for **Common Table Expression**.

A CTE is a **temporary named result set** that you create using the `WITH` keyword and use inside a SQL query.

👉 It makes complex queries easier to read and understand.

### Easy Memory Trick

```text
CTE = Temporary result + Give it a name + Use it in a query
```

---

# 2. Basic Syntax

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT *
FROM cte_name;
```

---

# 3. Simple Example

Suppose we have:

### employees

| id | name | salary |
|----|------|--------|
| 1 | Arun | 40000 |
| 2 | Ravi | 60000 |
| 3 | Dharun | 70000 |

We want employees whose salary is greater than 50000.

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

### Output

| id | name | salary |
|----|------|--------|
| 2 | Ravi | 60000 |
| 3 | Dharun | 70000 |

---

# 4. Why Use CTE?

Without CTE:

```sql
SELECT *
FROM (
    SELECT *
    FROM employees
    WHERE salary > 50000
) AS high_salary;
```

With CTE:

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

👉 CTE makes the query more readable.

---

# 5. CTE with Selected Columns

We don't have to select `*`.

```sql
WITH high_salary AS (
    SELECT name, salary
    FROM employees
    WHERE salary > 50000
)
SELECT name, salary
FROM high_salary;
```

---

# 6. CTE with Aggregate Functions ⭐

We can use aggregate functions inside a CTE.

```sql
WITH department_salary AS (
    SELECT
        department,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_salary;
```

Example output:

| department | avg_salary |
|------------|------------|
| IT | 65000 |
| HR | 45000 |

---

# 7. CTE with WHERE

We can filter the CTE result.

```sql
WITH department_salary AS (
    SELECT
        department,
        AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
)
SELECT *
FROM department_salary
WHERE avg_salary > 50000;
```

👉 First the CTE calculates the average salary.

Then the outer query filters the result.

---

# 8. Multiple CTEs ⭐

We can create more than one CTE.

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
),
it_employees AS (
    SELECT *
    FROM high_salary
    WHERE department = 'IT'
)
SELECT *
FROM it_employees;
```

Flow:

```text
employees
    ↓
high_salary
    ↓
it_employees
    ↓
final SELECT
```

---

# 9. CTE with JOIN

CTEs can also be joined with other tables.

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT
    high_salary.name,
    departments.department_name
FROM high_salary
JOIN departments
ON high_salary.department_id = departments.id;
```

---

# 10. CTE vs Subquery ⭐

| CTE | Subquery |
|-----|----------|
| Uses `WITH` | Written inside another query |
| Usually easier to read | Can become difficult to read when nested |
| Can define multiple CTEs | Multiple nested subqueries can become complex |
| Can be referenced in the statement | Usually used directly where needed |

### Example

**Subquery:**

```sql
SELECT *
FROM (
    SELECT *
    FROM employees
    WHERE salary > 50000
) AS temp;
```

**CTE:**

```sql
WITH high_salary AS (
    SELECT *
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

---

# 11. CTE vs Temporary Table

| CTE | Temporary Table |
|-----|-----------------|
| Exists for the statement | Exists for a session/transaction depending on DBMS |
| Created using `WITH` | Created using `CREATE TEMPORARY TABLE` in supported DBMS |
| No separate table creation needed | Creates a temporary table |
| Good for organizing a query | Useful when intermediate data needs to be reused across statements |

### Easy Memory Trick

```text
CTE → Temporary result for a query

TEMP TABLE → Temporary table
```

---

# 12. Recursive CTE ⭐⭐

A **recursive CTE** is a CTE that refers to itself.

It is useful for hierarchical data such as:

```text
Company
   ↓
Manager
   ↓
Team Leader
   ↓
Employee
```

Basic structure:

```sql
WITH RECURSIVE employee_tree AS (

    -- Anchor query
    SELECT id, name, manager_id
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive query
    SELECT e.id, e.name, e.manager_id
    FROM employees e
    JOIN employee_tree t
    ON e.manager_id = t.id
)

SELECT *
FROM employee_tree;
```

👉 Recursive CTEs are more advanced. For beginner placement preparation, understand the concept first.

---

# 13. CTE with UPDATE

Some database systems allow CTEs to be used with data modification statements.

Example:

```sql
WITH low_salary AS (
    SELECT id
    FROM employees
    WHERE salary < 30000
)
UPDATE employees
SET salary = 30000
WHERE id IN (
    SELECT id
    FROM low_salary
);
```

⚠️ Exact behavior and syntax can vary between database systems.

---

# 14. CTE with DELETE

Similarly, CTEs can sometimes be used with `DELETE`.

Example:

```sql
WITH old_employees AS (
    SELECT id
    FROM employees
    WHERE joining_year < 2015
)
DELETE FROM employees
WHERE id IN (
    SELECT id
    FROM old_employees
);
```

⚠️ CTE data-modification support varies by DBMS.

---

# 15. Important Point — CTE is Not a Permanent Table

A CTE does **not** permanently store a table in the database.

```sql
WITH high_salary AS (...)
SELECT *
FROM high_salary;
```

After the statement finishes, the CTE is no longer available.

### Remember:

```text
CTE ≠ Permanent Table
CTE ≠ View
```

---

# 16. CTE vs View

| CTE | View |
|-----|------|
| Temporary for a query | Stored database object |
| Defined using `WITH` | Created using `CREATE VIEW` |
| Disappears after query | Can be reused later |
| Good for complex queries | Good for reusable query logic |

---

# 17. When Should You Use a CTE?

Use a CTE when:

✅ Query is becoming complex  
✅ You want readable SQL  
✅ You need multiple logical steps  
✅ You want to reuse an intermediate result within one statement  
✅ You need recursive/hierarchical queries

---

# 🎯 Quick Revision

```text
CTE
 ↓
WITH
 ↓
Give temporary result a name
 ↓
Use it in SELECT / other statements
```

Basic structure:

```sql
WITH name AS (
    SELECT ...
)
SELECT *
FROM name;
```

---

# 💡 Interview One-Liners

### What is a CTE?

> A CTE is a temporary named result set defined using the `WITH` clause and used within a SQL statement.

### Why use a CTE?

> CTEs make complex SQL queries more readable and easier to organize.

### Is a CTE a permanent table?

> No. A CTE exists only for the statement in which it is defined.

### What is a recursive CTE?

> A recursive CTE is a CTE that refers to itself and is useful for hierarchical or tree-structured data.

### CTE vs Subquery?

> A CTE uses the `WITH` clause and usually improves readability, while a subquery is written directly inside another SQL statement.

### CTE vs View?

> A CTE is temporary for a query, while a view is a stored database object that can be reused.

---

# 📝 Practice Questions

### Q1.
Create a CTE to find employees whose salary is greater than 50000.

### Q2.
Create a CTE to calculate the average salary of each department.

### Q3.
Use a CTE to find departments whose average salary is greater than 60000.

### Q4.
Can a CTE be used after the SQL statement finishes?

```text
A) Yes
B) No
```

### Q5.
Which keyword is used to create a CTE?

```text
A) TEMP
B) WITH
C) CTE
D) CREATE
```

### Q6.
What is a recursive CTE used for?

---

# ⭐ Key Point

```text
WITH
 ↓
CTE
 ↓
Temporary named result
 ↓
Use in the main query
```

> 🧠 **Memory Trick:**  
> **CTE = "First prepare the result, give it a name, then use it."**
