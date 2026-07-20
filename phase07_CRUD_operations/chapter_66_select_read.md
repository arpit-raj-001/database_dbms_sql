<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📖 SELECT Basics</h1>
  <h2>Chapter 66</h2>
</div>

---

`SELECT` is used to retrieve data from one or more tables.

**General syntax:**
```sql
SELECT column_list
FROM table_name;
```

---

## 🌟 SELECT *

The simplest query:
```sql
SELECT *
FROM Students;
```
`*` means: Return every column.

### Internal Working
Execution begins with `FROM Students`. MySQL locates the table.
Then `SELECT *` returns every column for every row.

> [!WARNING]
> Suppose a table has:
> - 80 columns
> - 20 million rows
> 
> Using `SELECT *` reads unnecessary data.

**Instead:**
```sql
SELECT Name
FROM Students;
```
*Reads only one column. Less I/O. Better performance.*

---

## 🎭 Aliases (AS)

Aliases give temporary names to columns or tables.

**Syntax:**
```sql
SELECT column AS alias
FROM table;
```

**Example:**
```sql
SELECT Name AS Student_Name
FROM Students;
```
**Output:**
| Student_Name |
| :--- |
| `aadz` |
| `Arpit` |

*The original column name is not changed.*

### Multiple aliases
```sql
SELECT
 Name AS Student_Name,
 Age AS Student_Age,
 Department AS Branch
FROM Students;
```

### Table Alias
```sql
SELECT s.Name
FROM Students AS s;
```
*We'll use table aliases heavily in JOINs.*

---

## 🎯 DISTINCT

Suppose **Department** has: `ECE`, `ECE`, `CSE`, `ECE`, `ME`

**Query:**
```sql
SELECT DISTINCT Department
FROM Students;
```

**Output:**
| Department |
| :--- |
| `ECE` |
| `CSE` |
| `ME` |
*Duplicates are removed.*

### Multiple columns
```sql
SELECT DISTINCT Department, Gender
FROM Students;
```
*Uniqueness is checked on the combination of both columns.*

> [!NOTE]
> **Internal Working:**
> MySQL compares rows and removes duplicate combinations before returning the result. `DISTINCT` often requires sorting or hashing internally. Don't use it unless you actually need unique results.

---

## 🔍 WHERE

Probably the most frequently used SQL clause. `WHERE` filters rows.

**Syntax:**
```sql
SELECT *
FROM Students
WHERE Age > 20;
```
*Only matching rows are returned.*

---

## ⚖️ Comparison Operators

| Operator | Meaning |
| :--- | :--- |
| `=` | Equal |
| `!=` | Not equal (MySQL) |
| `<>` | Not equal (ANSI SQL) |

---

## 🧠 Logical Operators

### AND
*Both conditions must be true.*
```sql
SELECT *
FROM Students
WHERE Department = 'ECE'
AND CGPA > 8.5;
```

### OR
*At least one condition must be true.*
```sql
SELECT *
FROM Students
WHERE Department = 'ECE'
OR Department = 'CSE';
```

### NOT
*Negates a condition.*
```sql
SELECT *
FROM Students
WHERE NOT Department = 'ECE';
```

> [!TIP]
> **Operator precedence:**
> 1. NOT
> 2. AND
> 3. OR
> 
> *Use parentheses when combining multiple conditions.*

---

## 📏 BETWEEN

Checks whether a value lies within an inclusive range.
```sql
SELECT *
FROM Students
WHERE Age BETWEEN 18 AND 22;
```
*(Equivalent to `WHERE Age >= 18 AND Age <= 22;`)*

**Works with:** Numbers, Dates, Strings *(lexicographically)*.

---

## 📥 IN & NOT IN

Instead of writing multiple `OR` conditions:
```sql
WHERE Department='ECE' OR Department='CSE' OR Department='IT'
```
**Write:**
```sql
WHERE Department IN ('ECE','CSE','IT');
```

**NOT IN:** *(Opposite of IN)*
```sql
WHERE Department NOT IN ('ECE','CSE');
```

---

## 🃏 LIKE (Pattern Matching)

Used for pattern matching.
**Two wildcards:**
- `%` (Zero or more characters)
- `_` (Exactly one character)

**Starts with A:**
```sql
WHERE Name LIKE 'A%';
```

**Ends with a:**
```sql
WHERE Name LIKE '%a';
```

**Contains "ad":**
```sql
WHERE Name LIKE '%ad%';
```

**Matches exactly one character:**
```sql
WHERE Name LIKE '_a%';
```

**Raj Another:**
```sql
WHERE Name LIKE 'A____';
```
*(Matches names starting with A and having exactly five characters. Example: aadz).*

---

## 🚫 IS NULL / IS NOT NULL

> [!WARNING]
> Never compare `NULL` with `=`.
> **Wrong:** `WHERE Department = NULL;`
> **Correct:** `WHERE Department IS NULL;`

---

## 🔀 ORDER BY

Sorts the result.
**Ascending (default):**
```sql
SELECT * FROM Students ORDER BY Age;
```
**Explicit:** `ORDER BY Age ASC;`
**Descending:** `ORDER BY CGPA DESC;`

**Multiple columns:**
```sql
ORDER BY Department ASC, CGPA DESC;
```
*(First sorts by Department. Within each department, sorts by CGPA descending).*

---

## 🛑 LIMIT & OFFSET

**LIMIT (MySQL):** Returns only the first N rows.
```sql
SELECT * FROM Students LIMIT 5;
```

**OFFSET:** Skip rows.
```sql
SELECT * FROM Students LIMIT 5 OFFSET 10;
```
*(Skip first 10 rows, return next 5. Useful for pagination).*

> [!NOTE]
> **ANSI SQL syntax:** `FETCH FIRST 5 ROWS ONLY;`
> **SQL Server:** `SELECT TOP 5 * FROM Students;`

---

## ➕ Expressions & Functions in SELECT

**Arithmetic:**
```sql
SELECT Name, CGPA + 0.5 AS UpdatedCGPA FROM Students;
```

**String concatenation (MySQL):**
```sql
SELECT CONCAT(Name, ' - ', Department) AS StudentInfo FROM Students;
```

**Functions:**
```sql
SELECT UPPER(Name), LOWER(Name), LENGTH(Name) FROM Students;
```

---

## 🔀 CASE Statement
*SQL's equivalent of if-else.*

```sql
SELECT Name,
CASE
 WHEN CGPA >= 9 THEN 'Excellent'
 WHEN CGPA >= 8 THEN 'Good'
 ELSE 'Average'
END AS Grade
FROM Students;
```

---

## 💬 Comments
- **Single line:** `-- This is a comment`
- **MySQL specific:** `# This is a comment`
- **Multi-line:** `/* Multiple line comment */`

---

## 🏃‍♂️ Execution Order of SELECT
*This is an extremely common interview question.*

**Although we write:**
1. `SELECT`
2. `FROM`
3. `WHERE`
4. `ORDER BY`

**SQL logically processes it as:**
1. `FROM`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. `SELECT`
6. `ORDER BY`
7. `LIMIT`

> [!IMPORTANT]
> `SELECT` is not executed first, even though it appears first in the query. This execution order becomes crucial when you start using `GROUP BY`, aggregates, and aliases.
