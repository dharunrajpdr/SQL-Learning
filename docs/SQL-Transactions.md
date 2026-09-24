# 📌 SQL — TRANSACTIONS

## 1. What is a Transaction?

A **Transaction** is a group of one or more SQL operations treated as a **single unit of work**.

👉 Either all required operations are completed, or the transaction can be rolled back.

### Simple Example

Suppose Arun transfers ₹1000 to Bala.

```text
Arun Account
₹5000 → ₹4000

Bala Account
₹3000 → ₹4000
```

Two operations are required:

```sql
UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;
```

Both operations should be treated as one transaction.

---

# 2. Transaction Commands

The main transaction commands are:

| Command | Purpose |
|---------|---------|
| `START TRANSACTION` | Starts a transaction |
| `COMMIT` | Permanently saves changes |
| `ROLLBACK` | Undoes uncommitted changes |
| `SAVEPOINT` | Creates a point to roll back to |
| `ROLLBACK TO SAVEPOINT` | Rolls back to a savepoint |

---

# 3. START TRANSACTION

Starts a transaction.

```sql
START TRANSACTION;
```

Then we can perform SQL operations:

```sql
UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;
```

---

# 4. COMMIT ⭐

`COMMIT` permanently saves the changes made during the transaction.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

After `COMMIT`:

```text
Changes
   ↓
Saved permanently
```

---

# 5. ROLLBACK ⭐

`ROLLBACK` cancels changes made during the current transaction that have not been committed.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

ROLLBACK;
```

The update is undone.

### Easy Memory Trick

```text
COMMIT   → Save
ROLLBACK → Undo
```

---

# 6. SAVEPOINT

A `SAVEPOINT` creates a temporary point inside a transaction.

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 500
WHERE id = 1;

SAVEPOINT point1;

UPDATE accounts
SET balance = balance + 500
WHERE id = 2;
```

Now we can roll back only to `point1`:

```sql
ROLLBACK TO SAVEPOINT point1;
```

👉 The operations after `point1` are undone, while earlier changes remain part of the transaction.

---

# 7. Complete SAVEPOINT Example

```sql
START TRANSACTION;

UPDATE employees
SET salary = salary + 1000
WHERE id = 1;

SAVEPOINT salary_update;

UPDATE employees
SET salary = salary + 2000
WHERE id = 2;

ROLLBACK TO SAVEPOINT salary_update;

COMMIT;
```

### What happens?

```text
Start Transaction
       ↓
Update Employee 1
       ↓
SAVEPOINT
       ↓
Update Employee 2
       ↓
ROLLBACK TO SAVEPOINT
       ↓
Employee 2 update undone
       ↓
COMMIT
       ↓
Employee 1 update saved
```

---

# 8. ACID Properties ⭐⭐⭐

Transactions are commonly explained using the **ACID** properties.

```text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

---

# 9. Atomicity

**Atomicity** means a transaction is treated as a single unit.

Either the required operations succeed, or the transaction can be rolled back.

Example:

```text
Transfer ₹1000

Debit Arun   ✅
Credit Bala  ❌

Transaction should not leave the transfer half-completed.
```

### Easy Memory Trick

> **Atomicity = All or Nothing**

---

# 10. Consistency

**Consistency** means a transaction should preserve the database's defined rules and constraints.

Example:

```text
Before transaction:
Total money = ₹8000

After valid transfer:
Total money = ₹8000
```

The database moves from one valid state to another valid state.

### Easy Memory Trick

> **Consistency = Valid State → Valid State**

---

# 11. Isolation

**Isolation** controls how concurrent transactions interact with each other.

Example:

```text
Transaction A
       ↕
   Database
       ↕
Transaction B
```

The database uses isolation mechanisms to prevent inappropriate interference between concurrent operations.

### Easy Memory Trick

> **Isolation = Transactions should not improperly interfere**

---

# 12. Durability

**Durability** means that once a transaction is successfully committed, its changes should persist even if there is a subsequent system failure, subject to the database's recovery guarantees.

### Easy Memory Trick

> **Durability = Committed data stays saved**

---

# 13. ACID in One Table

| Property | Meaning | Memory Trick |
|----------|---------|--------------|
| Atomicity | All required operations succeed or can be rolled back | All or Nothing |
| Consistency | Maintains database rules | Valid → Valid |
| Isolation | Controls concurrent transactions | No improper interference |
| Durability | Committed changes persist | Saved |

---

# 14. Transaction Example 💳

Suppose:

### Accounts

| id | name | balance |
|----|------|---------|
| 1 | Arun | 5000 |
| 2 | Bala | 3000 |

Transfer ₹1000 from Arun to Bala:

```sql
START TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

