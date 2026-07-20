<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🛑 SQL Limit & Offset Notes</h1>
  <h2>Chapter 87</h2>
</div>

---

## 🛑 LIMIT

```sql
SELECT *
FROM Employees
LIMIT 10;
```
*Returns only 10 rows.*

**Lowest 10 salaries:**
```sql
SELECT *
FROM Employees
ORDER BY Salary ASC
LIMIT 10;
```

**What happens if you omit ORDER BY?**
```sql
SELECT *
FROM Employees
LIMIT 5;
```
- **Is this wrong?** No.
- **Is it deterministic?** No. The database does not guarantee which five rows you get.

**Why is this dangerous?**
Storage engines may:
- change execution plans
- rebuild indexes
- insert new rows
- reorganize pages

As a result, `LIMIT 5` may return different rows over time.

---

## 🌐 SQL Dialects

| Dialect | Syntax |
| :--- | :--- |
| **MySQL / PostgreSQL** | `SELECT * FROM Employees LIMIT 5;` |
| **SQL Server** | `SELECT TOP 5 * FROM Employees;` |
| **Oracle** | `SELECT * FROM Employees FETCH FIRST 5 ROWS ONLY;` |

---

## ⏭️ OFFSET

`OFFSET` tells SQL: Skip the first N rows, then return the remaining rows. It is commonly used for pagination.

```sql
SELECT *
FROM Employees
LIMIT 5
OFFSET 10;
```
**Meaning:** Skip 10 rows, Then return 5 rows.

### MySQL Shortcut
Equivalent syntax:
```sql
SELECT *
FROM Employees
LIMIT 10, 5;
```
*(Skip 10, Limit 5)*

---

## 📄 Pagination Basics

Suppose a website displays 10 employees per page.

| Page | Query | Rows |
| :--- | :--- | :--- |
| **Page 1** | `LIMIT 10 OFFSET 0` | Rows 1-10 |
| **Page 2** | `LIMIT 10 OFFSET 10` | Rows 11-20 |
| **Page 3** | `LIMIT 10 OFFSET 20` | Rows 21-30 |

> [!NOTE]
> **Formula:**
> `OFFSET = (Page - 1) × PageSize`

---

## 🔑 Keyset Pagination

Using `OFFSET 100000` is very slow. Instead of `OFFSET`, large systems use **keyset pagination**:

```sql
WHERE ID > last_seen_id
ORDER BY ID
LIMIT 20;
```
