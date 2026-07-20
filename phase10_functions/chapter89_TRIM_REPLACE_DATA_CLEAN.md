<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>✂️ SQL String Functions (INITCAP, TRIM, REPLACE)</h1>
  <h2>Chapter 89</h2>
</div>

---

## 🔠 INITCAP()

`INITCAP()` converts a string into Title Case.

**Title Case means:**
- The first letter of each word becomes uppercase.
- All remaining letters become lowercase.

**Example:**
`aadz raz` becomes `Aadz Raz`
`HELLO WORLD` becomes `Hello World`

Unlike `UPPER()` or `LOWER()`, `INITCAP()` changes both uppercase and lowercase letters to produce properly formatted words.

**Syntax (PostgreSQL):**
```sql
SELECT INITCAP(column_name) FROM Students;
```

### Vendor Support
| Database | Supports INITCAP()? |
| :--- | :--- |
| **PostgreSQL** | ✅ Yes |
| **Oracle** | ✅ Yes |
| **MySQL** | ❌ No |
| **SQL Server**| ❌ No |
| **SQLite** | ❌ No |

**MySQL Workaround:**
MySQL has no built-in `INITCAP()`. For a single-word string, you can simulate it using:
```sql
SELECT CONCAT(
 UPPER(LEFT(Name, 1)),
 LOWER(SUBSTRING(Name, 2))
)
FROM Students;
```

---

## ✂️ TRIM()

`TRIM()` removes unwanted characters from the beginning and/or end of a string.
By default, it removes spaces.

**Example:**
`' aadz '` becomes `'aadz'`

*Notice that spaces inside the string are not removed.*
`'aadz raz'` remains `'aadz raz'`

**Removing Specific Characters:**
`TRIM` can remove characters other than spaces.
```sql
SELECT TRIM('*' FROM '***SQL***');
-- Result: SQL

SELECT TRIM('-' FROM '---Hello---');
-- Result: Hello
```
*TRIM only removes characters at the edges. `AB*C*D` with `TRIM('*')` results in `AB*C*D` (Nothing changes).*

---

### LEADING & TRAILING

**LEADING:** Removes characters only from the beginning.
```sql
SELECT TRIM(LEADING '*' FROM '***SQL***');
-- Result: SQL***
```

**TRAILING:** Removes characters only from the end.
```sql
SELECT TRIM(TRAILING '*' FROM '***SQL***');
-- Result: ***SQL
```

> [!TIP]
> **Best Practices:**
> - Trim user input before inserting into the database.
> - Store clean data instead of trimming every query.
> - Use `TRIM()` mainly for presentation or cleanup.

---

### LTRIM() & RTRIM()

- **LTRIM():** Removes characters from the left (beginning) of a string. Trailing spaces remain.
- **RTRIM():** Removes characters from the right (end) of a string.

*(Historically, they removed only spaces, but PostgreSQL supports `LTRIM('***SQL', '*')`).*

---

## 🔄 REPLACE()

`REPLACE()` searches for a specified substring within a string and replaces every occurrence of that substring with another substring.
Think of it as SQL's global Find and Replace.

**Examples:**
| Type | Original Data | Clean Data |
| :--- | :--- | :--- |
| Phone numbers | `987-654-3210` | `9876543210` |
| Currency | `₹25,000` | `25000` |
| Emails | `AADZ@GMAIL.COM` | `aadz@gmail.com` |

**Syntax:**
```sql
SELECT REPLACE('Hello SQL', 'SQL', 'MySQL');
-- Result: Hello MySQL

SELECT REPLACE('Aadz', 'a', '*');
-- Result: A*dz (Only lowercase a is replaced)
```

### Remove Characters
Simply replace with an empty string.
```sql
SELECT REPLACE('987-654-3210', '-', '');
-- Result: 9876543210
```

### Nested REPLACE()
Need multiple replacements? Simply nest them.
```sql
SELECT REPLACE( REPLACE('₹25,000', '₹', ''), ',', '');
-- Result: 25000
```

---

## 🤷‍♂️ NULL Behavior

Like most SQL string functions, if any required argument is `NULL`, the result is generally `NULL`.

```sql
SELECT REPLACE(NULL, 'a', 'b');
-- Result: NULL
```

> [!TIP]
> A very common interview pattern is cleaning imported values using nested string functions.
