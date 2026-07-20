<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>✔️ SQL Constraints: CHECK and DEFAULT</h1>
  <h2>Chapter 77</h2>
</div>

---

## ✅ CHECK Constraint

A `CHECK` constraint ensures that inserted or updated data satisfies a specified Boolean condition.

```sql
CREATE TABLE Employees(
 EmployeeID INT PRIMARY KEY,
 Salary DECIMAL(10,2),
 CHECK (Salary > 0)
);
```

**Now:**
If `Salary = -50000`, the database rejects it.

**Examples:**
- `CHECK (Percentage BETWEEN 0 AND 100)`
- `CHECK (Quantity >= 0)`

---

## 🧩 Complex Conditions

A `CHECK` expression can combine multiple conditions.

```sql
CHECK (
 Department='ECE' OR Department='CSE'
)
```

---

## ➕ Multi-Column CHECK

One of the biggest advantages of table-level `CHECK`.

```sql
CREATE TABLE Projects(
 StartDate DATE,
 EndDate DATE,
 CHECK (StartDate < EndDate)
);
```

---

## 🛠️ CHECK with Functions

Many databases allow functions inside `CHECK` expressions.

```sql
CHECK (
 CHAR_LENGTH(Name) >= 3
)
```

```sql
CHECK (
 YEAR(BirthDate) >= 1900
)
```

---

## 🛑 Vendor Limitations
*This is an interview favorite.*

**MySQL:**
- Versions before 8.0.16 parsed `CHECK` constraints but ignored them.
- From 8.0.16 onward, `CHECK` constraints are enforced.

---

## 🔄 Internal Flow

**Suppose:**
```sql
UPDATE Students SET Age=-5 WHERE StudentID=1;
```

**Flow:**
1. Receive `UPDATE`
2. Modify row temporarily
3. Evaluate `CHECK`
4. True?
   - **Yes** → Save
   - **No** → Reject

> [!NOTE]
> `CHECK` constraints are evaluated only during `INSERT` and `UPDATE`. They do not slow down normal `SELECT` queries.

---

## 🌟 DEFAULT Constraint

A `DEFAULT` constraint supplies a predefined value when no value is explicitly provided for a column.

### Syntax
```sql
CREATE TABLE Users(
 UserID INT PRIMARY KEY,
 Country VARCHAR(50) DEFAULT 'India'
);
```

### CURRENT_DATE
```sql
JoiningDate DATE DEFAULT (CURRENT_DATE)
```

### CURRENT_TIMESTAMP
```sql
CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```
*This is one of the most common defaults used in production.*

---

## ⚠️ Advanced Usage

MySQL **does not** allow arbitrary functions like `UUID()` directly as a column `DEFAULT`.
**A common approach is:**
```sql
INSERT INTO Users(UserID) VALUES (UUID());
```

### ALTER DEFAULT
```sql
ALTER TABLE Users
MODIFY Country VARCHAR(50) DEFAULT 'USA';
```

### Remove Default
```sql
ALTER TABLE Users
MODIFY Country VARCHAR(50);
```

> [!TIP]
> If `NULL` is explicitly inserted, the default is not used.
