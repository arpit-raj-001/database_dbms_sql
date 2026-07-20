<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔡 String Functions & Character Encoding</h1>
  <h2>Chapter 88</h2>
</div>

---

String functions are built-in SQL functions that accept character data as input and return manipulated character data.

## 🔠 Common Use Cases

### Formatting output
```sql
SELECT UPPER(Name)
FROM Students;
```
**Result:**
- `AADZ`
- `ARPIT`

### Case-insensitive searching
```sql
SELECT *
FROM Students
WHERE LOWER(Name) = 'aadz';
```

---

## 💻 Character Encoding?

Computers store everything as numbers.
**Encoding defines which number represents which character.**

### ASCII
Old encoding.
- **Uses:** 7 bits
- **Supports:** 128 characters
- **Examples:** `A` = `65`, `a` = `97`
- **Limitation:** Cannot represent most world languages. (Chinese, Japanese, Arabic, Emojis, Mathematical symbols).

### Unicode
Designed to represent every language.
- **Supports:** Hindi, Chinese, Emojis, etc.

### UTF-8
Most common encoding today.
- **Characteristics:** Variable length (1–4 bytes per character).
- Backward compatible with ASCII.
- Used by MySQL, PostgreSQL, web browsers, APIs.
- *Preferred encoding for modern applications.*

---

## ⚖️ Are these equal?

`aadz` | `AADZ` | `Aadz`

**Are these equal?**
**Answer:** Depends on the database collation.

---

## 🔍 What is a Collation?

A collation determines:
- comparison rules
- sorting rules
- case sensitivity

### MySQL
Most default collations are case-insensitive.
**Example:**
```sql
WHERE Name='aadz'
```
may also match: `AADZ`.

### Case-Sensitive Search
```sql
SELECT *
FROM Users
WHERE BINARY Username='Aadz';
```
**Returns only:** `Aadz` (Not `AADZ`).

### PostgreSQL
`WHERE Name='aadz'` is case-sensitive. It will not match `Aadz`.

---

## 🌐 Vendor Differences

### Oracle
Empty string `''` is treated as `NULL`.

### Unicode Considerations
`UPPER()` works with Unicode-aware databases.
**Example (German):**
`straße` may become `STRASSE`.

---

## 🤷‍♂️ NULL Behavior

```sql
SELECT UPPER(NULL);
```
**Result:** `NULL`
