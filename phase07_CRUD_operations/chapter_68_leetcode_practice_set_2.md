<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes (Continued)</h1>
  <h2>Chapter 68</h2>
</div>

---

## LeetCode 176 — Second Highest Salary

**Table: Employee**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `salary` | int |

**Question:**
Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return null.

### Solution

```sql
SELECT (
 SELECT DISTINCT salary
 FROM Employee
 ORDER BY salary DESC
 LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

### How it works
The inner query:
```sql
SELECT DISTINCT salary
FROM Employee
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```
returns either `200` or `(empty)`.

The outer query `SELECT ( ... ) AS SecondHighestSalary;` gracefully handles the empty result by returning `null`, successfully solving the problem.
