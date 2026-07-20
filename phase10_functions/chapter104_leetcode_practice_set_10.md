<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes (Set 10)</h1>
  <h2>Chapter 104</h2>
</div>

---

## 1. Employees Earning More Than Their Managers

**Table: Employee**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |
| `salary` | int |
| `managerId` | int |

`id` is the primary key. Each row indicates the ID of an employee, their name, salary, and the ID of their manager.
Write a solution to find the employees who earn more than their managers.

### Solution
```sql
# Write your MySQL query statement below
SELECT
 e.name AS Employee
FROM Employee e
JOIN Employee m
ON e.managerId = m.id
WHERE e.salary > m.salary;
```

---

## 2. Duplicate Emails

**Table: Person**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `email` | varchar |

Write a solution to report all the duplicate emails.

### Solution
```sql
# Write your MySQL query statement below
SELECT
 email AS Email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;
```

---

## 3. Department Highest Salary

**Table: Employee**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |
| `salary` | int |
| `departmentId` | int |

**Table: Department**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |

Write a solution to find employees who have the highest salary in each of the departments.

### Solution
```sql
SELECT
 d.name AS Department,
 e.name AS Employee,
 e.salary AS Salary
FROM Employee e
JOIN Department d
ON e.departmentId = d.id
JOIN
(
 SELECT
 departmentId,
 MAX(salary) AS max_salary
 FROM Employee
 GROUP BY departmentId
) t
ON e.departmentId = t.departmentId
AND e.salary = t.max_salary;
```

---

## 4. Trips and Users

**Table: Trips**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `client_id` | int |
| `driver_id` | int |
| `city_id` | int |
| `status` | enum |
| `request_at` | varchar |

**Table: Users**
| Column Name | Type |
| :--- | :--- |
| `users_id` | int |
| `banned` | enum |
| `role` | enum |

Write a solution to find the cancellation rate of requests with unbanned users (both client and driver must not be banned) each day between "2013-10-01" and "2013-10-03".

### Solution
```sql
# Write your MySQL query statement below
SELECT
 t.request_at AS Day,
 ROUND(
 AVG(
 CASE
 WHEN t.status = 'completed' THEN 0
 ELSE 1
 END
 ),
 2
 ) AS 'Cancellation Rate'
FROM Trips t
JOIN Users c
ON t.client_id = c.users_id
JOIN Users d
ON t.driver_id = d.users_id
WHERE c.banned = 'No'
AND d.banned = 'No'
AND t.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY t.request_at;
```
