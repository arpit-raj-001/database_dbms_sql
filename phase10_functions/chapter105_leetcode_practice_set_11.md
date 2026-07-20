<div align="center">
  <small><i>Authored by : Arpit Raj , Lnmiit jaipur</i></small>
  <h1>📝 SQL Practice Notes (Set 11)</h1>
  <h2>Chapter 105</h2>
</div>

---

## Problem 1: Unique ID

**Table: Employees**
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
```
`id` is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.

**Table: EmployeeUNI**
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
```
`(id, unique_id)` is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.
Return the result table in any order.
The result format is in the following example.

**Example 1:**
**Input:**
Employees table:
```text
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
```
EmployeeUNI table:
```text
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
```
**Output:**
```text
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
```

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

**Table: Sales**
```text
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
```
`(sale_id, year)` is the primary key (combination of columns with unique values) of this table.
`product_id` is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product `product_id` in a certain year.
Note that the price is per unit.

**Table: Product**
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
```
`product_id` is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.

Write a solution to report the `product_name`, `year`, and `price` for each `sale_id` in the Sales table.
Return the resulting table in any order.

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

**Table: Visits**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
```
`visit_id` is the column with unique values for this table.
This table contains information about the customers who visited the mall.

**Table: Transactions**
```text
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
```
`transaction_id` is column with unique values for this table.
This table contains information about the transactions made during the `visit_id`.

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

**Table: Activity**
```text
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
```
The table shows the user activities for a factory website.
`(machine_id, process_id, activity_type)` is the primary key (combination of columns with unique values) of this table.
`machine_id` is the ID of a machine.
`process_id` is the ID of a process running on the machine with ID `machine_id`.
`activity_type` is an ENUM (category) of type ('start', 'end').
`timestamp` is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The `start` timestamp will always be less than or equal to the `end` timestamp for every `(machine_id, process_id)` pair.
It is guaranteed that each `(machine_id, process_id)` pair has a 'start' and 'end' timestamp.

There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.
The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.
The resulting table should have the `machine_id` along with the average time as `processing_time`, which should be rounded to 3 decimal places.
Return the result table in any order.

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

**Table: Employee**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
```
`empId` is the column with unique values for this table.
Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.

**Table: Bonus**
```text
+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
```
`empId` is the column of unique values for this table.
`empId` is a foreign key (reference column) to `empId` from the Employee table.
Each row of this table contains the id of an employee and their respective bonus.

Write a solution to report the name and bonus amount of each employee who satisfies either of the following:
- The employee has a bonus less than 1000.
- The employee did not get any bonus.

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

**Table: Students**
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
```
`student_id` is the primary key (column with unique values) for this table.
Each row of this table contains the ID and the name of one student in the school.

**Table: Subjects**
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
```
`subject_name` is the primary key (column with unique values) for this table.
Each row of this table contains the name of one subject in the school.

**Table: Examinations**
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
```
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each student from the Students table takes every course from the Subjects table.
Each row of this table indicates that a student with ID `student_id` attended the exam of `subject_name`.

Write a solution to find the number of times each student attended each exam.
Return the result table ordered by `student_id` and `subject_name`.
The result format is in the following example.

We first need every possible (student, subject) pair. Using `CROSS JOIN`.
Then, attach the examination records using `LEFT JOIN`.

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

**Table: Delivery**
```text
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
```
`delivery_id` is the column of unique values of this table.
The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).

If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.
The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.
Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

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

**Table: Employees**
```text
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |
+-------------+----------+
```
`employee_id` is the column with unique values for this table.
This table contains information about the employees and the id of the manager they report to. Some employees do not report to anyone (`reports_to` is null).

For this problem, we will consider a manager an employee who has at least 1 other employee reporting to them.
Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.
Return the result table ordered by `employee_id`.
The result format is in the following example.

**Example 1:**
**Input:**
Employees table:
```text
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
```
**Output:**
```text
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
```
**Explanation:** Hercy has 2 people report directly to him, Alice and Bob. Their average age is `(41+36)/2 = 38.5`, which is 39 after rounding it to the nearest integer.

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

