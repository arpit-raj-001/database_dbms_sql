<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔙 SQL RETURNING Clause</h1>
  <h2>Chapter 72</h2>
</div>

---

> [!IMPORTANT]
> **RETURNING is not a core MySQL feature.**
> It is a first-class feature in PostgreSQL, SQLite (recent versions), Oracle, and SQL Server (through `OUTPUT`).

---

## 🤔 Why RETURNING Exists

Normally, when you execute an `INSERT`, `UPDATE`, or `DELETE`, SQL tells you only how many rows were affected.

**Example:**
```sql
UPDATE Employees
SET Salary = Salary * 1.10
WHERE EmployeeID = 5;
```

**Output:**
```text
Query OK
1 row affected
```

But what if you immediately want to know:
- Which row was inserted?
- What is the generated ID?
- What is the updated salary?
- Which row got deleted?

Without `RETURNING`, you often need an additional `SELECT`.
**`RETURNING` combines both operations.**

---

## 🐘 PostgreSQL Examples

### RETURNING *
```sql
INSERT INTO Students(Name, Age)
VALUES ('aadz', 20)
RETURNING *;
```

**Output:**
| StudentID | Name | Age | CreatedAt |
| :--- | :--- | :--- | :--- |
| `101` | `aadz` | `20` | `2026-07-18 10:15:20` |

### Returning only specific columns
```sql
INSERT INTO Students(Name, Age)
VALUES ('aadz', 20)
RETURNING StudentID;
```

**Output:**
| StudentID |
| :--- |
| `101` |

### Return selected columns (UPDATE)
```sql
UPDATE Employees
SET Salary = Salary + 5000
WHERE EmployeeID = 5
RETURNING EmployeeID, Salary;
```

### Returning from DELETE
```sql
DELETE FROM Employees
WHERE EmployeeID = 5
RETURNING *;
```

**Output:**
| EmployeeID | Name |
| :--- | :--- |
| `5` | `Arpit` |

> [!TIP]
> The row is deleted and returned. Very useful for:
> - Audit logs
> - Undo features
> - Backups

---

## 🐬 MySQL

Normally you do:
```sql
UPDATE Employees
SET Salary = Salary + 5000
WHERE EmployeeID = 5;
```
Then:
```sql
SELECT *
FROM Employees
WHERE EmployeeID = 5;
```

---

## ⚖️ Comparison: RETURNING vs SELECT

| RETURNING | SELECT |
| :--- | :--- |
| Returns affected rows | Reads any rows |
| Works with `INSERT/UPDATE/DELETE` | Works independently |
| One query | Usually two queries |

---

## ⚙️ Internal Working

1. Find row
2. Update row
3. Before sending success message
4. Return requested columns
5. Finish query
