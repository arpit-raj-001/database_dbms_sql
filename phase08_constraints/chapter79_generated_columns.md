<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>⚙️ GENERATED COLUMNS</h1>
  <h2>Chapter 79</h2>
</div>

---

A **Generated Column** is a column whose value is automatically calculated from other columns in the same row. The database computes their value automatically.

### Example:

| FirstName | LastName | FullName |
| :--- | :--- | :--- |
| `aadz` | `raz` | `aadz raz` |
| `Arpit` | `Raj` | `Arpit Raj` |

**You only store:**
- `FirstName`
- `LastName`

**The database computes:**
`FullName = CONCAT(FirstName, ' ', LastName)`

---

## 🗂️ Types of Generated Columns

| Virtual Columns | Stored Columns |
| :--- | :--- |
| • Calculated every time you read the row. | • Calculated once during INSERT/UPDATE. |
| • Not stored on disk. | • Stored physically like normal columns. |

---

## 🛠️ Syntax (MySQL)

```sql
CREATE TABLE Products(
 Price DECIMAL(10,2),
 Quantity INT,
 Total DECIMAL(10,2) GENERATED ALWAYS AS (Price * Quantity)
);
```

Whenever `Price` or `Quantity` changes, `Total` changes automatically.

### Another Example
```sql
CREATE TABLE Employees(
 FirstName VARCHAR(50),
 LastName VARCHAR(50),
 FullName VARCHAR(101) GENERATED ALWAYS AS (CONCAT(FirstName, ' ', LastName))
);
```

---

## 💨 Virtual Generated Columns

These are not stored. Database computes them every `SELECT`.

**Example:**
```sql
CREATE TABLE Products(
 Price DECIMAL(10,2),
 Quantity INT,
 Total DECIMAL(10,2) GENERATED ALWAYS AS (Price * Quantity) VIRTUAL
);
```

- ✅ **Advantages:** Saves storage, Always up to date.
- ❌ **Disadvantages:** Every SELECT performs the calculation.

---

## 💾 Stored Generated Columns

Stored physically.

```sql
CREATE TABLE Products(
 Price DECIMAL(10,2),
 Quantity INT,
 Total DECIMAL(10,2) GENERATED ALWAYS AS (Price * Quantity) STORED
);
```

- ✅ **Advantages:** Very fast SELECT.
- ❌ **Disadvantages:** Uses extra storage.

---

## 📜 Rules for Generated Expressions

Generated expressions generally:
- Can reference columns from the same row.
- Cannot reference other tables.

---

## 🆚 Generated Columns vs Views
*This interview question is common.*

- **Generated Column:** Inside table, One table, Automatic per row, Can be stored.
- **View:** Separate virtual table/object, Multiple tables possible, Query-based, Usually not stored.

**View Example:**
```sql
CREATE VIEW ProductView AS
SELECT
 Price,
 Quantity,
 Price*Quantity AS Total
FROM Products;
```

---

## ⚡ Indexing Generated Columns

Suppose `Price * Quantity` is searched frequently.
- **Without index:** `Every row ↓ Multiply ↓ Compare` (Slow).
- **Stored generated column:** `Stored generated column ↓ Index ↓ Fast`.

**Example:**
```sql
CREATE INDEX idx_total
ON Products(Total);
```

**In MySQL:**
- Stored generated columns can be indexed.
- Virtual generated columns can also be indexed in MySQL 8.0

---

## 🌐 Vendor Comparison

| Database | Support |
| :--- | :--- |
| **MySQL** | Virtual + Stored |
| **PostgreSQL** | Stored only |
| **SQL Server** | Computed Columns (virtual or persisted) |
| **Oracle** | Virtual Columns |
| **SQLite** | Virtual + Stored |
