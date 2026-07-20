<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Notes & Problem Solutions (Practice Set 7)</h1>
  <h2>Chapter 101</h2>
</div>

---

## 1. Restaurant Expansion (Moving Average)

**Table: Customer**
| Column Name | Type |
| :--- | :--- |
| `customer_id` | int |
| `name` | varchar |
| `visited_on` | date |
| `amount` | int |

`(customer_id, visited_on)` is the primary key for this table.
This table contains data about customer transactions in a restaurant.
`visited_on` is the date on which the customer with ID (`customer_id`) has visited the restaurant. `amount` is the total paid by a customer.

You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).
Compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). `average_amount` should be rounded to two decimal places.
Return the result table ordered by `visited_on` in ascending order.

### Solution

```sql
SELECT
 c1.visited_on,
 SUM(c2.amount) AS amount,
 ROUND(SUM(c2.amount) / 7, 2) AS average_amount
FROM
(
 SELECT
 visited_on,
 SUM(amount) AS amount
 FROM Customer
 GROUP BY visited_on
) c1
JOIN
(
 SELECT
 visited_on,
 SUM(amount) AS amount
 FROM Customer
 GROUP BY visited_on
) c2
ON c2.visited_on BETWEEN DATE_SUB(c1.visited_on, INTERVAL 6 DAY)
 AND c1.visited_on
GROUP BY c1.visited_on
HAVING COUNT(*) = 7
ORDER BY c1.visited_on;
```

**Why `HAVING COUNT(*) = 7`?**
After aggregating, each row represents one day.
So for a given `visited_on`:
If there are exactly 7 rows in the joined range, then all 7 consecutive days exist.
Otherwise, it's not a complete 7-day window.

---

## 2. Daily Sales (Unique Leads & Partners)

**Table: DailySales**
| Column Name | Type |
| :--- | :--- |
| `date_id` | date |
| `make_name` | varchar |
| `lead_id` | int |
| `partner_id` | int |

There is no primary key for this table. It may contain duplicates.
This table contains the date and the name of the product sold and the IDs of the lead and partner it was sold to. The name consists of only lowercase English letters.

For each `date_id` and `make_name`, find the number of distinct `lead_id`'s and distinct `partner_id`'s.
Return the result table in any order.

### Solution
```sql
SELECT date_id,
make_name,
COUNT(DISTINCT lead_id) AS unique_leads,
COUNT(DISTINCT partner_id) AS unique_partners
FROM DailySales
GROUP BY date_id, make_name;
```

---

## 3. Employees Total Time

**Table: Employees**
| Column Name | Type |
| :--- | :--- |
| `emp_id` | int |
| `event_day` | date |
| `in_time` | int |
| `out_time` | int |

`(emp_id, event_day, in_time)` is the primary key (combinations of columns with unique values) of this table.
The table shows the employees' entries and exits in an office.
`event_day` is the day at which this event happened, `in_time` is the minute at which the employee entered the office, and `out_time` is the minute at which they left the office. `in_time` and `out_time` are between 1 and 1440.
It is guaranteed that no two events on the same day intersect in time, and `in_time` < `out_time`.

Write a solution to calculate the total time in minutes spent by each employee on each day at the office. Note that within one day, an employee can enter and leave more than once. The time spent in the office for a single entry is `out_time - in_time`.
Return the result table in any order.

### Solution
```sql
SELECT event_day as day,
emp_id,
SUM(out_time-in_time) AS total_time
FROM Employees
GROUP BY emp_id, event_day;
```

---

## 4. Ad-Free Sessions

**Table: Playback**
| Column | Type |
| :--- | :--- |
| `session_id` | int |
| `customer_id` | int |
| `start_time` | datetime |
| `end_time` | datetime |

**Table: Ads**
| Column | Type |
| :--- | :--- |
| `ad_id` | int |
| `customer_id` | int |
| `timestamp` | datetime |

A session is ad-free if no ad was shown during that session.
Return the IDs of all ad-free sessions.

### Solution
```sql
SELECT session_id
FROM Playback p
WHERE NOT EXISTS (
 SELECT 1
 FROM Ads a
 WHERE a.customer_id = p.customer_id
 AND a.timestamp BETWEEN p.start_time AND p.end_time
);
```

---

## 5. Matches Statistics

**Table: Teams**
| Column | Type |
| :--- | :--- |
| `team_id` | int |
| `team_name` | varchar |

