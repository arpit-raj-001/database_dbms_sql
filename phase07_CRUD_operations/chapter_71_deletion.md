<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗑️ DELETE Basics & Subqueries</h1>
  <h2>Chapter 71</h2>
</div>

---

## 🧐 DELETE Basics

`DELETE` removes rows from a table.

**General syntax:**
```sql
DELETE FROM table_name;
```

> [!NOTE]
> Unlike `DROP`, the table structure remains.
> - Finds matching rows.
> - Locks those rows.
> - Marks them for deletion.
> - Writes changes to the transaction log.
> - Frees the space later (depends on the storage engine).

---

## ⚠️ DELETE Without WHERE

```sql
DELETE FROM Students;
```
> [!WARNING]
> Every row is deleted.

---

## ⚖️ DELETE vs TRUNCATE

| Feature | DELETE | TRUNCATE |
| :--- | :--- | :--- |
| **Removes** | Selected rows or all rows | All rows |
| **WHERE** | ✅ Yes | ❌ No |
| **Logged** | Row by row | Minimal logging |
| **Rollback (InnoDB transaction)**| ✅ Yes | Depends on DBMS; MySQL treats it as DDL |
| **AUTO_INCREMENT reset**| ❌ No | ✅ Yes (MySQL) |
| **Speed** | Slower | Faster |

---

## 🧩 DELETE Using Subqueries

**Example:**
```sql
DELETE FROM Employees
WHERE EmployeeID IN (
 SELECT EmployeeID
 FROM FormerEmployees
);
```

---

## 🌊 Cascading Delete

**Suppose:**
- **Parent Table:** Departments
- **Child Table:** Employees

**Foreign key:**
```sql
FOREIGN KEY (DepartmentID)
REFERENCES Departments(DepartmentID)
ON DELETE CASCADE
```

**Now:**
```sql
DELETE FROM Departments
WHERE DepartmentID = 10;
```
> [!TIP]
> **Automatically deletes:**
> - Department 10
> - Every employee belonging to Department 10

---

## 🛑 Without ON DELETE CASCADE

The deletion fails if child rows exist.

---

## 🛡️ Soft Delete

Large applications often don't actually delete rows.

**Instead:**
```sql
UPDATE Students
SET is_deleted = TRUE
WHERE StudentID = 1;
```

**Then all queries become:**
```sql
SELECT *
FROM Students
WHERE is_deleted = FALSE;
```

> [!TIP]
> **Benefits:**
> - Recover deleted data
> - Audit history
> - Compliance
> - No accidental permanent deletion

---

## 🔙 Recovering Deleted Data

### Transactions
```sql
START TRANSACTION;
DELETE FROM Students
WHERE StudentID = 1;
```
**Oops! Recover:**
```sql
ROLLBACK;
```
*Everything is restored.*

**Save permanently:**
```sql
COMMIT;
```

### Backups
If the transaction is already committed, recovery usually requires:
- Database backups
- Binary logs (MySQL)

> [!NOTE]
> **Can DELETE be rolled back?**
> Yes, if executed inside a transaction (for transactional storage engines like InnoDB) and before `COMMIT`.

> [!NOTE]
> **What is ON DELETE CASCADE?**
> Automatically deletes child rows when the parent row is deleted.
