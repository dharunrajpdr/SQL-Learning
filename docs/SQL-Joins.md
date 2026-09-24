# 📘 SQL JOINs

> 💡 **In this lesson:** Learn how to combine data from multiple tables using SQL JOINs.

---

# 1️⃣ What is a JOIN?

## 🧠 What is JOIN?

A `JOIN` is used to **combine rows from two or more tables** based on a related column.

In real applications, data is usually stored in multiple tables.

For example:

```text
employees
    ↓
department_id
    ↓
departments
```

We can use `JOIN` to connect these tables.

---

# 2️⃣ Example Tables

Let's use two tables.

## 👨‍💼 Employees

| employee_id | name | department_id | salary |
|------------:|------|--------------:|-------:|
| 1 | Arun | 101 | 40000 |
| 2 | Ravi | 102 | 30000 |
| 3 | Kiran | 101 | 50000 |
| 4 | Priya | 103 | 35000 |
| 5 | Anu | 104 | 45000 |

## 🏢 Departments

| department_id | department_name |
|---------------:|-----------------|
| 101 | IT |
| 102 | HR |
| 103 | Finance |
| 105 | Marketing |

### 🔗 Common Column

Both tables have:

```text
department_id
```

This column is used to connect the tables.

---

# 3️⃣ Types of JOINs

There are mainly four important JOINs:

```text
                 SQL JOINs
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
   INNER JOIN    LEFT JOIN    RIGHT JOIN
                                  │
                                  ↓
                              FULL JOIN
```

| JOIN | What it returns |
|------|------------------|
| `INNER JOIN` | Matching rows from both tables |
| `LEFT JOIN` | All rows from left table + matching rows |
| `RIGHT JOIN` | All rows from right table + matching rows |
| `FULL OUTER JOIN` | All rows from both tables |

> ⭐ **For interviews, understand `INNER JOIN` and `LEFT JOIN` especially well.**

---

# 4️⃣ INNER JOIN

## 🧠 What is INNER JOIN?

`INNER JOIN` returns **only the rows that have matching values in both tables**.

### 📌 Syntax

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

---

## 🔹 Example

We want to display employee names along with their department names.

```sql
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.department_id;
```

### 📤 Output

| name | department_name |
|------|-----------------|
| Arun | IT |
| Ravi | HR |
| Kiran | IT |
| Priya | Finance |

Notice:

```text
Anu → department_id = 104
```

But `104` does not exist in the `departments` table.

So Anu is **not included**.

Similarly:

```text
Marketing → department_id = 105
```

There is no employee with `105`, so Marketing is not included.

### 🧠 Visual

```text
Employees              Departments

101 ────────────────→ 101 ✅
102 ────────────────→ 102 ✅
101 ────────────────→ 101 ✅
103 ────────────────→ 103 ✅
104 ────────────────→ ❌

Only matching records
        ↓
    INNER JOIN
```

> ⭐ **Key Point:**  
> `INNER JOIN` = **Only matching rows**

---

# 5️⃣ LEFT JOIN

## 🧠 What is LEFT JOIN?

`LEFT JOIN` returns:

```text
ALL rows from the LEFT table
+
Matching rows from the RIGHT table
```

If there is no match, SQL returns `NULL`.

### 📌 Syntax

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

---

## 🔹 Example

```sql
SELECT employees.name, departments.department_name
FROM employees
LEFT JOIN departments
ON employees.department_id = departments.department_id;
```

### 📤 Output

| name | department_name |
|------|-----------------|
| Arun | IT |
| Ravi | HR |
| Kiran | IT |
| Priya | Finance |
| Anu | NULL |

Why?

Because:

```text
Anu → department_id = 104
```

There is no department `104`.

But `LEFT JOIN` still keeps Anu because **employees is the left table**.

### 🧠 Visual

```text
LEFT TABLE                    RIGHT TABLE

Employees                     Departments
    │                              │
    │────── matching ──────────────│
    │                              │
    │────── matching ──────────────│
    │                              │
    │────── no match ──────────────X
    │
    ↓
Still included with NULL
```

> ⭐ **Key Point:**  
> `LEFT JOIN` = **Keep everything from the left table**

