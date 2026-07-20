<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔑 PRIMARY KEY & FOREIGN KEY</h1>
  <h2>Chapter 76</h2>
</div>

---

## 🥇 What is a PRIMARY KEY?

> [!NOTE]
> A PRIMARY KEY (PK) is a constraint that uniquely identifies every row in a table.

### Why is it called "Primary"?
A table may have several candidate keys:
- `StudentID`
- `Email`
- `AadhaarNumber`

All may uniquely identify a student. We choose one as the main identifier.

### A Primary Key has three important properties:
1. **UNIQUE:** No duplicates allowed.
2. **NOT NULL:** Every row must have a value.
3. **Only One PRIMARY KEY Per Table**

---

## 🛠️ Syntax

**Inline:**
```sql
CREATE TABLE Students(
 StudentID INT PRIMARY KEY,
 Name VARCHAR(100)
);
```

**Table-Level:**
```sql
CREATE TABLE Students(
 StudentID INT,
 Name VARCHAR(100),
 PRIMARY KEY(StudentID)
);
```

**Composite Primary Key:**
```sql
CREATE TABLE Enrollment(
 StudentID INT,
 CourseID INT,
 PRIMARY KEY(StudentID, CourseID)
);
```

---

## ⚙️ Internal Working

**Automatic Unique Index**
When a Primary Key is created, MySQL automatically creates a unique index.
- Without an index, Database scans every row.
- With a Primary Key index, Database directly locates the row.
*This makes lookups extremely fast.*

### Clustered vs Non-Clustered
*Interview favorite*

A clustered index dictates the physical sorting of the data itself on disk and acts as the table itself, meaning you can only have one per table. A non-clustered index acts like a book's back-of-the-book index, creating a separate data structure that contains pointers back to the physical data rows.

**Physical Storage & Quantity per Table:**
- You can have only one clustered index per table.
- You can have up to 999 non-clustered indexes.

**Performance:**
- Clustered indexes are significantly faster for range-based queries.
- Non-clustered indexes are typically faster for exact match lookups.

**MySQL (InnoDB):**
The Primary Key is a clustered index.

**SQL Server:**
Also supports clustered and non-clustered indexes. By default, the Primary Key is usually created as a clustered index unless specified otherwise.

---

## 🔄 Changing Primary Keys

> [!WARNING]
> Usually avoid changing primary keys in production because they are often referenced by foreign keys. If foreign keys reference this primary key, the operation will fail until those foreign keys are handled.

**Drop old PK:**
```sql
ALTER TABLE Students
DROP PRIMARY KEY;
```

**Create new PK:**
```sql
ALTER TABLE Students
ADD PRIMARY KEY(Email);
```

---

## 🔗 FOREIGN KEY

### 1. Introduction

**Referential Integrity**
A FOREIGN KEY ensures that relationships between tables remain valid.

**Suppose:**
- **Customers Table:** `CustomerID` (1, 2)
- **Orders Table:** `OrderID` (101, 102), `CustomerID` (1, 2)

*Every order must belong to an existing customer.*
- **Parent:** Customers
- **Child:** Orders
*(Orders references Customers).*

### Why Foreign Keys Exist
Without a Foreign Key, `Customer 999` might be inserted into `Orders` even if it doesn't exist. **Bad data.** Foreign Keys prevent this.

---

## 🛠️ Syntax

**Inline:**
```sql
CREATE TABLE Orders(
 OrderID INT PRIMARY KEY,
 CustomerID INT,
 FOREIGN KEY(CustomerID) REFERENCES Customers(CustomerID)
);
```

**Table-Level:**
```sql
CREATE TABLE Orders(
 OrderID INT PRIMARY KEY,
 CustomerID INT,
 CONSTRAINT fk_customer FOREIGN KEY(CustomerID) REFERENCES Customers(CustomerID)
);
```
*Named constraints make maintenance easier.*

---

## 📥 INSERT Rules
Parent first.
```sql
INSERT INTO Customers VALUES(1, 'aadz');
```
**Then:**
```sql
INSERT INTO Orders VALUES(101, 1);
```
*(If you insert `VALUES(101, 999)`, it is rejected because customer 999 doesn't exist).*

---

## 🔄 UPDATE & DELETE Rules

**DELETE Rules:**
Delete customer → Orders still exist. Database checks the foreign key action. If no cascading behavior is configured, the delete is rejected to avoid orphan rows.

**CASCADE:**
`ON DELETE CASCADE` → Delete parent → Delete children automatically.

**SET NULL:**
`ON DELETE SET NULL` → Delete parent → Foreign key column becomes `NULL`. *(Requires the foreign key column to allow `NULL`)*.

**SET DEFAULT:**
The child foreign key is set to its default value. *(Supported by some databases, not supported by MySQL InnoDB).*

**ON UPDATE CASCADE:**
Parent ID changes → Child IDs automatically updated.

---

## 🌳 Self-Referencing Foreign Keys

**Employees:**
| EmployeeID | ManagerID |
| :--- | :--- |
| 1 | NULL |
| 2 | 1 |
| 3 | 2 |

```sql
CREATE TABLE Employees(
 EmployeeID INT PRIMARY KEY,
 ManagerID INT,
 FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID)
);
```
*This models organizational hierarchies.*

---

## 🧩 Composite Foreign Keys
- **Parent:** `(StudentID, CourseID)`
- **Child:** `(StudentID, CourseID)`
Both columns together reference the composite primary (or unique) key.

---

## 🛑 Disabling Foreign Key Checks (MySQL)

```sql
SET FOREIGN_KEY_CHECKS = 0;
-- migration work
SET FOREIGN_KEY_CHECKS = 1;
```

> [!TIP]
> **Why are foreign keys used?**
> To enforce relationships between tables and prevent inconsistent or orphaned data. FOREIGN KEY references a primary (or unique) key in another (or the same) table to create relationships.
