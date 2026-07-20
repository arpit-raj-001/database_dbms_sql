<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes - Practice Set 4</h1>
  <h2>Chapter 98</h2>
</div>

---

## 1. Sales in First Quarter

**Tables:** Product, Sales

Find products that were sold only in the first quarter of 2019:
- Between `2019-01-01` and `2019-03-31` (inclusive).
- They must not have any sales outside this range.
Return: `product_id`, `product_name`

### Solution 1 — EXISTS / NOT EXISTS
```sql
SELECT p.product_id, p.product_name
FROM Product p
WHERE EXISTS(
 SELECT 1 FROM Sales s
 WHERE p.product_id=s.product_id AND
 sale_date BETWEEN '2019-01-01' AND '2019-03-31'
)
AND NOT EXISTS(
 SELECT 1 FROM Sales s
 WHERE p.product_id=s.product_id AND
 sale_date NOT BETWEEN '2019-01-01' AND '2019-03-31'
);
```

### Solution 2 — GROUP BY + HAVING
```sql
SELECT
 p.product_id,
 p.product_name
FROM Product p
JOIN Sales s
ON p.product_id = s.product_id
GROUP BY p.product_id, p.product_name
HAVING
 MIN(s.sale_date) >= '2019-01-01'
 AND
 MAX(s.sale_date) <= '2019-03-31';
```

---

## 2. Valid Emails

Write a solution to find the users who have valid emails.
A valid e-mail has a prefix name and a domain where:
- The prefix name is a string that may contain letters, digits, underscore, period, and/or dash. The prefix name **must start with a letter**.
- The domain must be exactly `@leetcode.com` in lowercase.

### Solution
```sql
SELECT *
FROM Users
WHERE REGEXP_LIKE (mail,'^[A-Za-z][A-Za-z0-9_.-]*@leetcode\.com$','c');
```

---

## 3. Product Orders in Period

**Tables:** Products, Orders

Find products that satisfy: Ordered during February 2020 (`2020-02-01` to `2020-02-29`).
- Total units ordered in February ≥ 100.
Return: `product_name`, `unit`

### Solution
```sql
# Write your MySQL query statement below
SELECT p.product_name, SUM(o.unit) AS unit
FROM Products p
JOIN Orders o
ON p.product_id=o.product_id
WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY p.product_id
HAVING SUM(o.unit)>=100;
```

---

## 4. Stock Capital Gain/Loss

**Table:** Stocks
`operation` is an enum(`'Buy'`, `'Sell'`)

For each stock, calculate its capital gain or loss.
- Buy → money spent (subtract)
- Sell → money earned (add)
Return: `stock_name`, `capital_gain_loss`

### Solution
```sql
# Write your MySQL query statement below
SELECT stock_name, SUM(
 CASE WHEN operation='buy' THEN -price
 ELSE price
 END
) AS capital_gain_loss
FROM Stocks
GROUP BY stock_name;
```

---

## 5. Unique Subjects Taught

**Table:** Teacher

Write a solution to calculate the number of unique subjects each teacher teaches in the university.

### Solution
```sql
SELECT teacher_id, COUNT(
 DISTINCT subject_id
) AS cnt
FROM Teacher
GROUP BY teacher_id;
```

---

## 6. Rising Temperature

**Table:** Weather

Write a solution to find all dates' id with higher temperatures compared to its previous dates (yesterday).

### Solution
```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON w1.recordDate=DATE_ADD(w2.recordDate, INTERVAL 1 DAY)
WHERE w1.temperature>w2.temperature;
```

---

## 7. Daily Active Users

**Table:** Activity

Write a solution to find the daily active user count for a period of 30 days ending `2019-07-27` inclusively. A user was active on someday if they made at least one activity on that day.

### Solution
```sql
# Write your MySQL query statement below
SELECT activity_date AS day,
COUNT(
 DISTINCT(user_id)
) AS active_users
FROM Activity
WHERE activity_date BETWEEN DATE_SUB('2019-07-27', INTERVAL 29 DAY) AND 
'2019-07-27'
GROUP BY activity_date;
```

---

## 8. Latest Login

**Table:** Logins

Write a solution to report the latest login for all users in the year 2020. Do not include the users who did not login in 2020.

### Solution
```sql
# Write your MySQL query statement below
SELECT user_id,
MAX(
 time_stamp
) AS last_stamp
FROM Logins
WHERE YEAR(time_stamp)=2020
GROUP BY user_id;
```

---

## 9. Missing Information

**Tables:** Employees, Salaries

Write a solution to report the IDs of all the employees with missing information. The information of an employee is missing if:
- The employee's name is missing, or
- The employee's salary is missing.
Return the result table ordered by `employee_id` in ascending order.

### Solution
```sql
# Write your MySQL query statement below
SELECT employee_id
FROM Employees e
WHERE NOT EXISTS(
 SELECT 1 FROM Salaries s
 WHERE e.employee_id=s.employee_id
)
UNION
SELECT employee_id
FROM salaries s
WHERE NOT EXISTS(
 SELECT 1 FROM Employees e
 WHERE e.employee_id=s.employee_id
)
ORDER BY employee_id;
```

---

## 10. Followers Count

**Input:** Followers Table

| user_id | follower_id |
| :--- | :--- |
| `0` | `1` |
| `1` | `0` |
| `2` | `0` |
| `2` | `1` |

### Solution
```sql
SELECT user_id,
COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id;
```
