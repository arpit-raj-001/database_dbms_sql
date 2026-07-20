<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔀 SQL Order By & Sorting Notes</h1>
  <h2>Chapter 86</h2>
</div>

---

By default, SQL does not guarantee the order of rows returned by a query.
If you want the results sorted, you must explicitly use the `ORDER BY` clause.

**Ascending:**
```sql
SELECT Name, Salary
FROM Employees
ORDER BY Salary ASC;
```

**Descending:**
```sql
SELECT *
FROM Employees
ORDER BY Salary DESC;
```

---

## 🔢 Multiple Column Sorting

```sql
SELECT *
FROM Employees
ORDER BY Department ASC,
Salary DESC;
```

**SQL sorts in stages:**
- **Step 1:** Sort by Department (ascending).
- **Step 2:** Within each department, Sort Salary descending.

**Using Expressions:**
```sql
SELECT Name, Price, Quantity
FROM Products
ORDER BY Price * Quantity DESC;
```

**Using Functions:**
```sql
SELECT FirstName, LastName
FROM Students
ORDER BY CONCAT(FirstName, ' ', LastName);
```

**Using Aliases:**
```sql
SELECT
 Name,
 Salary * 12 AS AnnualSalary
FROM Employees
ORDER BY AnnualSalary DESC;
```

---

## 🤷‍♂️ Handling NULLs in Sorting

**Suppose:**
| Name | Salary |
| :--- | :--- |
| `aadz` | `70000` |
| `Arpit`| `NULL` |
| `Riya` | `50000` |
| `Kabir`| `NULL` |

**MySQL Ascending:**
```sql
ORDER BY Salary ASC;
```

**Result:**
| Name | Salary |
| :--- | :--- |
| `Arpit`| `NULL` |
| `Kabir`| `NULL` |
| `Riya` | `50000` |
| `aadz` | `70000` |

### MySQL Workaround
To push `NULL`s last in ascending order:
```sql
SELECT *
FROM Employees
ORDER BY Salary IS NULL, Salary;
```
`Salary IS NULL` evaluates to:
- `0` for non-NULL
- `1` for NULL
So non-NULL values come first.

---

## 💾 External Sorting

If the data doesn't fit into memory:
1. Split data
2. Sort pieces
3. Merge pieces
*This is why sorting huge tables can become slow.*

---

## ⚡ With Index

Suppose an **Index on Salary** exists.
- The rows are already organized.
- Database can often read them directly.
- No explicit sorting required.

**Much faster:**
```sql
SELECT *
FROM Employees
ORDER BY Salary DESC
LIMIT 10;
```
*Database doesn't always need to sort every row. It can often stop after finding the top 10.*
