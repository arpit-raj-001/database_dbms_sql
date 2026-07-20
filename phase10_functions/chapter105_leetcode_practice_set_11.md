<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Practice Notes (Set 11)</h1>
  <h2>Chapter 105</h2>
</div>

---

## Stadium (Consecutive Rows)

**Table:** Stadium
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `visit_date` | date |
| `people` | int |

`visit_date` is the column with unique values. Each row contains the visit date and visit id to the stadium. As the id increases, the date increases as well.

Write a solution to display the records with three or more rows with consecutive id's, and the number of people is greater than or equal to 100 for each. Return the result table ordered by `visit_date` in ascending order.

### Query:
```sql
SELECT DISTINCT s1.*
FROM Stadium s1
JOIN Stadium s2
ON s2.id = s1.id + 1
JOIN Stadium s3
ON s3.id = s1.id + 2
WHERE s1.people >= 100
AND s2.people >= 100
AND s3.people >= 100

UNION

SELECT DISTINCT s2.*
FROM Stadium s1
JOIN Stadium s2
ON s2.id = s1.id + 1
JOIN Stadium s3
ON s3.id = s1.id + 2
WHERE s1.people >= 100
AND s2.people >= 100
AND s3.people >= 100

UNION

SELECT DISTINCT s3.*
FROM Stadium s1
JOIN Stadium s2
ON s2.id = s1.id + 1
JOIN Stadium s3
ON s3.id = s1.id + 2
WHERE s1.people >= 100
AND s2.people >= 100
AND s3.people >= 100

ORDER BY id;
```

**Explanation:**
The `UNION` is needed because each query returns only one position of the valid 3-day sequence.
If we have a valid 3-day sequence `id=5, 6, 7`:
- First query returns `id=5`
- Second query returns `id=6`
- Third query returns `id=7`
Combining them with `UNION` gives exactly what the problem wants.