---

# 6️⃣ RIGHT JOIN

## 🧠 What is RIGHT JOIN?

`RIGHT JOIN` returns:

```text
ALL rows from the RIGHT table
+
Matching rows from the LEFT table
```

If there is no match, SQL returns `NULL`.

### 📌 Syntax

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

---

## 🔹 Example

```sql
SELECT employees.name, departments.department_name
FROM employees
RIGHT JOIN departments
ON employees.department_id = departments.department_id;
```

### 📤 Output

| name | department_name |
|------|-----------------|
| Arun | IT |
| Ravi | HR |
| Kiran | IT |
| Priya | Finance |
| NULL | Marketing |

Why is Marketing included?

Because:

```text
Marketing → department_id = 105
```

It exists in the **right table**, even though there is no matching employee.

> ⭐ **Key Point:**  
> `RIGHT JOIN` = **Keep everything from the right table**

---

# 7️⃣ FULL OUTER JOIN

## 🧠 What is FULL OUTER JOIN?

`FULL OUTER JOIN` returns:

```text
ALL rows from LEFT table
+
ALL rows from RIGHT table
+
Matching rows
```

If there is no match, `NULL` is returned.

### 📌 Syntax

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

### 📤 Output

| name | department_name |
|------|-----------------|
| Arun | IT |
| Ravi | HR |
| Kiran | IT |
| Priya | Finance |
| Anu | NULL |
| NULL | Marketing |

### 🧠 Remember

```text
LEFT JOIN
→ Everything from LEFT

RIGHT JOIN
→ Everything from RIGHT

FULL JOIN
→ Everything from BOTH
```

> ⚠️ **Note:** MySQL does not directly support `FULL OUTER JOIN`. It can be simulated using `LEFT JOIN`, `RIGHT JOIN`, and `UNION`.

---

# 8️⃣ JOIN Using Aliases

When queries become large, table names can make the query difficult to read.

We can use **aliases**.

### Without Alias

```sql
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.department_id;
```

### With Alias

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

Much cleaner! ✨

```text
employees   → e
departments → d
```

> ⭐ **Interview Tip:**  
> Aliases make JOIN queries shorter and easier to understand.

---

# 9️⃣ JOIN + WHERE

We can combine `JOIN` with `WHERE`.

### Example

Find employees who work in the IT department.

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

### 📤 Output

| name | department_name |
|------|-----------------|
| Arun | IT |
| Kiran | IT |

### 🔄 Query Flow

```text
JOIN
 ↓
Connect employees + departments
 ↓
WHERE
 ↓
Keep only IT employees
```

---

# 🔟 JOIN + ORDER BY

We can also sort JOIN results.

### Example

Display employees with their departments and sort by salary.

```sql
SELECT e.name, d.department_name, e.salary
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id
ORDER BY e.salary DESC;
```

### 📤 Output

| name | department_name | salary |
|------|-----------------|-------:|
| Kiran | IT | 50000 |
| Arun | IT | 40000 |
| Priya | Finance | 35000 |
| Ravi | HR | 30000 |

---

# 1️⃣1️⃣ JOIN + GROUP BY

This is very important for SQL interviews.

### Example

Find the number of employees in each department.

```sql
SELECT d.department_name, COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name;
```

### 📤 Output

| department_name | employee_count |
|-----------------|---------------:|
| IT | 2 |
| HR | 1 |
| Finance | 1 |
| Marketing | 0 |

### 🧠 Why LEFT JOIN?

Because we want to include:

```text
Marketing
```

even though it has no employees.

---

# 1️⃣2️⃣ JOIN + GROUP BY + ORDER BY

Now let's combine everything.

### Example

Find departments with their employee count and sort from highest to lowest.

```sql
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count
FROM departments d
LEFT JOIN employees e
ON d.department_id = e.department_id
GROUP BY d.department_name
ORDER BY employee_count DESC;
```

### 📤 Output

| department_name | employee_count |
|-----------------|---------------:|
| IT | 2 |
| HR | 1 |
| Finance | 1 |
| Marketing | 0 |