**Table: Employee**
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| employee_id   | int     |
| department_id | int     |
| primary_flag  | varchar |
+---------------+---------+
```
`(employee_id, department_id)` is the primary key (combination of columns with unique values) for this table.
`employee_id` is the id of the employee.
`department_id` is the id of the department to which the employee belongs.
`primary_flag` is an ENUM (category) of type ('Y', 'N'). If the flag is 'Y', the department is the primary department for the employee. If the flag is 'N', the department is not the primary.

Employees can belong to multiple departments. When the employee joins other departments, they need to decide which department is their primary department. Note that when an employee belongs to only one department, their primary column is 'N'.

Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.
Return the result table in any order.
The result format is in the following example.

**Example 1:**
**Input:**
Employee table:
```text
+-------------+---------------+--------------+
| employee_id | department_id | primary_flag |
+-------------+---------------+--------------+
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |
+-------------+---------------+--------------+
```
**Output:**
```text
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |
+-------------+---------------+
```

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

**Table: Logs**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
```
In SQL, `id` is the primary key for this table.
`id` is an autoincrement column starting from 1.

Find all numbers that appear at least three times consecutively.
Return the result table in any order.
The result format is in the following example.

**Example 1:**
**Input:**
Logs table:
```text
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
```
**Output:**
```text
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
```

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

**Table: Products**
```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
```
`(product_id, change_date)` is the primary key (combination of columns with unique values) of this table.
Each row of this table indicates that the price of some product was changed to a new price at some date.
Initially, all products have price 10.

Write a solution to find the prices of all products on the date `2019-08-16`.
Return the result table in any order.
The result format is in the following example.

**Example 1:**
**Input:**
Products table:
```text
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
```
**Output:**
```text
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+
```

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

**Table: Employees**
```text
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
```
In SQL, `employee_id` is the primary key for this table.
This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (`manager_id` is null).

Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their `manager_id` set to the manager that left.
Return the result table ordered by `employee_id`.
The result format is in the following example.

**Example 1:**
**Input:**
Employees table:
```text
+-------------+-----------+------------+--------+
| employee_id | name      | manager_id | salary |
+-------------+-----------+------------+--------+
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |
+-------------+-----------+------------+--------+
```
**Output:**
```text
+-------------+
| employee_id |
+-------------+
| 11          |
+-------------+
```

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

**Table: Employee**
```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
```
`id` is the primary key (column with unique values) for this table.
`departmentId` is a foreign key (reference column) of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.

**Table: Department**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
```
`id` is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of a department and its name.

A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.
Write a solution to find the employees who are high earners in each of the departments.
Return the result table in any order.
The result format is in the following example.

**Example 1:**
**Input:**
Employee table:
```text
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
```
Department table:
```text
+----+-------+
| id | name  |
+----+-------+
| 1  | IT    |
| 2  | Sales |
+----+-------+
```
**Output:**
```text
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

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

**Table: Person**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
```
`id` is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.

Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.
For SQL users, please note that you are supposed to write a `DELETE` statement and not a `SELECT` one.

```sql
DELETE p1
FROM Person p1
JOIN Person p2
ON p1.email = p2.email
AND p1.id > p2.id;
```

---

## Problem 15: Products Sold Per Date

**Table Activities:**
```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| sell_date   | date    |
| product     | varchar |
+-------------+---------+
```
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each row of this table contains the product name and the date it was sold in a market.

Write a solution to find for each date the number of different products sold and their names.
The sold products names for each date should be sorted lexicographically.
Return the result table ordered by `sell_date`.
The result format is in the following example.

**Example 1:**
**Input:**
Activities table:
```text
+------------+------------+
| sell_date  | product    |
+------------+------------+
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |
+------------+------------+
```
**Output:**
```text
+------------+----------+------------------------------+
| sell_date  | num_sold | products                     |
+------------+----------+------------------------------+
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |
+------------+----------+------------------------------+
```

```sql
SELECT
 sell_date,
 COUNT(DISTINCT product) AS num_sold,
 GROUP_CONCAT(DISTINCT product ORDER BY product) AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