### Final Result

| id | name | balance |
|----|------|---------|
| 1 | Arun | 4000 |
| 2 | Bala | 4000 |

---

# 15. What If Something Goes Wrong?

Suppose the first update succeeds but the second operation fails.

```text
Arun → ₹4000
Bala → ₹3000
```

We don't want the database to remain in this incomplete state.

We can roll back the transaction:

```sql
ROLLBACK;
```

The database returns to the previous transaction state.

---

# 16. Transaction vs Normal SQL

### Normal operation

```sql
UPDATE employees
SET salary = 50000
WHERE id = 1;
```

### Transaction

```sql
START TRANSACTION;

UPDATE employees
SET salary = 50000
WHERE id = 1;

UPDATE employees
SET salary = 60000
WHERE id = 2;

COMMIT;
```

👉 Multiple operations can be handled as one unit.

---

# 17. COMMIT vs ROLLBACK

| COMMIT | ROLLBACK |
|--------|----------|
| Saves transaction changes | Undoes uncommitted changes |
| Makes changes durable | Returns to earlier transaction state |
| Ends the current transaction in many DBMSs | Ends the current transaction in many DBMSs |

### Easy Memory Trick

```text
COMMIT   = ✅ Save
ROLLBACK = ❌ Undo
```

---

# 18. SAVEPOINT vs ROLLBACK

### ROLLBACK

```sql
ROLLBACK;
```

👉 Rolls back the current transaction's uncommitted changes.

### ROLLBACK TO SAVEPOINT

```sql
ROLLBACK TO SAVEPOINT point1;
```

👉 Rolls back only to the specified savepoint.

```text
Transaction
    ↓
Operation 1
    ↓
SAVEPOINT
    ↓
Operation 2
    ↓
Operation 3
    ↓
ROLLBACK TO SAVEPOINT
    ↓
Undo Operation 2 & 3
```

---

# 19. Important Note ⚠️

Transaction behavior can vary depending on the **database system and storage engine**.

For example, in MySQL, transaction support depends on the storage engine; **InnoDB** supports transactions.

Also, some SQL statements may cause implicit commits depending on the DBMS.

👉 So always check the transaction behavior of the specific database you're using.

---

# 🎯 Quick Revision

```text
TRANSACTION
     ↓
Group of SQL operations
     ↓
START TRANSACTION
     ↓
Perform operations
     ↓
   ┌───────────────┐
   ↓               ↓
COMMIT          ROLLBACK
   ↓               ↓
Save             Undo
```

---

# 💡 Interview One-Liners

### What is a Transaction?

> A transaction is a group of database operations treated as a single unit of work.

### What is COMMIT?

> COMMIT permanently saves the changes made by the transaction.

### What is ROLLBACK?

> ROLLBACK undoes the uncommitted changes of the current transaction.

### What is SAVEPOINT?

> SAVEPOINT creates a point inside a transaction to which we can partially roll back.

### What is ACID?

> ACID stands for Atomicity, Consistency, Isolation, and Durability, which are properties used to describe reliable transaction processing.

### What is Atomicity?

> Atomicity means a transaction is treated as a single unit, commonly described as all-or-nothing.

### What is Durability?

> Durability means committed changes persist according to the database's recovery guarantees.

---

# 📝 Practice Questions

### Q1.
Write a transaction to transfer ₹500 from account 1 to account 2.

### Q2.
Write a transaction containing two `UPDATE` statements and commit the changes.

### Q3.
Write a transaction and undo the changes using `ROLLBACK`.

### Q4.
Create a `SAVEPOINT` after the first update and roll back to it.

### Q5.
What are the four ACID properties?

### Q6.
Explain the difference between `COMMIT` and `ROLLBACK`.

### Q7.
What is the purpose of `SAVEPOINT`?

---

# ⭐ Key Point

```text
START TRANSACTION → Start
COMMIT             → Save
ROLLBACK           → Undo
SAVEPOINT          → Create rollback point
```

### 🧠 ACID Memory Trick

```text
A → All or Nothing
C → Correct / Valid State
I → Independent Transactions
D → Data stays after Commit
```