### 🔄 Query Flow

```text
JOIN
 ↓
Connect the tables
 ↓
GROUP BY
 ↓
Create department groups
 ↓
COUNT()
 ↓
Count employees
 ↓
ORDER BY
 ↓
Sort the result
```

---

# 1️⃣3️⃣ JOIN + LIMIT

We can find the highest-paid employee along with their department.

```sql
SELECT e.name, d.department_name, e.salary
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id
ORDER BY e.salary DESC
LIMIT 1;
```

### 📤 Output

| name | department_name | salary |
|------|-----------------|-------:|
| Kiran | IT | 50000 |

---

# 🧠 JOIN Cheat Sheet

| JOIN | Result |
|------|--------|
| `INNER JOIN` | 🤝 Only matching rows |
| `LEFT JOIN` | ⬅️ Everything from left + matching right |
| `RIGHT JOIN` | ➡️ Everything from right + matching left |
| `FULL OUTER JOIN` | 🔄 Everything from both tables |

---

# 🎯 Easy Memory Trick

```text
INNER JOIN
     ↓
Only MATCHING 🤝

LEFT JOIN
     ↓
Keep LEFT ⬅️

RIGHT JOIN
     ↓
Keep RIGHT ➡️

FULL JOIN
     ↓
Keep BOTH 🔄
```

---

# ⭐ Most Important Interview Difference

## INNER JOIN vs LEFT JOIN

### INNER JOIN

```sql
SELECT *
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

```text
❌ Unmatched employees → removed
❌ Unmatched departments → removed
✅ Matching rows → included
```

### LEFT JOIN

```sql
SELECT *
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id;
```

```text
✅ All employees → included
✅ Matching departments → included
NULL for unmatched department
```

---

# 🚀 Real-World Example

Imagine an e-commerce application.

You might have:

```text
users
  │
  ├── orders
  │
  └── reviews
```

To get users and their orders:

```sql
SELECT u.name, o.order_id, o.amount
FROM users u
INNER JOIN orders o
ON u.user_id = o.user_id;
```

This is how JOINs are used in real backend applications.

---

# 🎯 Interview One-Liners

### INNER JOIN

> `INNER JOIN` returns only the records that have matching values in both tables.

### LEFT JOIN

> `LEFT JOIN` returns all records from the left table and matching records from the right table.

### RIGHT JOIN

> `RIGHT JOIN` returns all records from the right table and matching records from the left table.

### FULL OUTER JOIN

> `FULL OUTER JOIN` returns all records from both tables, including unmatched records.

---

# 🧠 Final Revision

```text
JOIN
 │
 ├── INNER → Matching only 🤝
 │
 ├── LEFT  → Everything from LEFT ⬅️
 │
 ├── RIGHT → Everything from RIGHT ➡️
 │
 └── FULL  → Everything from BOTH 🔄
```

### 🔥 Remember This

```text
INNER → MATCH
LEFT  → LEFT + MATCH
RIGHT → RIGHT + MATCH
FULL  → LEFT + RIGHT
```

---
# 📘 SQL JOINs – SELF JOIN, CROSS JOIN & NON-EQUI JOIN

> 💡 **In this lesson:** Learn three useful JOIN concepts after INNER, LEFT, RIGHT, and FULL JOIN.

---

# 1️⃣ SELF JOIN

## 🧠 What is SELF JOIN?

A `SELF JOIN` means **joining a table with itself**.

It is useful when rows in the same table have a relationship with each other.

### 🌍 Real-World Example

Consider an employee table:

| employee_id | name | manager_id |
|------------:|------|-----------:|
| 1 | Arun | NULL |
| 2 | Ravi | 1 |
| 3 | Kiran | 1 |
| 4 | Priya | 2 |
| 5 | Anu | 2 |

Here:

```text
Arun → Manager: None
Ravi → Manager: Arun
Kiran → Manager: Arun
Priya → Manager: Ravi
Anu → Manager: Ravi
```

The `manager_id` refers to another employee's `employee_id`.

So we need to use the **same table twice**.

---

## 📌 Syntax

```sql
SELECT ...
FROM table_name t1
JOIN table_name t2
ON t1.column = t2.column;
```

We use aliases to distinguish the two copies of the same table.

```text
employees e
employees m
```

Here:

```text
e → Employee
m → Manager
```

---

## 🔹 Example: Find Employee and Manager

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

### 📤 Output

| employee | manager |
|----------|---------|
| Arun | NULL |
| Ravi | Arun |
| Kiran | Arun |
| Priya | Ravi |
| Anu | Ravi |

### 🔄 How it works

```text
employees e
     ↓
