<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 Chapter: Advanced SQL Queries and Techniques (Set 8)</h1>
  <h2>Chapter 102</h2>
</div>

---

## Task Submissions by Day

**Table: Tasks**
| Column | Type |
| :--- | :--- |
| `task_id` | int |
| `assignee_id` | int |
| `submit_date` | date |

**Return:**
- `weekend_cnt` → tasks submitted on Saturday or Sunday
- `working_cnt` → tasks submitted on Monday–Friday

### Approach 1 — CASE (Best)

```sql
SELECT
 SUM(
 CASE
 WHEN DAYOFWEEK(submit_date) IN (1,7) THEN 1
 ELSE 0
 END
 ) AS weekend_cnt,
 SUM(
 CASE
 WHEN DAYOFWEEK(submit_date) NOT IN (1,7) THEN 1
 ELSE 0
 END
 ) AS working_cnt
FROM Tasks;
```

**Why DAYOFWEEK()?**
MySQL returns:
| Day | Value |
| :--- | :--- |
| Sunday | 1 |
| Monday | 2 |
| Tuesday | 3 |
| Wednesday | 4 |
| Thursday | 5 |
| Friday | 6 |
| Saturday | 7 |

So `DAYOFWEEK(submit_date) IN (1,7)` means Sunday OR Saturday.

---

## Consecutive Year Orders

**Table: Orders**
| Column | Type |
| :--- | :--- |
| `order_id` | int |
| `product_id` | int |
| `quantity` | int |
| `purchase_date` | date |

Return the `product_id` of products that satisfy:
- At least 3 orders in one year.
- At least 3 orders in the next consecutive year.

### Self Join (Best)
First count the orders per product per year.

```sql
SELECT DISTINCT o1.product_id
FROM
(
 SELECT
 product_id,
 YEAR(purchase_date) AS year,
 COUNT(*) AS orders
 FROM Orders
 GROUP BY product_id, YEAR(purchase_date)
) o1
JOIN
(
 SELECT
 product_id,
 YEAR(purchase_date) AS year,
 COUNT(*) AS orders
 FROM Orders
 GROUP BY product_id, YEAR(purchase_date)
) o2
ON o1.product_id = o2.product_id
AND o2.year = o1.year + 1
WHERE o1.orders >= 3
AND o2.orders >= 3;
```

---

## Form a Chemical Bond

**Tables**

**Elements**
| Column | Type |
| :--- | :--- |
| `symbol` | varchar |
| `type` | enum('Metal','Nonmetal') |

Return all possible compounds by pairing every Metal with every Nonmetal.

### Approach 1 — CROSS JOIN (Best)

```sql
SELECT
 e1.symbol AS metal,
 e2.symbol AS nonmetal
FROM Elements e1
CROSS JOIN Elements e2
WHERE e1.type = 'Metal'
AND e2.type = 'Nonmetal';
```

**Explanation:**
1. `FROM Elements e1 CROSS JOIN Elements e2`
Every row combines with every other row. This is called the Cartesian Product.
2. `WHERE e1.type='Metal' AND e2.type='Nonmetal'`
Keep only valid Metal-Nonmetal pairings.

### Approach 2 — JOIN
CROSS JOIN is equivalent to a JOIN with a condition that is always true.

```sql
SELECT
 e1.symbol AS metal,
 e2.symbol AS nonmetal
FROM Elements e1
JOIN Elements e2
ON 1 = 1
WHERE e1.type = 'Metal'
AND e2.type = 'Nonmetal';
```

---

## Latest Employee Salary

**Table: Salary**
| Column | Type |
| :--- | :--- |
| `emp_id` | int |
| `firstname` | varchar |
| `lastname` | varchar |
| `salary` | int |
| `department_id` | int |
| `salary_date` | date |

Each employee may have multiple salary records. Return the latest salary of every employee.

### Approach 1 — JOIN + MAX() (Best)

```sql
SELECT s.*
FROM Salary s
JOIN
(
 SELECT
 emp_id,
 MAX(salary_date) AS latest_date
 FROM Salary
 GROUP BY emp_id
) t
ON s.emp_id = t.emp_id
AND s.salary_date = t.latest_date;
```

### Approach 2 — Correlated Subquery

```sql
SELECT *
FROM Salary s
WHERE salary_date =
(
 SELECT MAX(salary_date)
 FROM Salary
 WHERE emp_id = s.emp_id
);
```

---

## Popularity Percentage

Suppose the table is:
| user1 | user2 |
| :--- | :--- |
| `1` | `2` |
| `1` | `3` |
| `2` | `3` |

Each row means `user1 ↔ user2`. Friendship is bidirectional.
So `1 2` means `1 is friend of 2` and `2 is friend of 1`.

**Now compute Popularity Percentage:**
`Popularity Percentage = (Number of Friends) / (Total Users - 1) * 100`

### Solution

```sql
SELECT
 user_id, 
 ROUND(
 COUNT(*) * 100 / 
 (
 SELECT COUNT(DISTINCT user_id) 
 FROM
 (
 SELECT user1 AS user_id FROM Friends
 UNION 
 SELECT user2 FROM Friends
 ) t
 ) - 1 
 ),
 2 
 ) AS percentage_popularity
FROM
(
 SELECT user1 AS user_id FROM Friends
 UNION ALL 
 SELECT user2 FROM Friends
) t
GROUP BY user_id;
```

**Why UNION ALL?**
Notice: Every friendship contains two users. We want to count friendships for each user. So we split every friendship into two rows.
If we use `UNION`, duplicates disappear. Then a friend count becomes wrong. Hence `UNION ALL`.
But for the Total Users calculation, we ONLY want unique users, so we use `UNION` instead of `UNION ALL`.
