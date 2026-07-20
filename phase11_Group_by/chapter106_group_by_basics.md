<div align="center">
  <small><i>Authored by: Arpit Raj, Lnmiit jaipur</i></small>
  <h1>📊 Why GROUP BY Exists</h1>
  <h2>Chapter 106</h2>
</div>

---

Suppose the table is:

| id | department | salary |
| :--- | :--- | :--- |
| 1 | IT | 50000 |
| 2 | IT | 60000 |
| 3 | HR | 40000 |
| 4 | HR | 45000 |
| 5 | Sales | 55000 |

**Question:**
Average salary of each department

Without `GROUP BY`, SQL doesn't know where one department ends and another begins.

`GROUP BY` tells SQL:
- Put all rows having the same department together.

Groups formed:
- **IT:** 50000, 60000
- **HR:** 40000, 45000
- **Sales:** 55000

Now SQL calculates averages separately.

```sql
SELECT department,
 AVG(salary)
FROM Employee
GROUP BY department;
```

### Output

| department | AVG(salary) |
| :--- | :--- |
| IT | 55000 |
| HR | 42500 |
| Sales | 55000 |

---

## Row-Level

Every input row produces one output row.

**Example**
```sql
SELECT name,
 salary * 1.1
FROM Employee;
```

## Group-Level

Many rows become one row.

**Example**
```sql
SELECT department,
 AVG(salary)
FROM Employee
GROUP BY department;
```

---

## SQL executes as

1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `Aggregate Functions`
5. `SELECT`
6. `ORDER BY`
7. `LIMIT`

---

## COUNT() with GROUP BY

`COUNT()` becomes much more useful when combined with `GROUP BY`.

### 14.2.1 COUNT(*)
Counts all rows in each group.

**Example**

| department |
| :--- |
| IT |
| IT |
| HR |
| HR |
| Sales |

```sql
SELECT department,
 COUNT(*)
FROM Employee
GROUP BY department;
```

**Output**

| department | COUNT(*) |
| :--- | :--- |
| IT | 2 |
| HR | 2 |
| Sales | 1 |

### 14.2.2 COUNT(column)
Counts only non-NULL values.

**Example**

| department | phone |
| :--- | :--- |
| IT | 111 |
| IT | NULL |
| IT | 222 |

```sql
SELECT department,
 COUNT(phone)
FROM Employee
GROUP BY department;
```

**Output**

| department | COUNT(phone) |
| :--- | :--- |
| IT | 2 |

*The NULL phone is ignored.*

### 14.2.3 COUNT(DISTINCT column)
Counts unique values inside each group.

**Example**

| department | city |
| :--- | :--- |
| IT | Delhi |
| IT | Delhi |
| IT | Mumbai |
| IT | Delhi |

```sql
SELECT department,
 COUNT(DISTINCT city)
FROM Employee
GROUP BY department;
```

**Output**

| department | COUNT(DISTINCT city) |
| :--- | :--- |
| IT | 2 |

*Only Delhi and Mumbai.*

---

## 14.2.4 NULL Handling

Suppose

| department | bonus |
| :--- | :--- |
| IT | 1000 |
| IT | NULL |
| IT | 2000 |

| Function | Result |
| :--- | :--- |
| `COUNT(*)` | 3 |
| `COUNT(bonus)` | 2 |

---

## GROUP BY Multiple Columns

Sometimes one column isn't enough.

**Example**

| department | city | salary |
| :--- | :--- | :--- |
| IT | Delhi | 50000 |
| IT | Delhi | 60000 |
| IT | Mumbai | 70000 |
| HR | Delhi | 40000 |
| HR | Mumbai | 45000 |

### Group by Department
```sql
GROUP BY department
```
Creates:
- IT
- HR

### Group by Department and City
```sql
GROUP BY department, city
```
Creates:
- IT + Delhi
- IT + Mumbai
- HR + Delhi
- HR + Mumbai

Each unique combination becomes a separate group.

**Example**
```sql
SELECT department,
 city,
 COUNT(*) AS employees
FROM Employee
GROUP BY department, city;
```

---

## Group by Year and Month

```sql
SELECT YEAR(order_date),
 MONTH(order_date),
 COUNT(*)
FROM Orders
GROUP BY YEAR(order_date),
 MONTH(order_date);
```
*Useful for monthly reports.*

---

## Question

Count employees in salary ranges.
- Low
- Medium
- High

**Query**
```sql
SELECT
 CASE
 WHEN salary < 40000 THEN 'Low'
 WHEN salary < 70000 THEN 'Medium'
 ELSE 'High'
 END AS salary_range,
 COUNT(*)
FROM Employee
GROUP BY
 CASE
 WHEN salary < 40000 THEN 'Low'
 WHEN salary < 70000 THEN 'Medium'
 ELSE 'High'
 END;
```

---

## GROUP BY CONCAT()

Suppose

| first_name | last_name |
| :--- | :--- |
| John | Smith |
| John | Smith |
| Alice | Brown |

```sql
SELECT
 CONCAT(first_name,' ',last_name) AS full_name,
 COUNT(*)
FROM Employee
GROUP BY CONCAT(first_name,' ',last_name);
```

**Output**

| full_name | count |
| :--- | :--- |
| John Smith | 2 |
| Alice Brown | 1 |

---

## GROUP BY EXTRACT()

PostgreSQL

```sql
SELECT
 EXTRACT(YEAR FROM order_date),
 COUNT(*)
FROM Orders
GROUP BY EXTRACT(YEAR FROM order_date);
```