Current employee

employees m
     ↓
Manager

e.manager_id
     ↓
matches
     ↓
m.employee_id
```

### 🧠 Example

For Ravi:

```text
Ravi's manager_id = 1

Employee with employee_id = 1
        ↓
      Arun

Therefore:

Ravi → Arun
```

> ⭐ **Key Point:**  
> `SELF JOIN` = **A table joined with itself**

---

# 2️⃣ Why do we need Aliases in SELF JOIN?

Consider:

```sql
SELECT name
FROM employees
JOIN employees
ON ...
```

This becomes confusing because SQL sees the same table twice.

So we use aliases:

```sql
employees e
employees m
```

Then:

```sql
e.name
```

means employee name.

And:

```sql
m.name
```

means manager name.

### 📌 Clean Query

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

> 💡 **Interview Tip:**  
> In a SELF JOIN, using different aliases is essential for distinguishing the two instances of the table.

---

# 3️⃣ SELF JOIN with WHERE

We can also filter the result.

### Example

Find employees who report directly to Arun.

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
JOIN employees m
ON e.manager_id = m.employee_id
WHERE m.name = 'Arun';
```

### 📤 Output

| employee | manager |
|----------|---------|
| Ravi | Arun |
| Kiran | Arun |

---

# 4️⃣ CROSS JOIN

## 🧠 What is CROSS JOIN?

`CROSS JOIN` combines **every row from the first table with every row from the second table**.

It is also called a **Cartesian Product**. :contentReference[oaicite:1]{index=1}

### 📌 Syntax

```sql
SELECT *
FROM table1
CROSS JOIN table2;
```

Notice:

```text
❌ No ON condition
```

---

# 5️⃣ Simple CROSS JOIN Example

Suppose we have:

## 🎨 Colors

| color |
|-------|
| Red |
| Blue |

## 👕 Sizes

| size |
|------|
| S |
| M |
| L |

Now:

```sql
SELECT *
FROM colors
CROSS JOIN sizes;
```

### 📤 Output

| color | size |
|-------|------|
| Red | S |
| Red | M |
| Red | L |
| Blue | S |
| Blue | M |
| Blue | L |

### 🧠 What happened?

Every color is combined with every size.

```text
Red  → S
     → M
     → L

Blue → S
     → M
     → L
```

Total combinations:

```text
2 colors × 3 sizes = 6 rows
```

---

# 6️⃣ CROSS JOIN Formula

If:

```text
Table A → M rows
Table B → N rows
```

Then:

```text
Result = M × N rows
```

### Example

```text
Customers = 5
Products  = 10

Result = 5 × 10
       = 50 rows
```

> ⚠️ **Important:**  
> A CROSS JOIN can produce a very large result because every row is combined with every other row. :contentReference[oaicite:2]{index=2}

---

# 7️⃣ Real-World Use of CROSS JOIN

A common use is generating all possible combinations.

For example:

```text
Colors
   +
Sizes
   ↓
Product Variations
```

You can generate:

```text
Red-S
Red-M
Red-L
Blue-S
Blue-M
Blue-L
```

Another example:

```text
Students × Subjects
```

This can generate all possible student-subject combinations.

---

# 8️⃣ SELF JOIN vs CROSS JOIN

| Feature | SELF JOIN | CROSS JOIN |
|---------|-----------|------------|
| Tables | Same table | Usually different tables |
| Main purpose | Find relationships within a table | Generate combinations |
| `ON` condition | Usually used | ❌ Not required |
| Example | Employee → Manager | Color → Size |
| Result | Related rows | Every possible combination |

### 🧠 Easy Memory Trick

