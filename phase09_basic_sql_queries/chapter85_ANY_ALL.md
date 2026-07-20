<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🎭 ANY & ALL Operators</h1>
  <h2>Chapter 85</h2>
</div>

---

## ❓ What is ANY?

`ANY` compares a value against a set of values. The condition becomes `TRUE` if the comparison is true for **at least one** value in the set.

> [!TIP]
> Think of it as:
> *"Does this comparison succeed for at least one element?"*

```sql
SELECT *
FROM Employees
WHERE Salary > ANY
(SELECT Salary
FROM Managers
);
```

**Another syntax:**
```sql
WHERE ID = ANY(10, 20, 30)
```
*(Equivalent to `WHERE ID IN (10,20,30)`)*

`= ANY` is rarely written in practice.

### SHORTCUT for ANY
```text
x < ANY(set)
     ↓
x < MAX(set)
```

**Optimization:**
The optimizer often rewrites `=ANY` as `IN`.
For comparisons like `>ANY`, MySQL may internally optimize using `MIN()`.

---

## 🌐 What is ALL?

`ALL` is the opposite of `ANY`. A comparison must be `TRUE` for **every** value in the set.

> [!TIP]
> Think of it as:
> *"Must satisfy all comparisons."*

### Syntax
```sql
SELECT *
FROM Employees
WHERE Salary > ALL
(SELECT Salary
FROM Managers
);
```

### SHORTCUT for ALL
```text
x > ALL(set)
     ↓
x > MAX(set)

x < ALL(set)
     ↓
x < MIN(set)
```

Instead of:
```sql
WHERE Price < ALL
(SELECT Price
FROM Products
)
```
people write:
```sql
WHERE Price =
(SELECT MIN(Price)
FROM Products
)
```
