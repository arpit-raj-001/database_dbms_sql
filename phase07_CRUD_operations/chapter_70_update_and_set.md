<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔄 UPDATE Basics</h1>
  <h2>Chapter 70</h2>
</div>

---

The `UPDATE` statement modifies existing rows in a table.

**General syntax:**
```sql
UPDATE table_name
SET column_name = value;
```

**Suppose we execute:**
```sql
UPDATE Students
SET Age = 22;
```

### MySQL performs:
1. Read the table.
2. Find all rows *(no `WHERE` clause)*.
3. Lock the rows *(depending on storage engine and transaction isolation)*.
4. Modify the values.
5. Write changes to disk.
6. Write to the transaction log *(redo/undo logs)*.

---

## 🎯 Updating a Single Column

**Example:**
```sql
UPDATE Students
SET Age = 21
WHERE StudentID = 1;
```

## 🎯 Updating Multiple Columns
*Separate columns using commas.*

```sql
UPDATE Students
SET Age = 21, Department = 'ECE'
WHERE StudentID = 1;
```

---

## 🧩 Multiple Conditions

```sql
UPDATE Students
SET Department = 'ECE'
WHERE Age > 20
AND CGPA > 8;
```

```sql
UPDATE Students
SET Department = 'ECE'
WHERE StudentID IN (1, 3, 5);
```

```sql
UPDATE Employees
SET Salary = Salary + 5000
WHERE Salary BETWEEN 40000 AND 60000;
```

> [!WARNING]
> **What if you forget the `WHERE` clause?**
> ```sql
> UPDATE Employees
> SET Salary = 100000;
> ```
> *Every employee now has the same salary!*
> This is one of the most common production mistakes.

---

## 🔀 Updating Using CASE

Useful for conditional updates.

**Example:**
```sql
UPDATE Employees
SET Salary =
CASE
 WHEN Salary < 50000 THEN Salary * 1.20
 WHEN Salary < 100000 THEN Salary * 1.10
 ELSE Salary * 1.05
END;
```

**Example 2:**
```sql
UPDATE Students
SET Grade =
CASE
 WHEN CGPA >= 9 THEN 'A'
 WHEN CGPA >= 8 THEN 'B'
 ELSE 'C'
END;
```

---

## ❌ Updating NULL Values

**Suppose:**
| Name | Phone |
| :--- | :--- |
| `aadz` | `NULL` |

**Update missing phone numbers:**
```sql
UPDATE Students
SET Phone = '9999999999'
WHERE Phone IS NULL;
```

---

## 🛡️ Safe Update Mode

If you run:
```sql
UPDATE Students
SET Age = 20;
```
You may see:
`Error Code: 1175. You are using safe update mode...`

**Safe Update Mode prevents updates that:**
- Don't use a `WHERE` clause with a key column.
- Or don't use `LIMIT`.

**To temporarily disable it:**
```sql
SET SQL_SAFE_UPDATES = 0;
```
**Re-enable:**
```sql
SET SQL_SAFE_UPDATES = 1;
```

---

## ⚖️ Difference between UPDATE and ALTER?
- **UPDATE** changes data.
- **ALTER** changes the table structure.

---

## 💻 Advanced Scenario

**The Scenario:** You have two tables: `Users` and `Banned_IPs`. You need to lock the accounts of any user who logged in from a banned IP address.
**Concepts used:** `UPDATE`, `SET`, `WHERE`, `IN` (Subquery)

**The Solution:**
```sql
UPDATE Users
SET is_locked = True
WHERE ip_address IN (
 SELECT ip_address 
 FROM Banned_IPs
);
```
