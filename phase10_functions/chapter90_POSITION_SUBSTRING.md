<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>✂️ MYSQL SUBSTRING & POSITION</h1>
  <h2>Chapter 90</h2>
</div>

---

## ✂️ SUBSTRING

**Syntax:**
```sql
SUBSTRING(string, start, length)
-- or
SUBSTR(string, start, length)
```

**Example:**
```sql
SELECT SUBSTRING('Database', 1, 4);
```
**Result:** `Data`

**If omitted (MySQL):**
```sql
SELECT SUBSTRING('Programming', 5);
```
**Result:** `ramming` *(Extracts everything from position 5 onward).*

---

### Negative Positions (Vendor Support)
*Very useful interview topic.*

**MySQL (Supports):**
```sql
SELECT SUBSTRING('Database', -4);
```
**Result:** `base` *(Counts from the end).*

- **PostgreSQL:** Negative positions are not supported.
- **SQL Server:** Not supported.

---

### SUBSTRING vs LEFT vs RIGHT

| Function | Purpose |
| :--- | :--- |
| **SUBSTRING** | Any part |
| **LEFT** | Beginning only |
| **RIGHT** | End only |

---

## 📍 POSITION()

`POSITION()` finds where one string appears inside another.

**Example:**
Find `@` in `aadz@gmail.com`.
```sql
SELECT POSITION('@' IN 'aadz@gmail.com');
```
**Result:** `5` *(Think of it as SQL's `indexOf()`)*.

*Unlike `REPLACE()`, which changes text, `SUBSTRING()` returns only a selected part.*

### MySQL Specifics
MySQL doesn't support `POSITION()` as the primary function in tutorials. Instead you'll usually see `LOCATE()`.
```sql
SELECT LOCATE('@', 'aadz@gmail.com');
```
**Result:** `5`

### Word Search
```sql
SELECT POSITION('World' IN 'Hello World');
```
**Result:** `7` *(If not found, returns `0` in MySQL).*

---

## 🧩 POSITION + SUBSTRING
*Very common.*

**Extract username from email:**
```sql
SELECT SUBSTRING(
 Email, 1, 
 LOCATE('@', Email) - 1
) FROM Users;
```

---

## ➕ CONCAT()

### 11.1 Introduction
`CONCAT()` joins multiple strings into one.

**Example:**
`aadz` + `raz`
```sql
SELECT CONCAT('aadz', 'raz');
```
**Result:** `aadzraz`

Usually you want spaces:
```sql
SELECT CONCAT('aadz', ' ', 'raz');
```
**Result:** `aadz raz`

### CONCAT vs `||`
- **ANSI SQL:** `FirstName || ' ' || LastName`
- **MySQL:** Use `CONCAT()`

---

## 🔗 CONCAT_WS()
Means: **Concatenate With Separator**

Instead of:
```sql
CONCAT(FirstName, ' ', LastName)
```
You write:
```sql
CONCAT_WS(' ', FirstName, LastName)
```

**Another Example:**
```sql
CONCAT_WS(', ', City, State, Country)
```
**Result:** `Jaipur, Rajasthan, India`

---

## 🤷‍♂️ NULL Handling
*Very important.*

**Suppose:**
`FirstName` = `aadz`
`LastName` = `NULL`

### MySQL CONCAT()
```sql
SELECT CONCAT(FirstName, ' ', LastName);
```
**Result:** `NULL` *(because any `NULL` argument makes the result `NULL`).*

### CONCAT_WS() behaves differently
It skips `NULL` values.
```sql
SELECT CONCAT_WS(' ', 'aadz', NULL, 'raz');
```
**Result:** `aadz raz`
