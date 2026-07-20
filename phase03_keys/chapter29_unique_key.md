<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>💎 UNIQUE Key</h1>
  <h2>Chapter 29</h2>
</div>

---

## 🧐 What is a Unique Key?

> [!NOTE]
> A **Unique Key** is a constraint that ensures all values in a column (or combination of columns) are unique. No two rows can have the same value for a Unique Key.

### A Unique Key:
- Prevents duplicate values.
- Can consist of one or more columns.
- Can have multiple constraints in one table.
- Is commonly used to enforce business rules.

---

## ❓ NULL Handling
*This is one of the most frequently asked interview topics.*

**Can a Unique Key contain NULL?**
**Answer:** Yes, but the exact behavior depends on the DBMS.

**Why?**
`NULL` means Unknown value. Unknown values are not considered equal under the SQL standard. Because of this, many databases allow multiple `NULL` values in a `UNIQUE` column.

**Example:**
| Email |
| :--- |
| `aadz@gmail.com` |
| `NULL` |
| `NULL` |

*This is accepted by many database systems.*

### 🛠️ DBMS Support for Multiple NULLs

| Database | Multiple NULLs in UNIQUE? |
| :--- | :--- |
| **MySQL** | ✅ Yes |
| **PostgreSQL** | ✅ Yes |
| **Oracle** | ✅ Yes |
| **SQL Server** | ⚠️ Traditionally only one `NULL` in a `UNIQUE` constraint *(behavior depends on implementation and indexing options)* |

---

## 🔢 Multiple Unique Keys in a Table
A table can have many Unique Keys.

**Possible constraints:**
- `PRIMARY KEY(EmployeeID)`
- `UNIQUE(Email)`
- `UNIQUE(Phone)`
- `UNIQUE(Username)`

### SQL Examples

**Creating a Unique Key**
```sql
CREATE TABLE Employee
(
 EmployeeID INT PRIMARY KEY,
 Email VARCHAR(100) UNIQUE,
 Name VARCHAR(50)
);
```

**Multiple Unique Keys**
```sql
CREATE TABLE UserAccount
(
 UserID INT PRIMARY KEY,
 Email VARCHAR(100) UNIQUE,
 Username VARCHAR(50) UNIQUE,
 Phone VARCHAR(20) UNIQUE
);
```

**Composite Unique Key**
```sql
CREATE TABLE Student
(
 RollNo INT,
 Semester INT,
 UNIQUE(RollNo, Semester)
);
```
*This prevents duplicate `(RollNo, Semester)` combinations.*

---

## 🔗 Relationship to Foreign Key
A Foreign Key may reference a Unique Key (not just a Primary Key), because it also guarantees uniqueness.

---

## 📉 Disadvantages

### Disadvantage 1 — Business Rules May Change
Suppose Phone Number is unique today. Tomorrow, the company allows family members to share one phone number. The Unique constraint must be removed.

### Disadvantage 2 — Additional Indexes
Most DBMSs create an index to enforce a Unique constraint. Having many Unique Keys can increase storage and slow down insert/update operations because additional indexes must be maintained.
