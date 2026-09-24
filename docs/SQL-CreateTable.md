# 📚 SQL — CREATE TABLE

## 🔹 What is `CREATE TABLE`?

`CREATE TABLE` is used to **create a new table** inside a database.

A table stores data in the form of **rows and columns**.

---

## 📝 Syntax

```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype
);
```

---

## 🗃️ Example

Imagine we have a `CollegeDB` database and want to store student information.

> **Create a table to store student details.**

### ⭐ SQL Query

```sql
CREATE TABLE Students (
    id INT,
    name VARCHAR(50),
    age INT
);
```

This creates a `Students` table with three columns:

| Column | Data Type |
|--------|-----------|
| `id` | `INT` |
| `name` | `VARCHAR(50)` |
| `age` | `INT` |

---

## ⭐ Key Point

> **`CREATE TABLE` is used to create a new table with columns and their data types.**
