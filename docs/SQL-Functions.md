# 📌 SQL — FUNCTIONS

## 1. What is a SQL Function?

A **SQL Function** is a built-in or user-defined operation that performs a specific task and returns a result.

👉 Functions help us perform calculations, manipulate text, work with dates, and more.

### Easy Memory Trick

```text
FUNCTION = Perform a task → Return a result
```

---

# 2. Types of SQL Functions

SQL functions can be broadly divided into:

```text
SQL FUNCTIONS
     ↓
 ┌───────────────┐
 ↓               ↓
Single-Row     Aggregate
Functions      Functions
 ↓               ↓
Work on        Work on
each row       multiple rows
```

---

# 3. Single-Row Functions

A single-row function works on **one row at a time** and returns one result for each row.

Common categories:

```text
String Functions
Numeric Functions
Date Functions
Conversion Functions
```

---

# 4. String Functions ⭐

String functions are used to work with text.

### UPPER()

Converts text to uppercase.

```sql
SELECT UPPER('hello');
```

Output:

```text
HELLO
```

---

### LOWER()

Converts text to lowercase.

```sql
SELECT LOWER('HELLO');
```

Output:

```text
hello
```

---

### LENGTH()

Returns the number of characters.

```sql
SELECT LENGTH('Dharun');
```

Output:

```text
6
```

---

### CONCAT()

Combines strings.

```sql
SELECT CONCAT('Dharun', ' Raj');
```

Output:

```text
Dharun Raj
```

Example with columns:

```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

---

### SUBSTRING()

Extracts part of a string.

```sql
SELECT SUBSTRING('DATABASE', 1, 4);
```

Output:

```text
DATA
```

⚠️ String-position rules can vary by database system.

---

### TRIM()

Removes leading and trailing spaces.

```sql
SELECT TRIM('  Hello  ');
```

Output:

```text
Hello
```

---

# 5. Numeric Functions

Used for mathematical operations.

### ROUND()

Rounds a number.

```sql
SELECT ROUND(45.678, 2);
```

Output:

```text
45.68
```

---

### CEIL()

Rounds up.

```sql
SELECT CEIL(45.2);
```

Output:

```text
46
```

---

### FLOOR()

Rounds down.

```sql
SELECT FLOOR(45.9);
```

Output:

```text
45
```

---

### ABS()

Returns the absolute value.

```sql
SELECT ABS(-25);
```

Output:

```text
25
```

---

### MOD()

Returns the remainder.

```sql
SELECT MOD(10, 3);
```

Output:

```text
1
```

---

# 6. Date Functions

Date functions are used to work with dates and times.

### CURRENT_DATE

Returns the current date.

```sql
SELECT CURRENT_DATE;
```

---

### CURRENT_TIMESTAMP

Returns the current date and time.

```sql
SELECT CURRENT_TIMESTAMP;
```

---

### YEAR()

Extracts the year.

```sql
SELECT YEAR('2026-09-24');
```

Output:

```text
2026
```

---

### MONTH()

Extracts the month.

```sql
SELECT MONTH('2026-09-24');
```

Output:

```text
9
```

---

### DAY()

Extracts the day.

```sql
SELECT DAY('2026-09-24');
```

Output:

```text
24
```

⚠️ Date-function names and syntax vary between MySQL, PostgreSQL, SQL Server, and other databases.

---

# 7. Aggregate Functions ⭐

Aggregate functions work on **multiple rows** and return a summarized result.

The most important ones are:

```text
COUNT()
SUM()
AVG()
MAX()
MIN()
```

Example:

```sql
SELECT AVG(salary)
FROM employees;
```

---

# 8. COUNT()

Counts rows.

```sql
SELECT COUNT(*)
FROM employees;
```

### COUNT(column)

```sql
SELECT COUNT(email)
FROM employees;
```

👉 `COUNT(column)` generally counts only non-NULL values.

---

# 9. SUM()

Returns the total.

```sql
SELECT SUM(salary)
FROM employees;
```

---

# 10. AVG()

Returns the average.

```sql
SELECT AVG(salary)
FROM employees;
```

---

# 11. MAX()

Returns the maximum value.

```sql
SELECT MAX(salary)
FROM employees;
```

---

# 12. MIN()

Returns the minimum value.

```sql
SELECT MIN(salary)
FROM employees;
```

---

# 13. Functions with GROUP BY ⭐

Functions can be combined with `GROUP BY`.

```sql
SELECT
    department,
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