**Table: Matches**
| Column | Type |
| :--- | :--- |
| `home_team_id` | int |
| `away_team_id` | int |
| `home_team_goals` | int |
| `away_team_goals` | int |

Return for every team:
`team_name`, `matches_played`, `points`, `goal_for`, `goal_against`, `goal_diff`
Order by: `points DESC`, `goal_diff DESC`, `team_name ASC`

### Solution

```sql
SELECT
 t.team_name,
 COUNT(*) AS matches_played,
 SUM(points) AS points,
 SUM(goal_for) AS goal_for,
 SUM(goal_against) AS goal_against,
 SUM(goal_for - goal_against) AS goal_diff
FROM Teams t
JOIN (
 SELECT
 home_team_id AS team_id,
 CASE
 WHEN home_team_goals > away_team_goals THEN 3
 WHEN home_team_goals = away_team_goals THEN 1
 ELSE 0
 END AS points,
 home_team_goals AS goal_for,
 away_team_goals AS goal_against
 FROM Matches
 UNION ALL
 SELECT
 away_team_id,
 CASE
 WHEN away_team_goals > home_team_goals THEN 3
 WHEN away_team_goals = home_team_goals THEN 1
 ELSE 0
 END,
 away_team_goals,
 home_team_goals
 FROM Matches
) m
ON t.team_id = m.team_id
GROUP BY t.team_id, t.team_name
ORDER BY
 points DESC,
 goal_diff DESC,
 team_name;
```

**Notice something:**
Each row contains statistics for two teams. But in the final answer, we want statistics per team. So we first split every match into two records using `UNION ALL`.

---

## 6. Confirmation Rate

**Table: Signups**
| Column Name | Type |
| :--- | :--- |
| `user_id` | int |
| `time_stamp` | datetime |

**Table: Confirmations**
| Column Name | Type |
| :--- | :--- |
| `user_id` | int |
| `time_stamp` | datetime |
| `action` | ENUM |

`(user_id, time_stamp)` is the primary key. `user_id` is a foreign key to Signups table. `action` is ENUM (`'confirmed'`, `'timeout'`).

The confirmation rate of a user is the number of `'confirmed'` messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.
Write a solution to find the confirmation rate of each user. Return the result table in any order.

### Solution

```sql
SELECT 
s.user_id,
ROUND(
 CASE WHEN COUNT(c.action)=0 THEN 0
 ELSE 
 SUM(
 CASE WHEN c.action='timeout' THEN 0
 ELSE 1
 END
 )/COUNT(c.action)
 END,
 2
)
AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
ON s.user_id=c.user_id
GROUP BY s.user_id;
```

---

## 7. Understanding SQL Joins

Match on `left.customer_id = right.customer_id`

### INNER JOIN · Result
Keep only matching rows.

### LEFT JOIN · Result
Keep all rows from left table, unmatched right columns become `NULL`.

### RIGHT JOIN · Result
Keep all rows from right table, unmatched left columns become `NULL`.

### FULL OUTER JOIN · Result
Keep every row; missing customers or levels become `NULL`.

---

## 8. Drop Type 1 Orders for Customers With Type 0 Orders

**Table: Orders**
| Column | Type |
| :--- | :--- |
| `order_id` | int |
| `customer_id` | int |
| `order_type` | int |

`order_type = 0` → Type 0 order
`order_type = 1` → Type 1 order

**Rule:**
If a customer has at least one Type 0 order, then remove all their Type 1 orders. Otherwise, keep all their orders.

### Solution

```sql
SELECT *
FROM Orders o1
WHERE order_type = 0
OR NOT EXISTS (
 SELECT 1
 FROM Orders o2
 WHERE o2.customer_id = o1.customer_id
 AND o2.order_type = 0
);
```

**Explanation:**
Keep the row if:
- It is already a Type 0 order, or
- The customer does not have any Type 0 order.

### Detailed Step-by-Step Explanation
Using `LEFT JOIN` approach:

```sql
SELECT o1.*
FROM Orders o1
LEFT JOIN Orders o2
ON o1.customer_id = o2.customer_id
AND o2.order_type = 0
WHERE o1.order_type = 0
OR o2.customer_id IS NULL;
```
1. `LEFT JOIN` the same table on customer id where the right table has order type 0.
2. If `o2.customer_id IS NULL`, it means this customer has NO Type 0 order. So we keep these.
3. If `o1.order_type = 0`, we also keep these.
