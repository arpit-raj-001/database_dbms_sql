<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔢 Datatypes Provide</h1>
  <h2>Chapter 61</h2>
</div>

---

**Datatypes Provide:**
- Validation
- Efficient storage
- Faster searching
- Better indexing
- Arithmetic operations
- Sorting
- Comparisons
- Query optimization

---

## 🛑 SQL requires every column to have a datatype

Every datatype has:
- storage size
- binary representation
- comparison rules
- indexing behavior
- arithmetic rules

> [!WARNING]
> **Suppose Aadz's shopping website stores `Age` as `TEXT`.**
> 
> **Problems:**
> - More storage
> - Slower comparisons
> - Incorrect sorting
> - Larger indexes
> - Poor performance

---

## 🗂️ Categories of Data Types

### Numeric Types
Store numbers.
**Examples:** `INT`, `BIGINT`, `DECIMAL`, `FLOAT`, `DOUBLE`

### Character Types
Store text.
**Examples:** `CHAR`, `VARCHAR`, `TEXT`

### Date & Time Types
Store dates and times.
**Examples:** `DATE`, `TIME`, `DATETIME`, `TIMESTAMP`, `YEAR`

### Binary Types
Store raw binary data.
**Examples:** `BINARY`, `VARBINARY`, `BLOB`

### Boolean Types
Store true/false values.
**Examples:** `BOOLEAN`, `BIT`

### JSON Type
Stores structured JSON documents.
**Example:**
```json
{
 "theme": "dark",
 "language": "English"
}
```

---

## 🔡 Deep Dive into Character Types

### CHAR(10)
Suppose you store: `aadz` *(Only 4 characters)*.
MySQL still reserves space for 10 characters (padding the remaining positions internally).
**Storage:** `|a|a|d|z| | | | | | |`

- **Advantages:** Faster for values with the same length. Predictable storage.
- **Disadvantages:** Wastes space for shorter values.

### VARCHAR
Most commonly used string datatype. Stores variable-length strings.
**Syntax:** `Name VARCHAR(100)`

Now Insert: `aadz`
Only 4 characters are stored (plus a small amount of length metadata).
**Maximum Length:** The theoretical maximum is `65,535` bytes per row.

- **Advantages:** Saves storage. Better for varying-length text.
- **Disadvantages:** Slightly more overhead than fixed-length storage.

### TEXT
`TEXT` stores large strings. Unlike `VARCHAR`, it's designed for long textual content. Large values are stored efficiently, and InnoDB may store very long `TEXT` values outside the main row with a pointer kept in the row.

- **Advantages:** Handles large text. No need to guess an upper limit like `VARCHAR(5000)`.
- **Disadvantages:** Less suitable for indexing than shorter string types. Often slower than `VARCHAR` for small strings.

---

## ❓ Why Not Store Everything as TEXT?
*One of the best interview questions.*

Imagine:
```sql
CREATE TABLE Student(
 ID TEXT,
 Name TEXT,
 CGPA TEXT,
 DOB TEXT
);
```
Everything works... until it doesn't.

### 1. Sorting
Text sorting:
`100` -> `20` -> `5`
becomes `100` -> `20` -> `5` (Alphabetical order!)

### 2. Arithmetic
Can you calculate `Salary + Bonus`? No.

### 3. Memory
Suppose `Age` is `21`.
- Stored as `INT`: ≈ 4 bytes.
- Stored as `TEXT`: Requires character storage plus additional overhead.

### 4. Index Performance
Indexes on integers are generally:
- smaller
- faster
- easier to compare
than indexes on long text values.

### 5. Validation
`Age` stored as `TEXT`. Someone inserts `Twenty One`. The database accepts it. Now your data is inconsistent.
