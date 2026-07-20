<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>⚙️ SQL Execution & Internal Flow (WHERE, AND, OR, NOT)</h1>
  <h2>Chapter 81</h2>
</div>

---

A SQL query is a statement that requests data or instructs the database to perform an operation.

## 📝 Query vs Statement
*This interview question appears surprisingly often.*

- **Statement:** A SQL statement is any valid SQL command.
- **Query:** A query is generally a SQL statement that asks the database for information.

```sql
SELECT * FROM Students;
```
*The database does much more than simply read the table.*

---

## 🌊 Internal flow:

```text
       User
        ↓
    SQL Query
        ↓
      Parser
        ↓
   Syntax Check
        ↓
    Optimizer
        ↓
  Execution Plan
        ↓
  Storage Engine
        ↓
  Rows Retrieved
        ↓
  Result Returned
```

---

### STEP 1 — PARSER
Checks:
- SQL syntax
- keywords
- table names
- column names

### STEP 2 — OPTIMIZER
Decides:
- Which index to use
- Whether to scan the whole table
- Join order (later)

### STEP 3 — STORAGE ENGINE
Actually reads data from disk or memory.

---

## ⏱️ Execution Order

```text
FROM
 ↓
WHERE
 ↓
SELECT
 ↓
DISTINCT
 ↓
ORDER BY
 ↓
LIMIT
```

**Why Isn't SQL Written in Execution Order?**
Because SQL is a declarative language. You describe what you want, not how to get it.

---

## 🔍 What is the WHERE Clause?

The `WHERE` clause filters rows. It tells the database:
*"Return only those rows that satisfy this condition."*

### Internal Working

**Suppose:**
```sql
SELECT *
FROM Employees
WHERE Salary > 60000;
```

**Logical execution:**
1. `FROM Employees`
2. `Read Row 1`
3. `Salary > 60000 ?`
4. `Yes → Return`
5. `Read Row 2`
6. `Salary > 60000 ?`
7. `No → Skip`
8. `Continue until end`

If no suitable index exists, this is called a **table scan** (or full table scan).
If an appropriate index exists, the database may jump directly to matching rows instead.

Every `WHERE` condition ultimately evaluates to one of three values:
- `TRUE`
- `FALSE`
- `UNKNOWN (NULL)`

*Only rows with `TRUE` survive.*

---

## ⚠️ NOT UNKNOWN = TRUE
*This is incorrect. It remains UNKNOWN.*

**Wrong:**
```sql
WHERE NOT Department='ECE'
AND Salary>50000;
```
This is interpreted as:
```sql
WHERE (NOT Department='ECE')
AND Salary>50000;
```
If your intent is different, use parentheses.
**Example:**
```sql
WHERE NOT
(
 Department='ECE'
 AND Salary>50000
);
```

---

## 🧮 SQL Operator Precedence

For the operators you've learned so far, the order is:

1. **NOT**
2. **AND**
3. **OR**
