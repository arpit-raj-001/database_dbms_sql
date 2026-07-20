<div align="center">
  <small><i>Authored by: Arpit Raj, Lnmiit jaipur</i></small>
  <h1>🔗 GROUP BY with JOIN?</h1>
  <h2>Chapter 108</h2>
</div>

---

Consider two tables.

**Customers**
| customer_id | customer_name |
| :--- | :--- |
| 1 | Arpit |
| 2 | aadz |
| 3 | Rahul |

**Orders**
| order_id | customer_id | amount |
| :--- | :--- | :--- |
| 101 | 1 | 1200 |
| 102 | 1 | 800 |
| 103 | 2 | 1500 |
| 104 | 1 | 600 |
| 105 | 3 | 900 |

**Question**
How many orders has each customer placed?
Customer names are in one table.
Orders are in another.
Need a `JOIN` first.

## 14.11.2 Basic Pattern

```sql
SELECT group_column,
 AGG(...)
FROM TableA
JOIN TableB
ON ...
GROUP BY group_column;
```

**Remember:**
JOIN first, GROUP later. 

### Example — Orders per Customer
```sql
SELECT c.customer_name,
 COUNT(*) AS total_orders
FROM Customers c
JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
```

### Total Spending per Customer
```sql
SELECT c.customer_name,
 SUM(o.amount) AS total_spent
FROM Customers c
JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_name;
```

---

## Movies per Director

**Directors**
| id | director |
| :--- | :--- |
| 1 | Christopher Nolan |
| 2 | Rajkumar Hirani |

**Movies**
| movie | director_id |
| :--- | :--- |
| Interstellar | 1 |
| Inception | 1 |
| 3 Idiots | 2 |

**Query**
```sql
SELECT d.director,
 COUNT(*) AS movies
FROM Directors d
JOIN Movies m
ON d.id=m.director_id
GROUP BY d.director;
```

---

## LEFT JOIN + GROUP BY

*Interview favorite.*

**Question**
Show all customers, including those who never ordered.

```sql
SELECT c.customer_name,
 COUNT(o.order_id) AS orders
FROM Customers c
LEFT JOIN Orders o
ON c.customer_id=o.customer_id
GROUP BY c.customer_name;
```
*`COUNT(o.order_id)` ignores NULL values.*

---

## Conditional Aggregations

Suppose we have:

**Students**
| Student | Gender | Result |
| :--- | :--- | :--- |
| Arpit | Male | Pass |
| aadz | Female | Pass |
| Rahul | Male | Fail |
| Priya | Female | Pass |
| Riya | Female | Fail |

**Question**
Find
- Male students
- Female students

```sql
SELECT
SUM(CASE
 WHEN gender='Male'
 THEN 1
 ELSE 0
 END) AS male_count,
SUM(CASE
 WHEN gender='Female'
 THEN 1
 ELSE 0
 END) AS female_count
FROM Students;
```

### Conditional AVG
Average salary of ECE employees.

```sql
SELECT
AVG(CASE
 WHEN department='ECE'
 THEN salary
 END
) AS avg_ece_salary
FROM Employee;
```

---

## Conditional Aggregation Patterns

⭐⭐⭐⭐⭐ Master these patterns—they solve dozens of interview questions.
Think of this as a toolbox.

### Pattern 1 — Count Matching Rows
```sql
SUM(CASE
 WHEN condition
 THEN 1
 ELSE 0
END)
```
**Example:** Count delivered orders.
```sql
SELECT
SUM(CASE
 WHEN status='Delivered'
 THEN 1
 ELSE 0
END)
FROM Orders;
```

### Pattern 2 — Sum Matching Values
```sql
SUM(CASE
 WHEN condition
 THEN amount
 ELSE 0
END)
```
**Example:** Delivered revenue.

### Pattern 3 — Average Matching Values
```sql
AVG(CASE
 WHEN condition
 THEN salary
END)
```
**Example:** Average salary of ECE employees.

### Pattern 4 — Multiple Counts in One Query
```sql
SELECT
SUM(CASE WHEN gender='Male' THEN 1 ELSE 0 END) male,
SUM(CASE WHEN gender='Female' THEN 1 ELSE 0 END) female
FROM Students;
```

### Pattern 5 — Percentage
*Interview favorite.*
Percentage of delivered orders.
```sql
SELECT
100.0 *
SUM(CASE
 WHEN status='Delivered'
 THEN 1
 ELSE 0
END)
/COUNT(*) AS delivery_percentage
FROM Orders;
```

### Pattern 6 — Ratio
Male-to-female ratio.
```sql
SELECT
SUM(CASE WHEN gender='Male' THEN 1 ELSE 0 END) /
SUM(CASE WHEN gender='Female' THEN 1 ELSE 0 END) AS ratio
FROM Students;
```
*(Guard against division by zero in production code.)*

### Pattern 7 — Pivot Simulation
Convert rows into columns.

**Orders**
| Status |
| :--- |
| Delivered |
| Pending |
| Cancelled |

**Query**
```sql
SELECT
SUM(CASE WHEN status='Delivered' THEN 1 ELSE 0 END) AS delivered,
SUM(CASE WHEN status='Pending' THEN 1 ELSE 0 END) AS pending,
SUM(CASE WHEN status='Cancelled' THEN 1 ELSE 0 END) AS cancelled
FROM Orders;
```

**Output**
| Delivered | Pending | Cancelled |
| :--- | :--- | :--- |
| 15 | 6 | 2 |
