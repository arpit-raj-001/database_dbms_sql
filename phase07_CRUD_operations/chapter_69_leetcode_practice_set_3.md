<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes (Continued)</h1>
  <h2>Chapter 69</h2>
</div>

---

## 183. Customers Who Never Order

**Table: Customers**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |

**Table: Orders**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `customerId` | int |

* `id` is the primary key for Customers.
* `customerId` is a foreign key to Customers.

**Question:** Write a solution to find all customers who never order anything.

### Solution
```sql
SELECT name AS Customers
FROM Customers
WHERE id NOT IN (
 SELECT customerId 
 FROM Orders
);
```

---

## 1350. Students With Invalid Departments

**The Goal:** Find the ID and name of students who are enrolled in a department that no longer exists in the Departments table.

### Solution
```sql
SELECT id, name
FROM Students
WHERE department_id NOT IN (
 SELECT id 
 FROM Departments
);
```

---

## 2026. Low-Quality Problems

**The Goal:** Find problems where the "like percentage" (likes divided by total votes) is strictly less than 60%. Sort the result by problem ID.

### Solution
```sql
SELECT problem_id
FROM Problems
WHERE (likes / (likes + dislikes)) < 0.60
ORDER BY problem_id ASC;
```

---

## 1821. Find Customers With Positive Revenue this Year

**The Goal:** Find customers who have a revenue strictly greater than zero in the year 2021.

### Solution
```sql
SELECT customer_id
FROM Customers
WHERE year = 2021 
  AND revenue > 0;
```

---

## 1667. Fix Names in a Table

**Table: Users**
| Column Name | Type |
| :--- | :--- |
| `user_id` | int |
| `name` | varchar |

**Question:** Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase. Return the result table ordered by `user_id`.

### Solution
```sql
SELECT user_id, 
  CONCAT(
    UPPER(SUBSTRING(name, 1, 1)), 
    LOWER(SUBSTRING(name, 2))
  ) AS name
FROM Users
ORDER BY user_id;
```

---

## 620. Not Boring Movies

**Table: Cinema**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `movie` | varchar |
| `description` | varchar |
| `rating` | float |

**Question:** Write a solution to report the movies with an odd-numbered ID and a description that is not "boring". Return the result table ordered by rating in descending order.

### Solution
```sql
SELECT id, movie, description, rating 
FROM Cinema 
WHERE id % 2 != 0 
  AND description != 'boring' 
ORDER BY rating DESC;
```

---

## 607. Sales Person

**Tables:** SalesPerson, Company, Orders.
`com_id` is a foreign key to the Company table.
`sales_id` is a foreign key to the SalesPerson table.

**Question:** Write a solution to find the names of all the salespersons who did not have any orders related to the company with the name "RED".

### Solution
```sql
SELECT name 
FROM SalesPerson 
WHERE sales_id NOT IN (
 SELECT sales_id 
 FROM Orders 
 WHERE com_id IN (
  SELECT com_id 
  FROM Company 
  WHERE name = 'RED'
 )
);
```

---

## 1084. Sales Analysis III

**Tables:** Product, Sales.
**Question:** Write a solution to report the products that were only sold in the first quarter of 2019. That is, between 2019-01-01 and 2019-03-31 inclusive.

### Solution
```sql
SELECT product_id, product_name
FROM Product
WHERE product_id IN (
 SELECT product_id
 FROM Sales
 WHERE sale_date BETWEEN '2019-01-01' AND '2019-03-31'
)
AND product_id NOT IN (
 SELECT product_id
 FROM Sales
 WHERE sale_date NOT BETWEEN '2019-01-01' AND '2019-03-31'
);
```

---

## 1495. Friendly Movies Streamed Last Month

**The Goal:** Report the distinct titles of the kid-friendly movies streamed in June 2020.

### Solution
```sql
SELECT DISTINCT title 
FROM Content 
WHERE Kids_content = 'Y' 
  AND content_type = 'Movies' 
  AND content_id IN (
   SELECT content_id 
   FROM TVProgram 
   WHERE program_date BETWEEN '2020-06-01' AND '2020-06-30'
  );
```

---

## 1683. Invalid Tweets

**Table: Tweets**
| Column Name | Type |
| :--- | :--- |
| `tweet_id` | int |
| `content` | varchar |

**Question:** Write a solution to find the IDs of the invalid tweets. The tweet is invalid if the number of characters used in the content of the tweet is strictly greater than 15.

### Solution
```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content)>15;
```

---

## 627. Swap Salary

**Table: Salary**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |
| `sex` | ENUM |
| `salary` | int |

**Question:** The `sex` column is ENUM value of type ('m', 'f'). Write a solution to swap all 'f' and 'm' values with a single update statement and no intermediate temporary tables.

### Solution
```sql
UPDATE Salary
SET sex = CASE
 WHEN sex = 'm' THEN 'f'
 ELSE 'm'
END;
```

---

## 1527. Patients With a Condition

**Table: Patients**
| Column Name | Type |
| :--- | :--- |
| `patient_id` | int |
| `patient_name` | varchar |
| `conditions` | varchar |

**Question:** 'conditions' contains 0 or more codes separated by spaces. Write a solution to find the `patient_id`, `patient_name`, and `conditions` of the patients who have Type I Diabetes. Type I Diabetes always starts with `DIAB1` prefix.

### Solution
```sql
SELECT patient_id, patient_name, conditions 
FROM Patients 
WHERE conditions LIKE 'DIAB1%' 
   OR conditions LIKE '% DIAB1%';
```

---

## 1873. Calculate Special Bonus

**Table: Employees**
| Column Name | Type |
| :--- | :--- |
| `employee_id` | int |
| `name` | varchar |
| `salary` | int |

**Question:** Write a solution to calculate the bonus of each employee. The bonus of an employee is 100% of their salary if the ID of the employee is an odd number and the employee's name does not start with the character 'M'. The bonus is 0 otherwise.

### Solution
```sql
SELECT employee_id,
 CASE 
  WHEN employee_id % 2 != 0 AND name NOT LIKE 'M%' THEN salary 
  ELSE 0 
 END AS bonus
FROM Employees
ORDER BY employee_id;
```

---

## 2377. Sort the Olympic Table

**The Goal:** Sort the countries to create a leaderboard. Sort by gold medals, then silver, then bronze (all descending). If there’s a tie, sort by the country's name alphabetically (ascending).

### Solution
```sql
SELECT country, gold, silver, bronze
FROM Olympic
ORDER BY gold DESC, silver DESC, bronze DESC, country ASC;
```
