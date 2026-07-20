<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔗 LEFT OUTER JOIN & RIGHT OUTER JOIN</h1>
  <h2>Chapter 113</h2>
</div>

---

## LEFT OUTER JOIN

Also called simply `LEFT JOIN`. The keyword `OUTER` is optional.

```sql
SELECT ...
FROM A
LEFT JOIN B
ON condition;
```

### 15.4.1 Definition

A `LEFT JOIN` returns:
- ✅ Every row from the left table
- ✅ Matching rows from the right table
- ✅ `NULL` for right-table columns when no match exists

> [!NOTE]
> **Think of it as:**
> "Keep everything on the left. If the right side matches, bring it. Otherwise fill the right side with NULL."

### Example

**Students**
| STUDENT_ID | NAME |
| :--- | :--- |
| 1 | Arpit |
| 2 | aadz |
| 3 | Riya |

**Enrollments**
| STUDENT_ID | COURSE |
| :--- | :--- |
| 1 | DBMS |
| 2 | OS |

**Query**
```sql
SELECT
 s.name,
 e.course
FROM Students s
LEFT JOIN Enrollments e
ON s.student_id = e.student_id;
```

**Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | DBMS |
| aadz | OS |
| Riya | NULL |

*Notice Riya is still present.*

---

## INNER JOIN vs LEFT JOIN

**INNER JOIN**
Students: Arpit, aadz, Riya
⬇ Only matching rows ⬇
Arpit, aadz

**LEFT JOIN**
Students: Arpit, aadz, Riya
⬇ Keep all ⬇
Arpit, aadz, Riya(NULL)

---

## Internal Execution

SQL conceptually performs:

**Step 1:** Cartesian Product
⬇
**Step 2:** Apply ON condition
⬇
**Step 3:** Keep every matched row
⬇
**Step 4:** For unmatched left rows Generate `Left row + NULL NULL NULL`

*This automatic NULL generation is what makes LEFT JOIN special.*

---

## Multiple Matches

**Students**
| ID | NAME |
| :--- | :--- |
| 1 | Arpit |

**Enrollments**
| STUDENT_ID | COURSE |
| :--- | :--- |
| 1 | DBMS |
| 1 | OS |

**Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | DBMS |
| Arpit | OS |

*Still duplicates.*
`LEFT JOIN` never removes duplicates.

---

## ON vs WHERE (Most Important Interview Topic)

**Students**
| ID | NAME |
| :--- | :--- |
| 1 | Arpit |
| 2 | aadz |

**Enrollments**
| STUDENT_ID | COURSE |
| :--- | :--- |
| 1 | DBMS |

### Query 1
```sql
SELECT *
FROM Students s
LEFT JOIN Enrollments e
ON s.id=e.student_id;
```

**Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | DBMS |
| aadz | NULL |

### Query 2
Now:
```sql
SELECT *
FROM Students s
LEFT JOIN Enrollments e
ON s.id=e.student_id
WHERE e.course='DBMS';
```

**Output**
| student | course |
| :--- | :--- |
| Arpit | DBMS |

`aadz` disappeared.
`LEFT JOIN` became `INNER JOIN`.

**Why?**
After LEFT JOIN: `aadz NULL`
WHERE `NULL='DBMS'` ➔ FALSE
Row removed.
*Your LEFT JOIN effectively became an INNER JOIN.*

### Correct filtering

```sql
SELECT *
FROM Students s
LEFT JOIN Enrollments e
ON s.id=e.student_id
AND e.course='DBMS';
```

**Now Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | DBMS |
| aadz | NULL |

*Huge interview favorite.*

---

## Multiple LEFT JOINs

**Students**
⬇ `LEFT JOIN` Enrollment
⬇ `LEFT JOIN` Courses
⬇ `LEFT JOIN` Teachers

*Every unmatched stage continues producing NULLs.*

---

## Problem / Why

| PROBLEM | WHY |
| :--- | :--- |
| 175. Combine Two Tables | Classic LEFT JOIN |
| 577. Employee Bonus | LEFT JOIN + NULL |
| 1581. Customer Who Visited but Did Not Make Any Transactions | LEFT JOIN + IS NULL |
| 570. Managers with at Least 5 Direct Reports | LEFT JOIN understanding helps |

### Example Solution — LeetCode 175
```sql
SELECT
 p.firstName,
 p.lastName,
 a.city,
 a.state
FROM Person p
LEFT JOIN Address a
ON p.personId=a.personId;
```

### Example Solution — LeetCode 577
```sql
SELECT
 e.name,
 b.bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId=b.empId
WHERE b.bonus < 1000
 OR b.bonus IS NULL;
```

### Example Solution — LeetCode 1581
```sql
SELECT
 v.customer_id,
 COUNT(*) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
ON v.visit_id=t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```
*This is one of the most frequently asked LEFT JOIN patterns.*

---

## RIGHT OUTER JOIN

### Syntax
```sql
SELECT ...
FROM A
RIGHT JOIN B
ON ...;
```

### Definition
Keep:
- every row from the right table
- matching rows from the left
- NULL when left has no match

### Example

**Students**
| ID | NAME |
| :--- | :--- |
| 1 | Arpit |

**Enrollments**
| STUDENT_ID | COURSE |
| :--- | :--- |
| 1 | DBMS |
| 2 | OS |

**Query**
```sql
SELECT
 s.name,
 e.course
FROM Students s
RIGHT JOIN Enrollments e
ON s.id=e.student_id;
```

**Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | DBMS |
| NULL | OS |

### Should You Use RIGHT JOIN?

In industry: **Almost never.**

Instead of:
```text
A RIGHT JOIN B
```

people simply write:
```text
B LEFT JOIN A
```

Same result. Much easier to read.

---

## FULL OUTER JOIN

### Definition
Return:
- matching rows
- left-only rows
- right-only rows

Everything.

### Example

**Students**
| ID | NAME |
| :--- | :--- |
| 1 | Arpit |
| 2 | aadz |

**Enrollments**
| STUDENT_ID | COURSE |
| :--- | :--- |
| 2 | DBMS |
| 3 | OS |

**Output**
| NAME | COURSE |
| :--- | :--- |
| Arpit | NULL |
| aadz | DBMS |
| NULL | OS |

### MySQL Doesn't Support FULL OUTER JOIN

*Classic interview question.*
Simulate using:

```sql
SELECT *
FROM A
LEFT JOIN B
ON A.id=B.id
UNION
SELECT *
FROM A
RIGHT JOIN B
ON A.id=B.id;
```

Or, more commonly:
```sql
SELECT *
FROM A
LEFT JOIN B
ON A.id = B.id
UNION
SELECT *
FROM B
LEFT JOIN A
ON A.id = B.id;
```

*UNION removes duplicate matching rows, giving the effect of a FULL OUTER JOIN*

---

## Need employees that appear in only one table

**Employee**
| EMPLOYEE_ID | NAME |
| :--- | :--- |
| 2 | Arpit |
| 4 | aadz |

**Salary**
| EMPLOYEE_ID | SALARY |
| :--- | :--- |
| 2 | 5000 |
| 3 | 7000 |

**Output**
| EMPLOYEE_ID |
| :--- |
| 3 |
| 4 |

**Query**
```sql
SELECT e.employee_id
FROM Employees e
LEFT JOIN Salaries s
ON e.employee_id=s.employee_id
WHERE s.employee_id IS NULL
UNION
SELECT s.employee_id
FROM Salaries s
LEFT JOIN Employees e
ON s.employee_id=e.employee_id
WHERE e.employee_id IS NULL
ORDER BY employee_id;
```