### Output

| department | avg_salary |
|------------|------------|
| IT | 55000 |
| HR | 42500 |

---

# 14. Functions with WHERE

We can use functions inside `WHERE`.

Example:

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

String example:

```sql
SELECT *
FROM employees
WHERE LOWER(name) = 'arun';
```

👉 This converts the name to lowercase before comparison.

---

# 15. Functions with ORDER BY

We can also use functions for sorting.

```sql
SELECT name, salary
FROM employees
ORDER BY LENGTH(name);
```

👉 Employees are sorted based on the length of their names.

---

# 16. COALESCE() ⭐

`COALESCE()` returns the first non-NULL value.

```sql
SELECT COALESCE(phone, 'Not Available')
FROM employees;
```

If `phone` is:

```text
NULL
```

Output:

```text
Not Available
```

If `phone` contains:

```text
9876543210
```

Output:

```text
9876543210
```

### Easy Memory Trick

```text
COALESCE()
    ↓
First non-NULL value
```

---

# 17. NULLIF()

`NULLIF()` returns `NULL` if two values are equal.

```sql
SELECT NULLIF(10, 10);
```

Output:

```text
NULL
```

If they are different:

```sql
SELECT NULLIF(10, 20);
```

Output:

```text
10
```

---

# 18. Function vs Procedure ⭐

| Function | Stored Procedure |
|----------|------------------|
| Returns a value | May return result sets/output values depending on DBMS |
| Commonly used inside expressions | Usually executed as a separate statement |
| Useful for calculations/transformation | Useful for performing a sequence of operations |
| Can often be used in `SELECT` | Usually called/executed separately |

### Easy Memory Trick

```text
FUNCTION  → Calculate → Return
PROCEDURE → Perform   → Execute
```

---

# 19. Important Functions to Remember

### String

```text
UPPER()
LOWER()
LENGTH()
CONCAT()
SUBSTRING()
TRIM()
```

### Numeric

```text
ROUND()
CEIL()
FLOOR()
ABS()
MOD()
```

### Date

```text
CURRENT_DATE
CURRENT_TIMESTAMP
YEAR()
MONTH()
DAY()
```

### Aggregate

```text
COUNT()
SUM()
AVG()
MAX()
MIN()
```

### NULL Handling

```text
COALESCE()
NULLIF()
```

---

# 🎯 Quick Revision

```text
SQL FUNCTIONS
      ↓
 ┌────┴─────┐
 ↓          ↓
Single     Aggregate
Row        Functions
 ↓          ↓
String     COUNT
Numeric    SUM
Date       AVG
NULL       MAX
           MIN
```

---

# 💡 Interview One-Liners

### What is a SQL Function?

> A SQL function performs a specific operation and returns a result.

### What is an Aggregate Function?

> An aggregate function processes multiple rows and returns a summarized result.

### Difference between COUNT(*) and COUNT(column)?

> COUNT(*) counts rows, while COUNT(column) generally counts non-NULL values in that column.

### What does COALESCE do?

> COALESCE returns the first non-NULL value from the provided expressions.

### What is the difference between a function and a stored procedure?

> A function is generally designed to return a value and can often be used in expressions, while a stored procedure is executed to perform a sequence of database operations.

---

# 📝 Practice Questions

### Q1.
Convert employee names to uppercase.

### Q2.
Find the length of every employee's name.

### Q3.
Find the total salary of all employees.

### Q4.
Find the highest and lowest salary.

### Q5.
Find the average salary of each department.

### Q6.
Display `Not Available` when an employee's phone number is NULL.

### Q7.
Find employees whose names contain more than 5 characters.

---

# ⭐ Key Point

```text
String Functions   → Work with text
Numeric Functions  → Work with numbers
Date Functions     → Work with dates
Aggregate Functions → Summarize rows
NULL Functions     → Handle NULL values
```

### 🧠 Easy Memory Trick

> **Function = Take input → Perform operation → Return result**
