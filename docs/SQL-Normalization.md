# 📌 SQL — NORMALIZATION

## 1. What is Normalization?

**Normalization** is the process of organizing data in a database to:

- Reduce duplicate data
- Avoid data inconsistency
- Improve data organization
- Maintain data integrity

### Easy Memory Trick

```text
Normalization = Remove unnecessary duplication
```

---

# 2. Why Do We Need Normalization?

Suppose we have:

### Student Table

| student_id | student_name | course | instructor |
|------------|--------------|--------|------------|
| 1 | Arun | Java | Ravi |
| 2 | Karthik | Java | Ravi |
| 3 | Dharun | SQL | Kumar |

Here, `Ravi` is stored multiple times.

If the instructor name changes, we may need to update multiple rows.

👉 This can cause **data inconsistency**.

---

# 3. Problems Without Normalization

Three important problems are called **Anomalies**.

```text
INSERT Anomaly
UPDATE Anomaly
DELETE Anomaly
```

---

# 4. Insert Anomaly

An **insert anomaly** happens when we cannot insert some information without adding unrelated information.

Example:

Suppose a new course is created but no student has enrolled yet.

If course and student information are stored in the same table, we may not be able to store the course properly.

---

# 5. Update Anomaly

An **update anomaly** happens when the same information is stored in multiple rows and we must update it everywhere.

Example:

```text
Ravi → Java Instructor
```

If Ravi's name changes to `Raj`, multiple rows may need to be updated.

If one row is missed:

```text
Ravi
Raj
Ravi
```

❌ Inconsistent data.

---

# 6. Delete Anomaly

A **delete anomaly** happens when deleting one piece of information accidentally removes another important piece of information.

Example:

If the only student enrolled in a course is deleted, the course information may also disappear.

---

# 7. Normal Forms

The major normal forms are:

```text
1NF
 ↓
2NF
 ↓
3NF
 ↓
BCNF
```

For beginner-level interviews, focus mainly on:

```text
1NF
2NF
3NF
```

---

# 8. First Normal Form — 1NF ⭐

A table is in **1NF** when:

- Each column contains atomic/single values
- There are no repeating groups

### ❌ Not 1NF

| student_id | name | phone |
|------------|------|-------|
| 1 | Arun | 9876, 8765 |

The `phone` column contains multiple values.

### ✅ 1NF

| student_id | name | phone |
|------------|------|-------|
| 1 | Arun | 9876 |
| 1 | Arun | 8765 |

Now each cell contains one value.

### Memory Trick

```text
1NF = One value per cell
```

---

# 9. Second Normal Form — 2NF ⭐

A table is in **2NF** when:

1. It is already in 1NF
2. There is no **partial dependency**

Partial dependency mainly matters when the table has a **composite primary key**.

---

# 10. What is a Composite Key?

A composite key contains more than one column.

Example:

```text
(student_id, course_id)
```

Together they uniquely identify a record.

---

# 11. Partial Dependency

Suppose:

| student_id | course_id | student_name | course_name |
|------------|-----------|--------------|-------------|
| 1 | 101 | Arun | Java |
| 1 | 102 | Arun | SQL |

Primary key:

```text
(student_id, course_id)
```

But:

```text
student_id → student_name
course_id  → course_name
```

`student_name` depends only on `student_id`.

`course_name` depends only on `course_id`.

This is **partial dependency**.

---

# 12. Convert to 2NF

Separate the tables.

### Students

| student_id | student_name |
|------------|--------------|
| 1 | Arun |

### Courses

| course_id | course_name |
|-----------|-------------|
| 101 | Java |
| 102 | SQL |

### Enrollment

| student_id | course_id |
|------------|-----------|
| 1 | 101 |
| 1 | 102 |

Now the partial dependencies are removed.

### Memory Trick

```text
2NF = 1NF + No Partial Dependency
```

---

# 13. Third Normal Form — 3NF ⭐

