<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📈 Aggregate Functions (Part 2)</h1>
  <h2>Chapter 97</h2>
</div>

---

## ➕ Chapter 3 — SUM()

### 3.1 Introduction
Adding numeric values.

### 3.6 NULL Behavior
**Empty table:**
```sql
SELECT SUM(salary) FROM Employee;
```
**Output:** `NULL`

Unlike `COUNT()`, `SUM()` returns `NULL` when there are no non-NULL values to add.

**Using COALESCE:**
```sql
SELECT COALESCE(SUM(salary), 0) FROM Employee;
```
**Output:** `0`

---

## ➗ Chapter 4 — AVG()

### 4.1 Introduction
Average values.

### 4.7 Rounding Results
`AVG` often returns long decimal values. (e.g., `200.246666666`)
**Use ROUND:**
```sql
SELECT ROUND(AVG(price), 2) FROM Product;
```

---

## 📉 Chapter 5 — MIN()

### 5.1 Introduction
Smallest value.

**Basic syntax:**
```sql
SELECT MIN(column_name) FROM table_name;
```

### 5.4 Strings
Alphabetical order. `MIN` compares strings lexicographically (dictionary order).

### 5.5 Dates
Earliest date. Dates are compared chronologically.

### 5.7 Index Optimization
```sql
SELECT MIN(id) FROM Employee;
```
If an index exists, the database can often find the first (smallest) indexed value without scanning the whole table.

**Without an index:** Read every row → Compare values → Return minimum *(Slow)*
**With an index:** Go to first entry → Return value *(Much faster)*

---

## 📈 Chapter 6 — MAX()

### 6.1 Introduction
Largest value.

### 6.7 Index Optimization
With an index, the database can often retrieve the last (largest) indexed value instead of scanning the entire table.

---

## 📏 Chapter 7 — STDDEV()

### 7.1 Introduction
Standard deviation.

### 7.3 Population vs Sample
Two versions exist:
- `STDDEV_POP()`: Population standard deviation. Use when the table contains every data point. *(divide by N)*
- `STDDEV_SAMP()`: Sample standard deviation. Use when you only have a sample. *(divide by N-1)*

*Sample standard deviation is usually slightly larger.*

**MySQL Syntax:**
```sql
SELECT STDDEV(salary) FROM Employee;
-- or
SELECT STDDEV_POP(salary) FROM Employee;
-- or
SELECT STDDEV_SAMP(salary) FROM Employee;
```

---

## 📐 Chapter 8 — VARIANCE()

### 8.1 Introduction
Variance measures spread just like standard deviation.
**Formula:** `Variance = STDDEV²`

### 8.2 Population vs Sample
- `VAR_POP()`
- `VAR_SAMP()`

---

## 🎯 Chapter 9 — MEDIAN()

### 9.1 Introduction
Middle value.
Median is the middle value after sorting. Unlike `AVG`, Median is not affected much by outliers.

### 9.5 Vendor Support
**MySQL:**
- No built-in `MEDIAN`.
- Need window functions.

### 9.6 Manual Median Calculation
**Concept:**
- `ROW_NUMBER()`
- `COUNT()`
- Middle row

---

## 📊 Chapter 10 — MODE()

### 10.1 Introduction
Most frequent value.
**Example:** `10, 20, 20, 30, 20, 40`
**Mode:** `20`

### 10.6 Practical Examples
**Most common grade:**
```sql
SELECT grade, COUNT(*)
FROM Student
GROUP BY grade
ORDER BY COUNT(*) DESC
LIMIT 1;
```
