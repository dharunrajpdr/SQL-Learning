# 📌 SQL — VIEWS

## 1. What is a View?

A **View** is a **virtual table** created from the result of a SQL query.

👉 It does not normally store the actual data separately.  
👉 It stores the **SQL query** used to retrieve the data.

### Easy Memory Trick

```text
TABLE → Stores data
VIEW  → Stores query
```

---

# 2. Why Use Views?

Views are useful for:

- 🔐 Hiding sensitive columns
- ♻️ Reusing complex queries
- 🧹 Simplifying SQL queries
- 👥 Controlling what data users can see
- 📊 Creating customized data views

---

# 3. Creating a View

### Syntax

```sql
CREATE VIEW view_name AS
SELECT columns
FROM table
WHERE condition;
```

---

# 4. Simple Example

### Employees

| id | name  | department | salary |
|----|-------|------------|--------|
| 1  | Arun  | IT         | 50000  |
| 2  | Bala  | HR         | 40000  |
| 3  | David | IT         | 60000  |

Create a view containing only IT employees.

```sql
CREATE VIEW it_employees AS
SELECT id, name, salary
FROM employees
WHERE department = 'IT';
```

Now we can use the view like a table:

```sql
SELECT *
FROM it_employees;
```

### Output

| id | name  | salary |
|----|-------|--------|
| 1  | Arun  | 50000  |
| 3  | David | 60000  |

---

# 5. View with Specific Columns 🔐

Suppose employees have sensitive information:

| id | name | email | salary | password |
|----|------|-------|--------|----------|

We don't want everyone to see the password.

Create a view without sensitive columns:

```sql
CREATE VIEW employee_details AS
SELECT id, name, email
FROM employees;
```

Now:

```sql
SELECT *
FROM employee_details;
```

👉 The view exposes only the selected columns.

---

# 6. View Using JOIN

Views can contain joins too.

### Employees

| id | name | dept_id |
|----|------|---------|
| 1 | Arun | 10 |
| 2 | Bala | 20 |

### Departments

| dept_id | dept_name |
|---------|-----------|
| 10 | IT |
| 20 | HR |

Create a view:

```sql
CREATE VIEW employee_department AS
SELECT
    e.id,
    e.name,
    d.dept_name
FROM employees e
JOIN departments d
ON e.dept_id = d.dept_id;
```

Now:

```sql
SELECT *
FROM employee_department;
```

### Output

| id | name | dept_name |
|----|------|-----------|
| 1 | Arun | IT |
| 2 | Bala | HR |

---

# 7. Updating Data Through a View

Some views can be used to modify the underlying table.

Example:

```sql
CREATE VIEW it_employees AS
SELECT id, name, salary
FROM employees
WHERE department = 'IT';
```

Then:

```sql
UPDATE it_employees
SET salary = 55000
WHERE id = 1;
```

👉 If the view is updatable, the underlying `employees` table is also updated.

⚠️ Not every view is updatable.

---

# 8. CREATE OR REPLACE VIEW

To modify an existing view:

```sql
CREATE OR REPLACE VIEW it_employees AS
SELECT id, name, salary, department
FROM employees
WHERE department = 'IT';
```

👉 This replaces the existing definition of the view.

---

# 9. Dropping a View

If you no longer need a view:

```sql
DROP VIEW it_employees;
```

👉 This removes the **view**, not the original table.

```text
DROP VIEW
    ↓
View removed

Original Table
    ↓
Still exists
```

---

# 10. View with Aggregate Functions

A view can also contain `GROUP BY` and aggregate functions.

Example:

```sql
CREATE VIEW department_salary AS
SELECT
    department,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

Use it:

```sql
SELECT *
FROM department_salary;
```

### Output

| department | average_salary |
|------------|----------------|
| IT | 55000 |
| HR | 40000 |

---

# 11. View vs Table ⭐

| Feature | Table | View |
|---------|-------|------|
| Stores actual data | ✅ | Usually ❌ |
| Stores query | ❌ | ✅ |
| Can use SELECT | ✅ | ✅ |
| Can use JOIN | ✅ | ✅ |
| Can simplify complex queries | ❌ | ✅ |
| Can hide columns | ❌ | ✅ |
| Takes separate storage for data | ✅ | Usually ❌ |

---

# 12. View vs Subquery

### Subquery

Used inside another query:

```sql
SELECT *
FROM (
    SELECT name, salary
    FROM employees
    WHERE salary > 50000
) AS high_salary;
```

### View

Saved and reusable:

```sql
CREATE VIEW high_salary AS
SELECT name, salary
FROM employees
WHERE salary > 50000;
```

Then:

```sql
SELECT *
FROM high_salary;
```

### Easy Difference

```text
SUBQUERY → Temporary query inside another query

VIEW     → Saved query that can be reused
```

---

# 13. Advantages of Views

### 🔐 Security

Hide sensitive columns.

```sql
CREATE VIEW public_employee AS
SELECT id, name, department
FROM employees;
```

### ♻️ Reusability

Instead of writing a complex query repeatedly:

```sql
SELECT *
FROM employee_department;
```

### 🧹 Simplicity

Users can query a simple view instead of understanding multiple joins.

### 🔄 Abstraction

The underlying table structure can be hidden from users.

---

# 14. Disadvantages of Views

- Complex views can affect performance.
- Some views cannot be updated.
- Too many nested views can make queries difficult to understand.
- A normal view does not automatically store a separate copy of the result.

---

# 15. View Query Flow

```text
CREATE VIEW
     ↓
SQL Query is defined
     ↓
View is created
     ↓
SELECT FROM View
     ↓
Database executes the underlying query
     ↓
Result is returned
```

---

# 🎯 Quick Revision

```text
CREATE VIEW
     ↓
Create virtual table

SELECT FROM VIEW
     ↓
Use the view

CREATE OR REPLACE VIEW
     ↓
Modify the view

DROP VIEW
     ↓
Delete the view
```

### Important Commands

```sql
CREATE VIEW
CREATE OR REPLACE VIEW
SELECT
DROP VIEW
```

---

# 💡 Interview One-Liners

### What is a View?

> A view is a virtual table based on the result of a SQL query.

### Why are Views used?

> Views are used to simplify complex queries, improve data security, and provide controlled access to data.

### Does a View store data?

> A normal view generally stores the query definition rather than a separate copy of the data.

### Can we update a View?

> Some views are updatable, but views containing certain operations such as aggregation or complex joins may not be directly updatable.

### What happens when we DROP a View?

> The view is removed, but the underlying tables remain unchanged.

### View vs Subquery?

> A subquery is written inside another query, while a view is a saved query that can be reused.

---

# 📝 Practice Questions

### Q1.
Create a view containing only employees from the IT department.

### Q2.
Create a view containing employee name and salary.

### Q3.
Create a view showing employee names and department names using `JOIN`.

### Q4.
Create a view showing the average salary of each department.

### Q5.
Delete an existing view using `DROP VIEW`.

---

# ⭐ Key Point

```text
VIEW = Virtual Table

CREATE VIEW → Create
SELECT FROM → Use
CREATE OR REPLACE → Modify
DROP VIEW → Delete
```
