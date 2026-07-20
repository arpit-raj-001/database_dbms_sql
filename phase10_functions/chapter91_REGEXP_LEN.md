<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📏 String Length Functions & Regular Expressions</h1>
  <h2>Chapter 91</h2>
</div>

---

## 📏 String Length Functions

`LENGTH()` returns the length of a string.
Depending on the database, this length may represent:
- Number of characters
- Number of bytes

**Example:**
```sql
SELECT CHAR_LENGTH('aadz');
```
**Result:** `4`

---

## 💾 Byte Length
*Very important.*

### ASCII
Suppose `ABC`
Each character → 1 byte.

```sql
SELECT LENGTH('ABC');
```
**Result:** `3`

### Unicode
Suppose `😊` (Emoji)
- Character count → 1
- Byte count → 4 (UTF-8)

So:
```sql
SELECT LENGTH('😊');
```
**Result:** `4`

While:
```sql
SELECT CHAR_LENGTH('😊');
```
**Result:** `1`

---

## 🏢 Vendor Differences
*This is the interview favorite.*

| Function | Measures |
| :--- | :--- |
| `CHAR_LENGTH` | Characters |
| `LENGTH` | Bytes (MySQL) |

**Example: `😊😊`**
| Function | Result |
| :--- | :--- |
| `CHAR_LENGTH` | `2` |
| `LENGTH` | `8` |

### Database specific LENGTH() behavior:
- **MySQL:** Bytes
- **PostgreSQL:** Characters
- **Oracle:** Characters
- **SQLite:** Characters

---

### Phone Number Validation
Exactly 10 digits.
```sql
SELECT *
FROM Customers
WHERE CHAR_LENGTH(phone) = 10;
```

---

## 🤷‍♂️ NULL Behavior

```sql
SELECT LENGTH(NULL);
```
**Result:** `NULL`

---

## 🧩 Regular Expressions (REGEXP)

### 13.1 Introduction
`LIKE` is useful for simple patterns.
- Starts with: `A%`
- Ends with: `%.com`
- Contains: `%SQL%`

But what if you need:
- valid email
- only digits
- exactly 10 digits
- starts with capital letter
- optional country code
- hashtags
- URLs

*That's where Regular Expressions (Regex) come in.*

### REGEXP Syntax (MySQL)
```sql
SELECT *
FROM Users
WHERE Name REGEXP 'pattern';
```

**Example:** Names beginning with A.
```sql
SELECT *
FROM Users
WHERE Name REGEXP '^A';
```

---

## 🔍 Regex Building Blocks

### Basic Patterns
| Description | Pattern | Description | Pattern |
| :--- | :--- | :--- | :--- |
| Any Digit | `[0-9]` | Any Letter | `[A-Za-z]` |
| Uppercase | `[A-Z]` | Lowercase | `[a-z]` |
| Whitespace | `[[:space:]]` | Digit (MySQL 8+) | `[[:digit:]]` |

### 13.4 Wildcards
`.` means any single character.
**Example:** `A.C` Matches: `ABC`, `AXC`, `A9C`

### 13.5 Quantifiers
*Very important.*

| Quantifier | Meaning | Example | Matches |
| :--- | :--- | :--- | :--- |
| `*` | Zero or more | `ab*` | `a`, `ab`, `abb`, `abbbbb` |
| `+` | One or more | `ab+` | `ab`, `abb`, `abbb` |
| `?` | Optional | `colou?r` | `color`, `colour` |
| `{}` | Exactly | `[0-9]{6}`| Exactly six digits. |
| Range | Between | `[0-9]{8,12}`| Between 8 and 12 digits. |

---

### 13.6 Character Classes
- `[A-Z]`: Uppercase
- `[0-9]`: Digits
- `[a-z]`: Lowercase
- `[A-Za-z0-9]`: Alphanumeric

### 13.7 Anchors
- `^`: Beginning
- `$`: End

**Example: Exactly six digits:**
`^[0-9]{6}$`

### 13.8 Alternation
Like OR.
`cat|dog` *(Matches cat or dog)*

### 13.9 Grouping
`(abc)` Groups expressions. Useful with alternation.
`(cat|dog)`

### 13.10 Escaping Characters
- Need literal `.` → Write `\.`
- Literal `+` → Write `\+`

---

## 🛠️ REGEXP Functions

### REGEXP_REPLACE
Replace using regex.
**Example: Remove every digit:**
```sql
REGEXP_REPLACE(Text, '[0-9]', '')
```

### REGEXP_SUBSTR
Extract matching text.

### REGEXP_INSTR
Returns the position of the first regex match.

---

## 🎯 Practical Examples

**Validate Email (Very common):**
```sql
WHERE Email REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'
```

**Validate Phone (Exactly 10 digits):**
```sql
WHERE Phone REGEXP '^[0-9]{10}$'
```

**Validate PIN (Exactly six digits):**
```sql
WHERE PIN REGEXP '^[0-9]{6}$'
```
