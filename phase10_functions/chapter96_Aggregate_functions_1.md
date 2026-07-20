<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📊 Aggregate Functions (Part 1)</h1>
  <h2>Chapter 96</h2>
</div>

---

## 📖 Chapter 1 — Introduction to Aggregate Functions

### 1.1 What are Aggregate Functions?
**Definition:** Single-value output from multiple rows.

**Example:**
```sql
SELECT SUM(salary)
FROM Employee;
```
SQL looks at all four salary values (50000, 70000, 65000, 80000) and produces only one value: **265000**.

| Function | Purpose |
| :--- | :--- |
| `COUNT()` | Count rows |
| `SUM()` | Add values |
| `AVG()` | Average |
| `MIN()` | Smallest value |
| `MAX()` | Largest value |

*Almost every SQL database supports these.*

**Instagram (Total followers):**
```sql
SELECT COUNT(*) FROM Followers;
```
**Netflix (Average movie rating):**
```sql
SELECT AVG(rating) FROM Movies;
```

---

### 1.2 Aggregate Functions vs Scalar Functions

| A scalar function | Aggregate function |
| :--- | :--- |
| e.g., `UPPER(name)` | e.g., `SUM(salary)` |
| looks at **One row** | looks at **Entire table** |
| ↓ | ↓ |
| One output (per row) | One output (for the group/table) |

> [!IMPORTANT]
> **This difference is extremely important.**
> Row-wise operations vs Group-wise operations.

---

### 1.3 Logical Query Execution

Where do aggregate functions execute in the SQL pipeline?
Aggregate functions are **not** executed immediately. They execute **after** `WHERE` has filtered the rows.

> [!WARNING]
> **Why Can't Aggregate Functions Be Used in WHERE?**
> Many beginners write:
> ```sql
> SELECT * FROM Employee WHERE AVG(salary) > 50000;
> ```
> *This produces an error.*
> **Why?** Because `WHERE` executes **BEFORE** `AVG()`. At the time `WHERE` is running, `AVG` hasn't even been calculated yet.

---

### 1.4 NULL Handling
> [!NOTE]
> Almost every aggregate function ignores `NULL` values. That means `NULL` is skipped during calculations.

---

### 1.5 Aggregate Function Categories
- Counting
- Summation
- Averaging
- Extremes
- Statistical aggregates

---

## 🔢 Chapter 2 — COUNT()

### 2.1 Introduction
Counting rows.

### 2.3 COUNT(*)
**Count all rows.**
`COUNT(*)` This is the exception. `COUNT(*)` counts rows. It does not care whether columns are `NULL`.

### 2.4 COUNT(column)
**Ignore `NULL` values.**
`COUNT(column)` is different.
```sql
SELECT COUNT(email) FROM Employee;
```
*Here, COUNT only counts **Non-NULL** email values.*

### 2.5 COUNT(DISTINCT)
**Unique values only.**
`COUNT(DISTINCT)` counts the number of unique (different) non-NULL values in a column.
```sql
SELECT COUNT(DISTINCT city) FROM Student;
```

**COUNT(DISTINCT Multiple Columns):**
```sql
SELECT COUNT(DISTINCT city, state) FROM Customers;
```
*Counts unique (city, state) pairs.*

---

### 2.6 COUNT with Expressions

**Query:**
```sql
SELECT COUNT(salary > 50000) FROM Employee;
```

> [!WARNING]
> **Important Vendor Behavior**
> This is not portable SQL.
> - **MySQL** treats TRUE and FALSE as non-NULL values (internally 1 and 0), so this query returns 3, because every row produces either TRUE or FALSE—not NULL.
> - **PostgreSQL** also evaluates the boolean expression for every non-NULL salary, so `COUNT(expression)` counts all non-NULL boolean results.
> If salary itself is `NULL`, then `salary > 50000` evaluates to `NULL`, and that row is not counted.
> **Therefore, do not use `COUNT(boolean_expression)` when you want to count matching rows.**

**Correct Way:**
```sql
SELECT COUNT(*) FROM Employee WHERE salary > 50000;
```

---

### COUNT(CASE...)
This is one of the most useful SQL techniques.

**Example Table:**
| Name | Department |
| :--- | :--- |
| `aadz` | `IT` |
| `Arpit` | `HR` |

**Query:**
```sql
SELECT COUNT(
 CASE
 WHEN department='IT' THEN 1
 END
)
FROM Employee;
```

**MySQL Shortcut:**
Instead of `CASE`, MySQL developers often write:
```sql
COUNT(IF(department='IT', 1, NULL))
```

---

### Summary

| Function | Empty Table |
| :--- | :--- |
| `COUNT(*)` | `0` |
| `COUNT(column)` | `0` |
| `COUNT(DISTINCT)`| `0` |
| `SUM()` | `NULL` |
| `AVG()` | `NULL` |
| `MIN()` | `NULL` |
| `MAX()` | `NULL` |
