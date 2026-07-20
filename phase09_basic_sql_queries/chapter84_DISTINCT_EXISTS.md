<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🎯 DISTINCT and EXISTS</h1>
  <h2>Chapter 84</h2>
</div>

---

## 🌟 What is DISTINCT?

`DISTINCT` removes duplicate rows from the result.

**Suppose a table contains:**
| Student | Department |
| :--- | :--- |
| `aadz` | `ECE` |
| `Arpit` | `ECE` |
| `Riya` | `CSE` |

```sql
SELECT DISTINCT Department
FROM Students;
```
**Result:**
| Department |
| :--- |
| `ECE` |
| `CSE` |

---

### DISTINCT on Multiple Columns
*This is often misunderstood.*

**Suppose:**
| Name | Department |
| :--- | :--- |
| `aadz` | `ECE` |
| `aadz` | `ECE` |
| `aadz` | `IT` |
| `Arpit`| `ECE` |

**Query:**
```sql
SELECT DISTINCT Name, Department
FROM Students;
```

**Result:**
| Name | Department |
| :--- | :--- |
| `aadz` | `ECE` |
| `aadz` | `IT` |
| `Arpit`| `ECE` |

> [!NOTE]
> `DISTINCT` considers the entire row of selected columns, not each column independently.

**Example:**
```sql
SELECT DISTINCT City, State
FROM Customers;
```
*Even if the city repeats, different states create different combinations.*

---

### DISTINCT with NULL
*Another favorite interview question.*

**Suppose Phone:**
`NULL`, `NULL`, `98765`, `91234`

**Query:**
```sql
SELECT DISTINCT Phone
FROM Students;
```

**Result:**
| Phone |
| :--- |
| `NULL` |
| `98765`|
| `91234`|

Although `NULL ≠ NULL` for comparisons, `DISTINCT` treats all `NULL` values as one unique value in the output.

Removing duplicates requires additional work. The optimizer may use:
- Sorting
- Hashing
*Large tables with many duplicate values require more processing.*

---

## 🔍 EXISTS

> [!IMPORTANT]
> `EXISTS` almost always appears together with subqueries.

### What is EXISTS?
`EXISTS` checks whether a subquery returns at least one row.
It does not care:
- what values are returned,
- how many columns are returned,
- or what those values are.

It only asks one question: **Did the subquery return at least one row?**
- If yes → `TRUE`
- Otherwise → `FALSE`

### NOT EXISTS
Opposite of `EXISTS`. Returns rows where no matching row exists.

---

### Performance Advantages
Suppose a customer has 10,000 orders. `EXISTS` stops immediately after finding the first matching row.

```text
Found one row?
      ↓
Stop searching.
```
*Because of this, `EXISTS` is often more efficient than alternatives when checking existence.*

---

## 🏢 Real-World Examples

### Customers with orders
```sql
SELECT *
FROM Customers c
WHERE EXISTS (
 SELECT 1
 FROM Orders o
 WHERE o.CustomerID = c.CustomerID
);
```

We usually write `SELECT 1` to make the intention clear:
*"I don't care about the data. I only care whether a row exists."*

### Customers without orders
```sql
SELECT *
FROM Customers c
WHERE NOT EXISTS (
 SELECT 1
 FROM Orders o
 WHERE o.CustomerID = c.CustomerID
);
```

### Employees managing someone
```sql
SELECT *
FROM Employees e
WHERE EXISTS (
 SELECT 1
 FROM Employees m
 WHERE m.ManagerID = e.EmployeeID
);
```

*Sometimes `IN` is simpler.*
