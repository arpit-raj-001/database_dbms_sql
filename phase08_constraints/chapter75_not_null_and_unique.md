<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🛡️ NOT NULL and UNIQUE</h1>
  <h2>Chapter 75</h2>
</div>

---

## 🛑 NOT NULL

### Syntax
**Column-Level:**
```sql
CREATE TABLE Employees(
 EmployeeID INT PRIMARY KEY,
 Name VARCHAR(100) NOT NULL
);
```
*(Most common syntax).*

**Table-Level:**
> [!WARNING]
> Unlike `PRIMARY KEY` or `UNIQUE`, `NOT NULL` cannot be declared as a table-level constraint in standard SQL. It is always attached directly to a column.

---

### Internal Working: NULL Bitmap

Most database engines keep a small bitmap.
Imagine:
| Name | Age | Salary | Phone |
| :--- | :--- | :--- | :--- |
| `0` | `1` | `0` | `1` |

**Could mean:**
| NOT NULL | NULL | NOT NULL | NULL |
| :--- | :--- | :--- | :--- |

> [!TIP]
> The actual implementation differs by database engine, but conceptually each row keeps metadata indicating which nullable columns contain `NULL`.

---

### Validation Process

**Suppose:**
```sql
INSERT INTO Employees(Name) VALUES(NULL);
```

**Flow:**
1. Receive `INSERT`
2. Read table schema
3. Is Name `NOT NULL`?
4. **YES** → Value = `NULL`? → **YES** → Reject statement.

---

### Existing NULL Values

**Suppose:**
```sql
Name VARCHAR(100)
```
Later:
```sql
ALTER TABLE Employees
MODIFY Name VARCHAR(100) NOT NULL;
```
If `NULL` already exists, the `ALTER TABLE` fails because existing rows violate the new constraint.

```sql
UPDATE Employees SET Name='Unknown' WHERE Name IS NULL;
```
**Then:**
```sql
ALTER TABLE Employees
MODIFY Name VARCHAR(100) NOT NULL;
```
> [!NOTE]
> Every primary key is automatically `NOT NULL`.

---

## 💎 UNIQUE Constraint

### 1. Introduction

**Purpose:**
Ensure that no two rows contain the same value in a column.

| PRIMARY KEY | UNIQUE |
| :--- | :--- |
| Only one per table | Many allowed |
| Never `NULL` | May allow `NULL`s (DBMS dependent) |
| Main identifier | Alternate identifier |
| Automatically `NOT NULL` | Nullable unless specified otherwise |

---

### 2. Syntax

**Single Column:**
```sql
CREATE TABLE Users(
 Email VARCHAR(100) UNIQUE
);
```

**Multiple Columns:**
```sql
CREATE TABLE Employees(
 CompanyID INT,
 Email VARCHAR(100),
 UNIQUE(CompanyID, Email)
);
```
*Each (CompanyID, Email) combination must be unique.*

**Named Constraint:**
```sql
CREATE TABLE Users(
 Email VARCHAR(100),
 CONSTRAINT uq_email UNIQUE(Email)
);
```
*Useful for maintenance and debugging.*

---

### 3. NULL Handling ⭐⭐⭐⭐⭐
*One of the most asked interview topics.*

#### MySQL
```sql
Email UNIQUE
```
**Allowed:**
- `NULL`
- `NULL`
- `NULL`
**Yes.** Multiple `NULL`s are allowed because `NULL` is considered "unknown," and unknown values are not treated as equal.

#### PostgreSQL
Same behavior. Multiple `NULL`s allowed.

#### SQL Server
Different. A traditional `UNIQUE` constraint allows only one `NULL` value.

---

### 4. Adding UNIQUE Later

```sql
ALTER TABLE Users
ADD CONSTRAINT uq_email
UNIQUE(Email);
```
If duplicates already exist, the statement fails until the duplicate data is cleaned up.

### 5. Removing UNIQUE

```sql
ALTER TABLE Users
DROP INDEX uq_email;
```
*(MySQL syntax).*
