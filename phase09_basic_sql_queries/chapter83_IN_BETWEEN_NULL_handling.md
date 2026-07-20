<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>⚖️ IN, BETWEEN, and NULL Handling</h1>
  <h2>Chapter 83</h2>
</div>

---

## 🎯 The IN Operator

The `IN` operator checks whether a value belongs to a given set of values.

```sql
SELECT *
FROM Students
WHERE StudentID IN
(SELECT StudentID
FROM ScholarshipStudents
);
```

**Meaning:** Return every student whose ID appears in the ScholarshipStudents table.

---

## ⚠️ NOT IN & NULL Pitfalls

`NOT IN` returns rows whose value does not belong to the specified set.

> [!WARNING]
> **NULL Pitfalls**
> *This is one of the most common SQL interview questions.*

**Suppose:**
```sql
WHERE Salary NOT IN (1000, 2000, NULL)
```

Many people expect: *All salaries except 1000 and 2000.*
**Wrong.**

Because SQL evaluates:
```sql
Salary <> 1000
AND
Salary <> 2000
AND
Salary <> NULL
```

But `Salary <> NULL` is **UNKNOWN**.
The entire condition becomes `UNKNOWN`, and no rows match.

---

## 📏 BETWEEN & NOT BETWEEN

`BETWEEN` checks whether a value lies within a specified range.

```sql
SELECT *
FROM Students
WHERE Name BETWEEN 'A' AND 'M';
```

### NOT BETWEEN

**Example:**
```sql
SELECT *
FROM Employees
WHERE Salary NOT BETWEEN 50000 AND 100000;
```

---

## 🤷‍♂️ What is NULL?

**Definition:** `NULL` means: The database does not know the value, or the value does not exist.

### Three-Valued Logic
SQL has three logical states.
| Result | Meaning |
| :--- | :--- |
| `TRUE` | Condition satisfied |
| `FALSE` | Condition not satisfied |
| `UNKNOWN` | Cannot determine because of `NULL` |

### Three-Valued Logic with OR

| A | B | A OR B |
| :--- | :--- | :--- |
| `TRUE` | `TRUE` | `TRUE` |
| `TRUE` | `FALSE` | `TRUE` |
| `TRUE` | `UNKNOWN`| `TRUE` |
| `FALSE` | `TRUE` | `TRUE` |
| `FALSE` | `FALSE` | `FALSE` |
| `FALSE` | `UNKNOWN`| `UNKNOWN` |
| `UNKNOWN`| `TRUE` | `TRUE` |
| `UNKNOWN`| `FALSE` | `UNKNOWN` |
| `UNKNOWN`| `UNKNOWN`| `UNKNOWN` |

---

## ➕ Aggregate Functions with NULL

Consider the following Salaries: `1000`, `2000`, `NULL`

### COUNT(*)
```sql
SELECT COUNT(*)
```
**Result:** `3` (Counts rows).

### COUNT(Salary)
```sql
SELECT COUNT(Salary)
```
**Result:** `2` (Ignores NULL).

### AVG
```sql
SELECT AVG(Salary)
```
**Average:** `(1000+2000)/2 = 1500` (NULL is ignored).

### SUM
```sql
SUM(Salary)
```
**Result:** `3000` (NULL is ignored).

### MIN/MAX
NULL values are ignored.

---

## 🧠 Internal Representation & Performance

Internally, MySQL stores a `NULL` bitmap for nullable columns. Conceptually, think of it as:

```text
      Row
       ↓
  NULL bitmap
       ↓
Column contains NULL?
       ↓
    Yes / No
```
*This allows the database to know whether a column contains a real value or not.*

### Performance Considerations

Searching with:
```sql
WHERE Column IS NULL
```
can use an index if one exists on the column (`COUNT(*)` counts all rows, `COUNT(column)` ignores `NULL` values).
