<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🛠️ What is CRUD? & INSERT Basics</h1>
  <h2>Chapter 65</h2>
</div>

---

## 📚 What is CRUD?

**CRUD** is an acronym for the four basic operations performed on data in a database.

| Letter | Meaning | Implemented using | Category | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **C** | Create | `INSERT` | DML | Because you're modifying table data. |
| **R** | Read | `SELECT` | DQL | You're only retrieving data. |
| **U** | Update | `UPDATE` | DML | |
| **D** | Delete | `DELETE` | DML | |

### Is `CREATE TABLE Student(...)` CRUD?
**No.**
It creates a *table*, not a record. CRUD deals with rows of data, not the database structure.

---

## 👣 The 4 Steps

### Step 1 — Create
Suppose a new student joins.
```sql
INSERT INTO Students
VALUES (1, 'aadz', 20);
```
*Row created.*

### Step 2 — Read
Teacher searches for the student.
```sql
SELECT * FROM Students;
```
*Row retrieved.*

### Step 3 — Update
Student changes age.
```sql
UPDATE Students
SET Age = 21
WHERE StudentID = 1;
```
*Row modified.*

### Step 4 — Delete
Student record is removed.
```sql
DELETE FROM Students
WHERE StudentID = 1;
```
*Row deleted.*

---

## 🏫 Setting Up The Database

```sql
CREATE DATABASE CollegeDB;
USE CollegeDB;

CREATE TABLE Students(
 StudentID INT AUTO_INCREMENT PRIMARY KEY,
 Name VARCHAR(100) NOT NULL,
 Age TINYINT UNSIGNED,
 Gender ENUM('Male','Female','Other'),
 Department VARCHAR(50),
 CGPA DECIMAL(3,2),
 Email VARCHAR(255) UNIQUE,
 AdmissionDate DATE
);
```

---

## ➕ INSERT Basics

```sql
INSERT INTO Students
VALUES (1, 'aadz', 20, 'Female', 'ECE', 7.97, '24uec006@gmail.com', '2025-08-20');
```
*One new row is inserted.*

### General Syntax

**Method 1 (All Columns)**
```sql
INSERT INTO table_name VALUES (...);
```

**Method 2 (Recommended)**
```sql
INSERT INTO table_name(col1, col2, ...) VALUES (...);
```

### Inserting Specific Columns
This is the most common style in production.
```sql
INSERT INTO Students(Name, Age)
VALUES ('aadz', 20);
```
Only Name and Age are provided. All other columns receive:
- their `DEFAULT` value,
- `NULL`,
- or an `AUTO_INCREMENT` value (if applicable).

---

## 🚀 Multiple Row Insert

Instead of:
```sql
INSERT INTO Students(Name, Age) VALUES('aadz', 20);
INSERT INTO Students(Name, Age) VALUES('Arpit', 21);
```

**Write:**
```sql
INSERT INTO Students(Name, Age)
VALUES 
('aadz', 20),
('Arpit', 21);
```

> [!TIP]
> **Advantages:**
> - Faster
> - Less network overhead
> - Fewer transactions

---

## 🕒 Timestamp Defaults

```sql
CREATE TABLE LoginHistory(
 ID INT AUTO_INCREMENT PRIMARY KEY,
 LoginTime TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO LoginHistory VALUES(NULL, DEFAULT);
```
`LoginTime` is automatically set to the current timestamp.

---

## 🔢 AUTO_INCREMENT Behavior

**Suppose:** `StudentID INT AUTO_INCREMENT`

**Then:**
```sql
INSERT INTO Students(Name) VALUES('aadz');
```
Result: `StudentID = 1`

**Next:**
```sql
INSERT INTO Students(Name) VALUES('Arpit');
```
Result: `StudentID = 2`

**Explicit Value:**
You can still do:
```sql
INSERT INTO Students VALUES (100, 'aadz');
```
The next generated value will continue after the highest existing value.

---

## 📥 INSERT FROM SELECT

Copies rows from another table.
**Suppose we have an `ArchiveStudents` table.**

```sql
INSERT INTO ArchiveStudents
SELECT * FROM Students;
```
*Every row is copied.*

**Copy only specific rows:**
```sql
INSERT INTO ArchiveStudents
SELECT * FROM Students WHERE CGPA > 9;
```
*Only high-CGPA students are archived.*

**Copy selected columns:**
```sql
INSERT INTO StudentBackup(Name, Email)
SELECT Name, Email FROM Students;
```

---

## 🛡️ INSERT IGNORE (MySQL)

**Suppose:** `Email UNIQUE`
**Existing:** `aadz@example.com`

**Now:**
```sql
INSERT INTO Students VALUES (..., 'aadz@example.com');
```
**Error:** `Duplicate entry`

**Instead use:**
```sql
INSERT IGNORE INTO Students VALUES (..., 'aadz@example.com');
```
> [!TIP]
> Duplicate rows are skipped with a warning instead of aborting the statement.

---

## ⚠️ Common INSERT Errors

1. **Column Count Doesn't Match**
2. **Wrong Datatype:** `VALUES('Twenty')` for `Age`.
3. **Duplicate Primary Key**
4. **NULL Violation:** `INSERT INTO Students(Age) VALUES(20);` (When `Name` is `NOT NULL`).
