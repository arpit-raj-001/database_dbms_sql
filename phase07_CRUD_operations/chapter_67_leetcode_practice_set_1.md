<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📝 SQL Interview Notes</h1>
  <h2>Chapter 67</h2>
</div>

---

## LeetCode 1757 — Recyclable and Low Fat Products

**Schema: Products**
| Column Name | Type |
| :--- | :--- |
| `product_id` | int |
| `low_fats` | enum |
| `recyclable` | enum |

**Question:**
Return products that are both:
- low fat
- recyclable

### Solution (MySQL / PostgreSQL / SQL Server)

```sql
SELECT product_id
FROM Products
WHERE low_fats='Y'
AND recyclable='Y';
```

---

## LeetCode 584 — Find Customer Referee

**Table: Customer**
| Column Name | Type |
| :--- | :--- |
| `id` | int |
| `name` | varchar |
| `referee_id` | int |

**Question:**
Find the names of the customer that are either:
- referred by any customer with `id != 2`.
- not referred by any customer.

### Solution

```sql
# Write your MySQL query statement below
SELECT name
FROM Customer
WHERE referee_id != 2
OR referee_id IS NULL;
```