```text
SELF JOIN
    ↓
Same table 🔄
    
CROSS JOIN
    ↓
Every combination ✖️
```

---

# 9️⃣ NON-EQUI JOIN

## 🧠 What is a Non-Equi JOIN?

A **Non-Equi JOIN** uses a condition other than `=`.

For example:

```sql
>
<
>=
<=
<>
BETWEEN
```

The usual JOIN:

```sql
ON a.id = b.id
```

is an equality condition.

A Non-Equi JOIN might use:

```sql
ON e.salary BETWEEN s.min_salary AND s.max_salary
```

---

# 🔟 Non-Equi JOIN Example

Suppose we have an employees table:

| name | salary |
|------|-------:|
| Arun | 25000 |
| Ravi | 45000 |
| Kiran | 75000 |

And a salary grade table:

| grade | min_salary | max_salary |
|-------|-----------:|-----------:|
| C | 0 | 30000 |
| B | 30001 | 60000 |
| A | 60001 | 100000 |

We want to find the grade of each employee.

### Query

```sql
SELECT
    e.name,
    e.salary,
    s.grade
FROM employees e
JOIN salary_grades s
ON e.salary BETWEEN s.min_salary AND s.max_salary;
```

### 📤 Output

| name | salary | grade |
|------|-------:|-------|
| Arun | 25000 | C |
| Ravi | 45000 | B |
| Kiran | 75000 | A |

### 🧠 Why is this Non-Equi JOIN?

Because we are not comparing:

```sql
e.salary = s.min_salary
```

Instead:

```sql
e.salary BETWEEN s.min_salary AND s.max_salary
```

So the JOIN condition uses a range.

---

# 1️⃣1️⃣ Another Non-Equi JOIN Example

Suppose:

## Employees

| name | salary |
|------|-------:|
| Arun | 40000 |
| Ravi | 60000 |

## Salary Levels

| level | minimum_salary |
|-------|---------------:|
| Junior | 20000 |
| Mid | 50000 |

We could compare values using:

```sql
SELECT
    e.name,
    e.salary,
    s.level
FROM employees e
JOIN salary_levels s
ON e.salary >= s.minimum_salary;
```

> ⚠️ This can produce multiple matches for one employee because one salary may satisfy multiple ranges/conditions. Always check whether the data model makes the condition unique.

---

# 1️⃣2️⃣ Types of JOIN – Complete Revision

```text
                         SQL JOINs
                             │
       ┌─────────────┬───────┼───────────┬─────────────┐
       ↓             ↓       ↓           ↓             ↓
    INNER          LEFT    RIGHT       FULL          CROSS
       │             │       │           │             │
    MATCH         LEFT    RIGHT        BOTH        EVERY
    ONLY          TABLE    TABLE        TABLE     COMBINATION
```

And:

```text
SELF JOIN
   ↓
Same table joined with itself
```

```text
NON-EQUI JOIN
   ↓
JOIN using conditions such as >, <, >=, <=, BETWEEN
```

---

# 🧠 Complete JOIN Cheat Sheet

| JOIN | Easy Meaning | Example |
|------|--------------|---------|
| `INNER JOIN` | 🤝 Matching rows | Employee + Department |
| `LEFT JOIN` | ⬅️ Everything from left | All Employees |
| `RIGHT JOIN` | ➡️ Everything from right | All Departments |
| `FULL OUTER JOIN` | 🔄 Everything from both | Combine all records |
| `CROSS JOIN` | ✖️ Every combination | Color + Size |
| `SELF JOIN` | 🔁 Same table | Employee + Manager |
| `NON-EQUI JOIN` | 📏 Range/condition matching | Salary + Grade |

---

# 🎯 Interview One-Liners

### SELF JOIN

> A SELF JOIN is a JOIN where a table is joined with itself, usually using different aliases.

### CROSS JOIN

> A CROSS JOIN returns the Cartesian product, meaning every row of one table is combined with every row of the other table.

### NON-EQUI JOIN

> A Non-Equi JOIN uses comparison operators such as `>`, `<`, `>=`, `<=`, `<>`, or `BETWEEN` instead of only equality.

---