A table is in **3NF** when:

1. It is already in 2NF
2. There is no **transitive dependency**

---

# 14. What is Transitive Dependency?

Suppose:

| employee_id | employee_name | department_id | department_name |
|-------------|---------------|---------------|-----------------|
| 1 | Arun | 10 | IT |
| 2 | Ravi | 20 | HR |

Dependencies:

```text
employee_id → department_id
department_id → department_name
```

Therefore:

```text
employee_id → department_name
```

The department name indirectly depends on employee ID.

This is called **transitive dependency**.

---

# 15. Convert to 3NF

Separate the information.

### Employees

| employee_id | employee_name | department_id |
|-------------|---------------|---------------|
| 1 | Arun | 10 |
| 2 | Ravi | 20 |

### Departments

| department_id | department_name |
|---------------|-----------------|
| 10 | IT |
| 20 | HR |

Now department information is stored separately.

### Memory Trick

```text
3NF = 2NF + No Transitive Dependency
```

---

# 16. 1NF vs 2NF vs 3NF

| Normal Form | Main Rule |
|-------------|-----------|
| 1NF | Atomic values |
| 2NF | No partial dependency |
| 3NF | No transitive dependency |

### Easy Memory Trick

```text
1NF → Atomic
2NF → Partial dependency removed
3NF → Transitive dependency removed
```

---

# 17. Normalization Example

Suppose we have:

```text
Student
----------------------------------
student_id
student_name
course_id
course_name
instructor_name
```

Possible problems:

```text
Student information
+
Course information
+
Instructor information
```

are mixed together.

We can separate them:

```text
Students
Courses
Instructors
Enrollment
```

And connect them using:

```text
PRIMARY KEY
FOREIGN KEY
```

---

# 18. Normalization vs Denormalization

### Normalization

```text
Reduce duplication
      ↓
More tables
      ↓
Better data consistency
```

### Denormalization

```text
Combine data
      ↓
Fewer joins
      ↓
Can improve read performance in some situations
      ↓
More duplication
```

---

# 19. Advantages of Normalization

✅ Reduces duplicate data  
✅ Improves consistency  
✅ Makes updates easier  
✅ Prevents anomalies  
✅ Improves database organization  

---

# 20. Disadvantages

❌ More tables may be created  
❌ More JOINs may be required  
❌ Complex queries can sometimes become harder to write  

---

# 🎯 Quick Revision

```text
NORMALIZATION
      ↓
Organize database
      ↓
Reduce redundancy
      ↓
Avoid anomalies
      ↓
1NF → 2NF → 3NF
```

### Remember:

```text
1NF → One value per cell

2NF → No partial dependency

3NF → No transitive dependency
```

---

# 💡 Interview One-Liners

### What is normalization?

> Normalization is the process of organizing data to reduce redundancy and improve data integrity.

### What is 1NF?

> A table is in 1NF when each column contains atomic values and there are no repeating groups.

### What is 2NF?

> A table is in 2NF when it is in 1NF and has no partial dependency on a composite key.

### What is 3NF?

> A table is in 3NF when it is in 2NF and has no transitive dependency.

### What is a composite key?

> A composite key is a key made up of two or more columns.

### What is an anomaly?

> An anomaly is an unwanted problem caused by poor database design, such as insert, update, or delete anomalies.

---

# 📝 Practice Questions

### Q1.
What is the main purpose of normalization?

### Q2.
What does 1NF require?

### Q3.
What is partial dependency?

### Q4.
What is transitive dependency?

### Q5.
What is the difference between 2NF and 3NF?

### Q6.
Name the three common normal forms.

---

# ⭐ Key Point

```text
Normalization
     ↓
Reduce Redundancy
     ↓
Avoid Anomalies
     ↓
1NF → 2NF → 3NF
```

> 🧠 **Memory Trick:**  
> **1NF = Atomic**  
> **2NF = No Partial**  
> **3NF = No Transitive**
