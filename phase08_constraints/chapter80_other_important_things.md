<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧩 Database Constraints Notes</h1>
  <h2>Chapter 80</h2>
</div>

---

## 🗑️ Drop Constraint

### Drop PRIMARY KEY
```sql
ALTER TABLE Students
DROP PRIMARY KEY;
```
*(Cannot drop it while dependent foreign keys exist).*

### Drop FOREIGN KEY (MySQL)
```sql
ALTER TABLE Orders
DROP FOREIGN KEY fk_customer;
```

### Drop CHECK
```sql
ALTER TABLE Employees
DROP CHECK chk_salary;
```

### Drop UNIQUE
In MySQL, a `UNIQUE` constraint is backed by a unique index.
```sql
ALTER TABLE Users
DROP INDEX uq_email;
```

---

## ✏️ Rename Constraint

### MySQL
MySQL does not provide a general `RENAME CONSTRAINT` statement.
Usually you:
1. Drop the old constraint.
2. Create it again with the new name.

### SQL Server
Supports renaming via stored procedures.

### PostgreSQL
```sql
ALTER TABLE Employees
RENAME CONSTRAINT old_name TO new_name;
```

---

## 🔓 Enable / Disable Constraints

Useful during:
- Bulk imports
- Data migration

### MySQL (Foreign keys)
**Disable:**
```sql
SET FOREIGN_KEY_CHECKS = 0;
```
**Later Enable:**
```sql
SET FOREIGN_KEY_CHECKS = 1;
```

### SQL Server
**Disable:**
```sql
ALTER TABLE Employees
NOCHECK CONSTRAINT ALL;
```
**Enable:**
```sql
ALTER TABLE Employees
CHECK CONSTRAINT ALL;
```

---

## 🚚 Migration Strategies

Typical production workflow:
1. Create new column
2. Copy data
3. Validate
4. Add constraints
5. Deploy application
6. Remove old column

**Why do named constraints matter?**
They make it much easier to:
- drop constraints,
- modify them,
- manage database migrations.

---

## 💾 How Constraints are Stored

Constraints are metadata. They are not stored inside every row. Instead, database keeps them in internal catalogs.

**Think of it like:**
`Table Data + Metadata`

**Metadata describes:**
- columns
- indexes
- constraints
- defaults
- foreign keys

### MySQL Information Schema
**Examples:**
```sql
SHOW CREATE TABLE Students;
```
*shows the table definition, including constraints.*

---

## 🗂️ Constraints and Indexes

| Constraint | Index Creation / Rules |
| :--- | :--- |
| **PRIMARY KEY** | Automatically creates Unique Index. |
| **UNIQUE** | Automatically creates Unique Index. |
| **FOREIGN KEY** | Needs Referenced column Indexed.<br><br>**In MySQL InnoDB:**<br>• The parent key must be indexed (typically PRIMARY KEY or UNIQUE).<br>• An index on the child foreign key is automatically created if one does not already exist. |
| **CHECK** | No index. Simply evaluates expression. |
| **DEFAULT** | No index. |
| **NOT NULL** | No index. |

---

## ⚡ Why do constraints help query optimization?

> [!TIP]
> Because they provide the optimizer with guarantees about the data, allowing it to generate more efficient execution plans.
