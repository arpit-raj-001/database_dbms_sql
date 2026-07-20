<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes - Practice Set 5</h1>
  <h2>Chapter 99</h2>
</div>

---

## 1. Game Play Analysis

**Table:** Activity

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to determine the number of players who logged in on the day immediately following their initial login, and divide it by the total number of players.

### Solution
```sql
SELECT ROUND(COUNT(a.player_id)/(SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity a
JOIN(
 SELECT player_id, MIN(event_date) AS firstlogin
 FROM Activity
 GROUP BY player_id
) b
ON a.player_id=b.player_id
WHERE a.event_date=DATE_ADD(b.firstlogin, INTERVAL 1 DAY);
```
**Step 1:** Find each player's first login and make a table out of it. This will be used to join the main table with this table and shortlist only valid players, and in step 2 just find what you need.

---

## 2. Managers with at Least Five Direct Reports

**Table:** Employee

Write a solution to find managers with at least five direct reports.

### Solution 1
```sql
SELECT e.name
FROM Employee e
JOIN(
 SELECT managerId
 FROM Employee
 GROUP BY managerId
 HAVING COUNT(*)>=5
) m
ON e.id=m.managerId;
```

### Solution 2
```sql
SELECT name
FROM Employee e1
WHERE(
 SELECT COUNT(*)
 FROM Employee e2
 WHERE e1.id=e2.managerId
) >= 5;
```
*subquery method is inefficient but works*

---

## 3. Customers Who Bought All Products

**Tables:** Customer, Product

Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.

### Solution
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key)=(
 SELECT COUNT(*)
 FROM Product
);
```

---

## 4. Market Analysis I

**Tables:** Users, Orders, Items

Write a solution to find for each user, the join date and the number of orders they made as a buyer in 2019.

### Solution
```sql
# Write your MySQL query statement below
SELECT u.user_id AS buyer_id, u.join_date,
(
 SELECT COUNT(*)
 FROM Orders o
 WHERE u.user_id=o.buyer_id
 AND YEAR(o.order_date)=2019
) AS orders_in_2019
FROM Users u;
```

---

## 5. Monthly Transactions I

**Table:** Transactions

Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

### Solution
```sql
# Write your MySQL query statement below
SELECT DATE_FORMAT(trans_date,'%Y-%m') AS month,
 country,
 COUNT(*) AS trans_count,
 SUM(CASE 
  WHEN state='approved' THEN 1 ELSE 0
 END) AS approved_count,
 SUM(amount) AS trans_total_amount,
 SUM(CASE
  WHEN state='approved' THEN amount ELSE 0
 END) AS approved_total_amount
FROM Transactions
GROUP BY month, country;
```

---

## 6. Last Person to Fit in the Bus

**Table:** Queue

`turn` determines the order in which the people will board the bus. `weight` is the weight of the person in kilograms.
There is a queue of people waiting to board a bus. However, the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.
Write a solution to find the `person_name` of the last person that can fit on the bus without exceeding the weight limit.

### Solution 1
```sql
SELECT person_name
FROM Queue q1
WHERE(
 SELECT SUM(weight)
 FROM Queue q2
 WHERE q2.turn<=q1.turn 
) <= 1000
ORDER BY turn DESC
LIMIT 1;
```

### Solution 2
```sql
# Write your MySQL query statement below
SELECT q1.person_name
FROM Queue q1
JOIN Queue q2
ON q2.turn<=q1.turn
GROUP BY q1.person_id, q1.person_name, q1.turn
HAVING SUM(q2.weight)<=1000
ORDER BY q1.turn DESC
LIMIT 1;
```
