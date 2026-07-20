<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Practice Notes (Set 9)</h1>
  <h2>Chapter 103</h2>
</div>

---

## Problem 1: Unique ID

**Table:** Employees
**Table:** EmployeeUNI

`(id, unique_id)` is the primary key for the EmployeeUNI table.
Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

### Query:
```sql
# Write your MySQL query statement below
SELECT
 eu.unique_id,
 e.name
FROM Employees e
LEFT JOIN EmployeeUNI eu
ON e.id = eu.id;
```

---

## Problem 2: Sales Report

**Table:** Sales
**Table:** Product

`(sale_id, year)` is the primary key of Sales table. `product_id` is a foreign key.
Write a solution to report the `product_name`, `year`, and `price` for each `sale_id` in the Sales table.

### Query:
```sql
SELECT
 p.product_name,
 s.year,
 s.price
FROM Sales s
JOIN Product p
ON s.product_id = p.product_id;
```

---

## Problem 3: Customer Visits without Transactions

**Table:** Visits
**Table:** Transactions

Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

### Query:
```sql
SELECT
 v.customer_id,
 COUNT(*) AS count_no_trans
FROM Visits v
WHERE NOT EXISTS
(
 SELECT 1
 FROM Transactions t
 WHERE t.visit_id = v.visit_id
)
GROUP BY v.customer_id;
```

---

## Problem 4: Average Processing Time

**Table:** Activity

`(machine_id, process_id, activity_type)` is the primary key.
`activity_type` is an ENUM of type ('start', 'end').
Write a solution to find the average time each machine takes to complete a process.

### Query:
```sql
SELECT
 a.machine_id,
 ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a
JOIN Activity b
ON a.machine_id = b.machine_id
AND a.process_id = b.process_id
WHERE a.activity_type = 'start'
AND b.activity_type = 'end'
GROUP BY a.machine_id;
```

---

## Problem 5: Employee Bonus

**Table:** Employee
**Table:** Bonus

Write a solution to report the name and bonus amount of each employee who satisfies either:
- The employee has a bonus less than 1000.
- The employee did not get any bonus.

### Query:
```sql
SELECT
 e.name,
 b.bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE b.bonus < 1000
OR b.bonus IS NULL;
```

---

## Problem 6: Student Examinations

**Tables:** Students, Subjects, Examinations

Write a solution to find the number of times each student attended each exam.

### Query:
```sql
SELECT
 s.student_id,
 s.student_name,
 sub.subject_name,
 COUNT(e.subject_name) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e
ON s.student_id = e.student_id
AND sub.subject_name = e.subject_name
GROUP BY
 s.student_id,
 s.student_name,
 sub.subject_name
ORDER BY
 s.student_id,
 sub.subject_name;
```

---

## Problem 7: Immediate Food Delivery

**Table:** Delivery

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

### Query:
```sql
SELECT
 ROUND(
 AVG(order_date = customer_pref_delivery_date) * 100,
 2
 ) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN
(
 SELECT
 customer_id,
 MIN(order_date)
 FROM Delivery
 GROUP BY customer_id
);
```

---

## Problem 8: Managers and Reports

**Table:** Employees

Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.

### Query:
```sql
SELECT
 m.employee_id,
 m.name,
 COUNT(e.employee_id) AS reports_count,
 ROUND(AVG(e.age)) AS average_age
FROM Employees m
JOIN Employees e
ON m.employee_id = e.reports_to
GROUP BY
 m.employee_id,
 m.name
ORDER BY
 m.employee_id;
```

---

## Problem 9: Primary Department

**Table:** Employee

Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.

### Query:
```sql
SELECT
 e.employee_id,
 e.department_id
FROM Employee e
JOIN
(
 SELECT
 employee_id,
 COUNT(*) AS cnt
 FROM Employee
 GROUP BY employee_id
) t
ON e.employee_id = t.employee_id
WHERE t.cnt = 1
OR e.primary_flag = 'Y';
```

---

## Problem 10: Consecutive Numbers

**Table:** Logs

Find all numbers that appear at least three times consecutively.

### Query:
```sql
SELECT DISTINCT
 l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2
ON l2.id = l1.id + 1
JOIN Logs l3
ON l3.id = l1.id + 2
WHERE l1.num = l2.num
AND l2.num = l3.num;
```

---

## Problem 11: Product Price at a Given Date

**Table:** Products

Write a solution to find the prices of all products on the date 2019-08-16. (Initially, all products have price 10).

### Query:
```sql
SELECT
 p.product_id,
 p.new_price AS price
FROM Products p
JOIN
(
 SELECT
 product_id,
 MAX(change_date) AS latest_date
 FROM Products
 WHERE change_date <= '2019-08-16'
 GROUP BY product_id
) t
ON p.product_id = t.product_id
AND p.change_date = t.latest_date
UNION
SELECT
 product_id,
 10 AS price
FROM Products
GROUP BY product_id
HAVING MIN(change_date) > '2019-08-16';
```

---

## Problem 12: Employees Whose Manager Left

**Table:** Employees

Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company.

### Query:
```sql
SELECT
 employee_id
FROM Employees
WHERE salary < 30000
AND manager_id IS NOT NULL
AND manager_id NOT IN
(
 SELECT employee_id
 FROM Employees
)
ORDER BY employee_id;
```

---

## Problem 13: High Earners in Department

**Tables:** Employee, Department

Write a solution to find the employees who are high earners (top 3 unique salaries) in each of the departments.

### Query:
```sql
SELECT
 d.name AS Department,
 e.name AS Employee,
 e.salary AS Salary
FROM Employee e
JOIN Department d
ON e.departmentId = d.id
WHERE
(
 SELECT COUNT(DISTINCT salary)
 FROM Employee
 WHERE departmentId = e.departmentId
 AND salary > e.salary
) < 3;
```

---

## Problem 14: Delete Duplicate Emails

**Table:** Person

Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

### Query:
```sql
DELETE p1
FROM Person p1
JOIN Person p2
ON p1.email = p2.email
AND p1.id > p2.id;
```

---

## Problem 15: Products Sold Per Date

**Table:** Activities

Write a solution to find for each date the number of different products sold and their names (sorted lexicographically).

### Query:
```sql
SELECT
 sell_date,
 COUNT(DISTINCT product) AS num_sold,
 GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
