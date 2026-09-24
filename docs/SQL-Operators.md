# 📘 SQL – LIKE, BETWEEN, IN & CASE

> 💡 **In this lesson:** Learn how to search, filter, and conditionally display data in SQL.

---

# 1️⃣ LIKE

## 🧠 What is LIKE?

`LIKE` is used to **search for a specific pattern** in a column.

It is mainly used with text/string values.

### 📌 Syntax

```sql
SELECT *
FROM table_name
WHERE column_name LIKE pattern;
```

---

## 🔹 Wildcards

There are two important wildcards:

| Wildcard | Meaning |
|----------|---------|
| `%` | Any number of characters |
| `_` | Exactly one character |

---

## 🔹 Example Table

| id | name | department |
|---:|------|------------|
| 1 | Arun | IT |
| 2 | Ravi | HR |
| 3 | Kiran | IT |
| 4 | Priya | Finance |
| 5 | Anu | Sales |

---

## 🔹 Example 1: Names starting with `A`

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

### 📤 Output

| id | name | department |
|---:|------|------------|
| 1 | Arun | IT |
| 5 | Anu | Sales |

### 🧠 Meaning

```text
A%
│
└── Starts with A
    followed by anything
```

---

## 🔹 Example 2: Names ending with `i`

```sql
SELECT *
FROM employees
WHERE name LIKE '%i';
```

### 📤 Output

| id | name |
|---:|------|
| 2 | Ravi |
| 3 | Kiran |

> ⚠️ The exact result depends on the database's case-sensitivity and collation settings.

---

## 🔹 Example 3: Names containing `an`

```sql
SELECT *
FROM employees
WHERE name LIKE '%an%';
```

### 📤 Output

| name |
|------|
| Kiran |

### 🧠 Meaning

```text
%an%

Anything
  ↓
 an
  ↓
Anything
```

---

## 🔹 Example 4: `_` wildcard

```sql
SELECT *
FROM employees
WHERE name LIKE '_run';
```

`_` represents exactly **one character**.

```text
Arun
↑
One character
```

So `Arun` matches:

```text
_run
```

---

## ⭐ LIKE Cheat Sheet

```text
'A%'   → Starts with A

'%A'   → Ends with A

'%A%'  → Contains A

'_A%'  → Second character is A

'____' → Exactly 4 characters
```

---

# 2️⃣ BETWEEN

## 🧠 What is BETWEEN?

`BETWEEN` is used to check whether a value is **within a range**.

### 📌 Syntax

```sql
SELECT *
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

> ⭐ `BETWEEN` is generally **inclusive**, meaning the boundary values are included.

---

## 🔹 Example: Salary between 30000 and 45000

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 30000 AND 45000;
```

### 📤 Output

| name | salary |
|------|-------:|
| Arun | 40000 |
| Ravi | 30000 |
| Priya | 35000 |
| Anu | 45000 |

### 🧠 Meaning

```text
30000 ≤ salary ≤ 45000
```

Both:

```text
30000 ✅
45000 ✅
```

are included.

---

## 🔹 BETWEEN with Dates

`BETWEEN` can also be used with dates.

```sql
SELECT *
FROM orders
WHERE order_date BETWEEN '2026-01-01' AND '2026-01-31';
```

> ⚠️ With timestamps, boundary behavior can matter. For a full day in timestamp-based data, a half-open range such as `>= start AND < next_day` is often safer.

---

# 3️⃣ IN

## 🧠 What is IN?

`IN` is used to check whether a value matches **one of several specified values**.

Instead of writing multiple `OR` conditions, we can use `IN`.

### ❌ Without IN

```sql
SELECT *
FROM employees
WHERE department = 'IT'
   OR department = 'HR'
   OR department = 'Sales';
```

### ✅ With IN

```sql
SELECT *
FROM employees
WHERE department IN ('IT', 'HR', 'Sales');
```

Much cleaner! ✨

---

## 🔹 Example

```sql
SELECT *
FROM employees
WHERE department IN ('IT', 'Finance');
```

### 📤 Output

| name | department |
|------|------------|
| Arun | IT |
| Kiran | IT |
| Priya | Finance |

---

## 🔹 NOT IN

`NOT IN` returns values that are **not present** in the given list.

```sql
SELECT *
FROM employees
WHERE department NOT IN ('IT', 'HR');
```

This returns employees whose department is neither IT nor HR.

> ⚠️ **Important:** `NOT IN` can behave unexpectedly when `NULL` is involved. Be careful when working with nullable data.

---

# 4️⃣ CASE

## 🧠 What is CASE?

