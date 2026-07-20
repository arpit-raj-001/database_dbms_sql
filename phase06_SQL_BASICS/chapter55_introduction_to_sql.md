<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗣️ Structured Query Language (SQL)</h1>
  <h2>Chapter 55</h2>
</div>

---

**SQL** stands for Structured Query Language.

It is the standard language used to communicate with relational databases. SQL is a **declarative language**, not a general-purpose programming language. 

You tell the database:
- **WHAT** you want.
- **Not HOW** to get it.

---

## 🛠️ What Can SQL Do?

**SQL can:**
- Create databases / Delete databases
- Create tables / Delete tables
- Insert records / Update records / Delete records / Search records
- Join tables
- Create indexes / views / stored procedures
- Manage users
- Control permissions
- Manage transactions

---

## ⚡ Basic SQL Operations

### Create database
```sql
CREATE DATABASE College;
```

### Create table
```sql
CREATE TABLE Student(
 StudentID INT,
 Name VARCHAR(100)
);
```

### Insert
```sql
INSERT INTO Student
VALUES (1, 'aadz');
```

### Read
```sql
SELECT *
FROM Student;
```

### Update
```sql
UPDATE Student
SET Name='Arpit'
WHERE StudentID=1;
```

### Delete
```sql
DELETE FROM Student
WHERE StudentID=1;
```

---

## 🆚 SQL vs DBMS

| SQL | DBMS |
| :--- | :--- |
| A language | Software |
| Used to communicate with a database | Manages the database |

---

## 🗣️ What is a SQL Dialect?

> [!NOTE]
> A dialect is a vendor-specific variation of standard SQL. The core language is shared, but each DBMS adds its own features and syntax.

### Major SQL Dialects

| Dialect | Characteristics |
| :--- | :--- |
| **MySQL** | • Open source<br>• Owned by Oracle<br>• Very popular for web applications<br>• Fast and beginner-friendly<br>• Used in many startups and educational environments |
| **PostgreSQL**| • Open source<br>• Known for standards compliance<br>• Rich feature set<br>• Excellent for complex queries and advanced data types<br>• Popular in enterprise systems |
| **SQL Server**| • Developed by Microsoft<br>• Uses the T-SQL dialect<br>• Common in enterprises using the Microsoft ecosystem |
| **Oracle** | • Developed by Oracle<br>• Uses PL/SQL extensions<br>• Very common in banking, telecom, and large enterprises |
| **SQLite** | • Lightweight<br>• Serverless<br>• Entire database is stored in a single file<br>• Common in mobile apps, embedded devices, and local applications |

### Example of Dialect Differences
*Get the first five rows.*

**MySQL / PostgreSQL:**
```sql
SELECT *
FROM Student
LIMIT 5;
```

**SQL Server:**
```sql
SELECT TOP 5 *
FROM Student;
```

---

## 🗂️ SQL Command Categories

Although SQL is one language, its commands are commonly grouped by purpose.

### 1. DDL — Data Definition Language
Defines or changes the database structure.
**Common commands:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`

### 2. DML — Data Manipulation Language
Works with the data stored in tables.
**Common commands:** `INSERT`, `UPDATE`, `DELETE`

### 3. DQL — Data Query Language
Retrieves data.
**Main command:** `SELECT`

### 4. DCL — Data Control Language
Controls permissions and access.
**Common commands:** `GRANT`, `REVOKE`

### 5. TCL — Transaction Control Language
Manages transactions.
**Common commands:** `COMMIT`, `ROLLBACK`, `SAVEPOINT`

### Summary Table
| Category | Purpose | Common Commands |
| :--- | :--- | :--- |
| **DDL** | Define database objects | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **DML** | Modify data | `INSERT`, `UPDATE`, `DELETE` |
| **DQL** | Retrieve data | `SELECT` |
| **DCL** | Control permissions | `GRANT`, `REVOKE` |
| **TCL** | Manage transactions | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |
