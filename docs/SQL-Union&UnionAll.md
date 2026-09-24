# 📌 SQL — UNION & UNION ALL

## 1. What is UNION?

`UNION` is used to **combine the results of two or more SELECT queries** into a single result.

### Easy Memory Trick

```text
SELECT 1
   +
SELECT 2
   ↓
 UNION
   ↓
One combined result
```

---

# 2. Basic Syntax

```sql
SELECT column1, column2
FROM table1

UNION

SELECT column1, column2
FROM table2;
```

---

# 3. Simple Example

### employees_2025

| name | department |
|------|------------|
| Arun | IT |
| Ravi | HR |

### employees_2026

| name | department |
|------|------------|
| Dharun | IT |
| Karthik | Finance |

Query:

```sql
SELECT name, department
FROM employees_2025

UNION

SELECT name, department
FROM employees_2026;
```

### Output

| name | department |
|------|------------|
| Arun | IT |
| Ravi | HR |
| Dharun | IT |
| Karthik | Finance |

---

# 4. UNION Removes Duplicates ⭐

Suppose:

### table1

```text
Arun
Ravi
Dharun
```

### table2

```text
Ravi
Karthik
Dharun
```

Using:

```sql
SELECT name FROM table1
UNION
SELECT name FROM table2;
```

Output:

```text
Arun
Ravi
Dharun
Karthik
```

👉 Duplicate values are removed.

---

# 5. UNION ALL ⭐

`UNION ALL` also combines results, but **keeps duplicates**.

```sql
SELECT name
FROM table1

UNION ALL

SELECT name
FROM table2;
```

Output:

```text
Arun
Ravi
Dharun
Ravi
Karthik
Dharun
```

---

# 6. UNION vs UNION ALL

| UNION | UNION ALL |
|-------|-----------|
| Combines results | Combines results |
| Removes duplicates | Keeps duplicates |
| May require extra work to remove duplicates | Usually faster |
| Used when unique results are needed | Used when all rows are needed |

### Easy Memory Trick

```text
UNION     → Unique
UNION ALL → All
```

---

# 7. Important Rule — Same Number of Columns ⭐

Both SELECT statements should return the **same number of columns**.

Correct:

```sql
SELECT name, salary
FROM employees1

UNION

SELECT name, salary
FROM employees2;
```

❌ Incorrect:

```sql
SELECT name, salary
FROM employees1

UNION

SELECT name
FROM employees2;
```

Reason:

```text
First query  → 2 columns
Second query → 1 column
```

---

# 8. Compatible Data Types

Corresponding columns should have **compatible data types**.

Example:

```sql
SELECT name
FROM employees

UNION

SELECT name
FROM managers;
```

Both `name` columns contain text.

---

# 9. Column Names in UNION

The final result generally uses the column names from the **first SELECT**.

Example:

```sql
SELECT name AS person_name
FROM employees

UNION

SELECT name AS employee_name
FROM managers;
```

The output column will generally be named:

```text
person_name
```

---

# 10. UNION with WHERE

We can use `WHERE` in both queries.

```sql
SELECT name
FROM employees
WHERE department = 'IT'

UNION

SELECT name
FROM managers
WHERE department = 'IT';
```

---

# 11. UNION with ORDER BY ⭐

`ORDER BY` is normally placed **at the end** of the complete UNION query.

```sql
SELECT name
FROM employees

UNION

SELECT name
FROM managers

ORDER BY name;
```

This sorts the combined result.

---

# 12. UNION with Different Tables

UNION is useful when two tables have similar types of data.

Example:

```text
students_2025
students_2026
```

We can combine them:

```sql
SELECT name
FROM students_2025

UNION

SELECT name
FROM students_2026;
```

---

# 13. UNION vs JOIN ⭐

This is an important interview question.

### UNION

Combines **rows** from different SELECT results.

```text
Table A
 ↓
Rows
 ↓
UNION
 ↓
Table B
 ↓
Rows
```

### JOIN

Combines **columns/data from related tables** based on a condition.

```text
Table A + Table B
       ↓
      JOIN
       ↓
 Combined columns
```

Example:

```sql
SELECT name
FROM employees

UNION

SELECT name
FROM managers;
```

vs.

```sql
SELECT e.name, d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.id;
```

### Memory Trick

```text
UNION → Combine rows/results

JOIN  → Combine related columns
```

---

# 14. UNION vs UNION ALL — Example

Suppose:

### Query 1

```text
A
B
C
```

### Query 2

```text
B
C
D
```

Using `UNION`:

```text
A
B
C
D
```

Using `UNION ALL`:

```text
A
B
C
B
C
D
```

---

# 15. Performance Difference ⭐

`UNION` has to identify and remove duplicates.

`UNION ALL` does not remove duplicates.

Therefore:

```text
UNION ALL
   ↓
No duplicate-removal step
   ↓
Usually faster
```

👉 If you know duplicates are acceptable or impossible, `UNION ALL` can be preferable.

---

# 16. Multiple UNIONs

We can combine more than two queries.

```sql
SELECT name FROM table1

UNION

SELECT name FROM table2

UNION

SELECT name FROM table3;
```

All three result sets are combined.

---

# 17. Using UNION with Different Column Names

The actual column names don't need to be the same, but the columns should represent compatible data.

Example:

```sql
SELECT employee_name
FROM employees

UNION

SELECT manager_name
FROM managers;
```

Both produce one text column.

---

# 18. Common Mistakes ❌

### Mistake 1 — Different number of columns

```sql
SELECT name, salary
FROM employees

UNION

SELECT name
FROM managers;
```

❌ Not valid.

---

### Mistake 2 — Incompatible data types

For example, combining unrelated types may cause errors or implicit conversions depending on the DBMS.

---

### Mistake 3 — Using JOIN when UNION is needed

Remember:

```text
UNION → Rows
JOIN  → Related data/columns
```

---

# 🎯 Quick Revision

```text
UNION
  ↓
Combines SELECT results
  ↓
Removes duplicates
```

```text
UNION ALL
  ↓
Combines SELECT results
  ↓
Keeps duplicates
```

---

# 💡 Interview One-Liners

### What is UNION?

> UNION combines the results of multiple SELECT queries and removes duplicate rows.

### What is UNION ALL?

> UNION ALL combines multiple SELECT results while keeping duplicate rows.

### Difference between UNION and UNION ALL?

> UNION removes duplicates, while UNION ALL keeps duplicates and is generally faster.

### What is required for UNION?

> The SELECT statements should return the same number of columns with compatible data types.

### UNION vs JOIN?

> UNION combines rows from multiple query results, while JOIN combines related data from tables based on a condition.

---

# 📝 Practice Questions

### Q1.
Combine employee names from `employees_2025` and `employees_2026` using `UNION`.

### Q2.
Combine the same tables using `UNION ALL`.

### Q3.
What happens to duplicate rows when using `UNION`?

```text
A) They are removed
B) They are duplicated
C) They become NULL
D) Error
```

### Q4.
Which is generally faster when duplicate removal is not required?

```text
A) UNION
B) UNION ALL
```

### Q5.
Can you use UNION if the first query returns 3 columns and the second returns 2 columns?

```text
A) Yes
B) No
```

### Q6.
What is the main difference between JOIN and UNION?

---

# ⭐ Key Point

```text
UNION     → Combine results + Remove duplicates
UNION ALL → Combine results + Keep duplicates

JOIN      → Combine related table data
```

> 🧠 **Memory Trick:**  
> **UNION = Stack results vertically.**  
> **JOIN = Combine related data horizontally.**