`CASE` is used to perform **conditional logic inside SQL**.

It is similar to:

```text
if
else if
else
```

in programming languages.

### 📌 Syntax

```sql
CASE
    WHEN condition THEN result
    WHEN condition THEN result
    ELSE result
END
```

---

## 🔹 Example: Salary Category

Suppose we want to classify employees based on salary.

```sql
SELECT
    name,
    salary,
    CASE
        WHEN salary >= 50000 THEN 'High'
        WHEN salary >= 40000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### 📤 Output

| name | salary | salary_category |
|------|-------:|-----------------|
| Arun | 40000 | Medium |
| Ravi | 30000 | Low |
| Kiran | 50000 | High |
| Priya | 35000 | Low |
| Anu | 45000 | Medium |

### 🧠 Think of it like Java/C:

```text
if salary >= 50000
    High

else if salary >= 40000
    Medium

else
    Low
```

---

## 🔹 CASE with GROUP BY

We can use `CASE` to create categories and then group them.

```sql
SELECT
    CASE
        WHEN salary >= 40000 THEN 'High'
        ELSE 'Low'
    END AS category,
    COUNT(*) AS total
FROM employees
GROUP BY
    CASE
        WHEN salary >= 40000 THEN 'High'
        ELSE 'Low'
    END;
```

### 📤 Output

| category | total |
|----------|------:|
| High | 3 |
| Low | 2 |

---

# 5️⃣ LIKE + BETWEEN + IN

These operators can also be combined.

### 🔥 Example

Find employees whose:

```text
Name starts with A
AND
Salary is between 30000 and 50000
AND
Department is IT or Sales
```

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%'
  AND salary BETWEEN 30000 AND 50000
  AND department IN ('IT', 'Sales');
```

### 🧠 Query Flow

```text
LIKE
 ↓
Name starts with A

BETWEEN
 ↓
Salary is 30000–50000

IN
 ↓
Department is IT or Sales
```

---

# 6️⃣ CASE + ORDER BY

We can also sort using a `CASE` expression.

### 🔹 Example

Suppose we want IT employees to appear first.

```sql
SELECT
    name,
    department,
    salary
FROM employees
ORDER BY
    CASE
        WHEN department = 'IT' THEN 1
        ELSE 2
    END;
```

This creates a temporary sorting value:

```text
IT       → 1
Other    → 2
```

So IT rows appear before other departments.

---

# 🧠 Quick Revision

| Keyword | Purpose | Easy Word |
|---------|---------|-----------|
| `LIKE` | 🔍 Pattern matching | SEARCH |
| `BETWEEN` | 📏 Range checking | RANGE |
| `IN` | 📋 Match from a list | LIST |
| `CASE` | 🔀 Conditional logic | IF/ELSE |

---

# 🎯 Easy Memory Trick

```text
LIKE     → SEARCH 🔍

BETWEEN  → RANGE 📏

IN       → LIST 📋

CASE     → CONDITION 🔀
```

---

# ⭐ Interview One-Liners

### LIKE

> `LIKE` is used for pattern matching in string values using wildcards such as `%` and `_`.

### BETWEEN

> `BETWEEN` checks whether a value falls within a specified range, usually including both boundaries.

### IN

> `IN` checks whether a value matches any value from a specified list.

### CASE

> `CASE` provides conditional logic in SQL, similar to if-else statements.

---

# 📝 Practice Questions

### Q1️⃣ LIKE

Find employees whose names start with `A`.

---

### Q2️⃣ LIKE

Find employees whose names contain `ri`.

---

### Q3️⃣ BETWEEN

Find employees whose salary is between `30000` and `45000`.

---

### Q4️⃣ BETWEEN

Find employees whose salary is between `40000` and `60000`.

---

### Q5️⃣ IN

Find employees working in:

```text
IT
HR
Finance
```

---

### Q6️⃣ NOT IN

Find employees who are not working in:

```text
IT
HR
```

---

### Q7️⃣ CASE ⭐

Create a salary category:

```text
salary >= 50000 → High
salary >= 40000 → Medium
otherwise       → Low
```

---

### Q8️⃣ 🔥 Combined

Find employees whose:

```text
department IN ('IT', 'HR')
AND
salary BETWEEN 30000 AND 50000
AND
name LIKE 'A%'
```

---

# 🏆 Final Cheat Sheet

```text
LIKE
→ Pattern search
→ % = any number of characters
→ _ = one character

BETWEEN
→ Check a range
→ Usually inclusive

IN
→ Check multiple possible values

NOT IN
→ Exclude multiple possible values

CASE
→ Conditional logic
→ Similar to if/else
```

---
