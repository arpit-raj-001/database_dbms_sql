<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗄️ Database Basics & Management</h1>
  <h2>Chapter 56</h2>
</div>

---

A database is actually a logical container that organizes and stores related objects.

**These objects include:**
- Tables
- Views
- Indexes
- Stored Procedures
- Functions
- Triggers
- Events
- Constraints

**Reasons for multiple databases:**
- Better organization
- Security
- Independent backups
- Easier maintenance
- Scalability

---

## ❓ Where Does MySQL Store a Database?
*This is a favorite interview question.*

When you execute:
```sql
CREATE DATABASE College;
```
MySQL doesn't simply remember the name. Internally it creates a directory on disk.
For example (Windows):
```text
C:\ProgramData 
  MySQL 
    MySQL Server 8.0 
      Data 
        College\
```
Inside that directory, MySQL stores metadata and the files corresponding to the database objects.
*(With modern MySQL versions using the InnoDB storage engine, table data is managed differently than older versions, but conceptually the database has its own directory).*

---

## 🛠️ CREATE DATABASE

The simplest syntax is:
```sql
CREATE DATABASE College;
```
After execution, MySQL creates `College` and the database becomes available.

### Internal Working
MySQL performs several checks.
- **Step 1:** Checks whether `College` already exists.
- **Step 2:** Checks whether the current user has permission.
- **Step 3:** Allocates metadata.
- **Step 4:** Creates the database directory and initializes the metadata needed to manage objects within it.
- **Returns:** Query OK

> [!TIP]
> To avoid an error if the database already exists, use:
> ```sql
> CREATE DATABASE IF NOT EXISTS College;
> ```
> Now MySQL behaves like this:
> - If `College` doesn't exist → Create it.
> - If it already exists → Do nothing. No error.

---

### Is `College` the same as `college`?
**Answer:** It depends.
- On Windows, usually **yes**.
- On Linux, linux filesystems are often **case-sensitive**.

---

## 🎯 Selecting a Database

Creating a database doesn't automatically mean your subsequent commands will use it. You must select it.

**Syntax:**
```sql
USE College;
```
Now every table you create goes inside `College`:
```sql
CREATE DATABASE College;

USE College;

CREATE TABLE Student(
 ID INT,
 Name VARCHAR(100)
);
```

---

## 🗑️ DROP DATABASE

Deletes an entire database.

**Syntax:**
```sql
DROP DATABASE College;
```

**This removes:** Tables, Views, Indexes, Stored Procedures, Functions, Triggers, Data. Everything inside the database is removed.

### Internal Working (MySQL)
- Verifies permissions.
- Ensures no conflicting operations prevent the drop.
- Removes metadata.
- Deletes the database directory and associated files.

> [!WARNING]
> Afterward, recovery is generally **only** possible from backups.

**Safe drop:**
```sql
DROP DATABASE IF EXISTS College;
```
If the database doesn't exist, MySQL simply skips the operation without throwing an error.
