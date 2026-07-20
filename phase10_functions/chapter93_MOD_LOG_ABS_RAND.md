<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>➗ SQL Functions: MOD, SQRT, RAND, LN</h1>
  <h2>Chapter 93</h2>
</div>

---

## ➗ MOD()

### Syntax
```sql
MOD(dividend, divisor)
```

### Even/Odd Detection
*The most common interview application.*

**Even numbers:**
```sql
SELECT *
FROM Students
WHERE MOD(student_id, 2) = 0;
```

**Odd numbers:**
```sql
SELECT *
FROM Students
WHERE MOD(student_id, 2) = 1;
```

### Difference between % and MOD()?
`MOD()` is a function and is more portable across SQL dialects, while `%` is an operator supported by some databases.

---

## ➕ ABS() & SQRT()

`ABS()` returns the absolute value of a number.
`SQRT()` computes the square root.

```sql
SELECT ROUND(SQRT(20), 2);
```
**Result:** `4.47`

> [!WARNING]
> **Very important:**
> ```sql
> SQRT(-25)
> ```
> Mathematically impossible in real numbers.
> Most databases return `NULL` or `Error`, depending on the vendor.

---

## 🎲 RANDOM()

### 9.1 Introduction
Random number generation syntax varies by database.

| Database | Function |
| :--- | :--- |
| **MySQL** | `RAND()` |
| **PostgreSQL** | `RANDOM()` |
| **SQLite** | `RANDOM()` |
| **SQL Server** | `RAND()` |

### Random Integer (1–100)

**MySQL:**
```sql
SELECT FLOOR(RAND() * 100) + 1;
```

### Random Row Selection

**MySQL:**
```sql
SELECT *
FROM Students
ORDER BY RAND()
LIMIT 1;
```

**ORDER BY RANDOM()**
Easy but expensive. The database assigns a random value to every row and sorts the entire result set.

---

## 📈 Logarithmic Functions

### Syntax

**Natural logarithm:**
```sql
LN(number)
```

**Base-10 logarithm:**
```sql
LOG10(number)
```

**General logarithm (syntax varies by vendor):**
```sql
LOG(base, number)
```

**Example:**
```sql
SELECT LOG(2, 8);
```
**Result:** `3`
