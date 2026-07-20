<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🃏 LIKE & ILIKE Operators</h1>
  <h2>Chapter 82</h2>
</div>

---

## 7.1 Introduction

### What is LIKE?
`LIKE` is used to search for patterns inside strings.
Unlike `=`, which requires an exact match, `LIKE` allows partial matching.

**Example:**
```sql
SELECT * FROM Students WHERE Name = 'aadz';
```
*Only matches: `aadz`*

**But:**
```sql
SELECT * FROM Students WHERE Name LIKE 'A%';
```
*Matches: `aadz`, `Arpit`*

---

## 🃏 Wildcards

- `%`: Represents zero or more characters.
- `_`: Represents exactly one character.

**Example:**
```sql
WHERE Name LIKE '_a%'
```
**Matches:**
- `Raj`
- `Ram`
- `Harsh`

```sql
WHERE Name LIKE 'A____'
```
*Matches exactly five-letter names starting with A. e.g: `aadz`*

---

## 🛡️ Escaping Wildcards

Suppose your data actually contains `50%`.

**Searching:**
```sql
WHERE Discount LIKE '50%'
```
means *Starts with 50*, **not** *Contains the literal %*.

To search for a real `%`, escape it.

**MySQL:**
```sql
WHERE Discount LIKE '50\%' ESCAPE '';
```

Similarly, to search for an underscore:
```sql
WHERE Code LIKE 'AB\_12' ESCAPE '';
```

---

## ⚡ Index Considerations
*This is an important interview topic.*

Suppose an index exists on `Name`.

### Starts With
```sql
WHERE Name LIKE 'A%'
```
The index can usually be used because the database knows where names starting with `A` begin in the index.

### Ends With
```sql
WHERE Name LIKE '%A'
```
Usually **cannot efficiently use** a normal B-tree index because the beginning of the string is unknown.

---

## 🔍 ILIKE (Case-Insensitive Search)

> [!IMPORTANT]
> `ILIKE` is not supported in MySQL. It is a PostgreSQL-specific operator. Different databases treat string comparisons differently.

**In PostgreSQL:**
```sql
WHERE Name LIKE 'aadz'
```
is case-sensitive. It will not match `Aadz`.

To perform a case-insensitive search, PostgreSQL provides **ILIKE**.

### Syntax (PostgreSQL)
```sql
SELECT * FROM Students WHERE Name ILIKE 'aadz';
```

**Matches:**
- `aadz`
- `AADZ`
- `aadz`
- `AaDz`

**Universal Workaround:**
```sql
WHERE LOWER(Name) LIKE 'a%';
```
*Works in MySQL, PostgreSQL, SQL Server, and Oracle.*
