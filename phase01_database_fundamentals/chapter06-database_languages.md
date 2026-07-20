AUTHORED BY : ARPIT RAJ , LNMIIT JAIPUR

# Ch 6 Database Languages
**(DDL, DML, DQL, DCL, TCL)**

In a database, we perform various operations sometimes,
• create table and index
• insert data
• grant permission
• rollback failed transaction

These are different operations and instead of treating them all as one type of command, SQL classifies them into logical groups called database languages.

Different type of operation → different categories → database languages

**SQL**
↓
DDL | DML | DCL | TCL | DQL

---

## Logical Classifications

### 1) DDL → data definition language
define and modify the schema of database objects (database, table, index, schema, view)

**Eg: CREATE, ALTER, DROP, TRUNCATE, RENAME**

> **Internally:**
> • the system catalogue (metadata) is updated
> • storage structures are modified
> • constraints are registered
> 
> *every DDL operation modifies system catalog as schema is changed*

### 2) DML → data manipulation language
• DDL managed the structure
• DML manages the data inside that structure

**Eg: INSERT, UPDATE, DELETE, MERGE** (combine insert/update logic)

> **Internally:**
> • find free space
> • writes to new row
> • updates indexes
> • writes to transaction log
> • checks constraints

### 3) DQL → data query language
→ retrieve data without modifying it

**Eg: SELECT**

> **Internally:**
> parses SQL → optimizes query → reads data → return result

### 4) DCL → data control language
→ manages authorization and access controls

**Eg: GRANT, REVOKE**

### 5) TCL → transaction control language

**Eg: COMMIT, ROLLBACK, SAVEPOINT, SET TRANSACTION**

> *DDL, DML, DQL, DCL, TCL are logical classifications of SQL statements.*

---

## Practice Questions

**Q1: Differentiate between DDL and DML.**
A1: DDL (Data Definition Language) defines or modifies the database schema by creating, altering, renaming, or dropping database objects. It primarily affects metadata.
DML (Data Manipulation Language) inserts, updates, deletes, or merges rows within existing database objects. It primarily affects user data while leaving the schema unchanged.

**Q2: Why does CREATE TABLE modify metadata but INSERT does not?**
A2: CREATE TABLE introduces a new database object, requiring the DBMS to update the system catalog with metadata such as the table definition, columns, data types, and constraints.
INSERT adds rows to an existing table. The schema remains unchanged, so metadata generally does not change; only user data and associated indexes/logs are updated.

**Q3: Explain the purpose of DCL and TCL with examples.**
A3: DCL (Data Control Language) manages permissions and security through commands such as GRANT and REVOKE.
TCL (Transaction Control Language) manages transaction boundaries using commands such as COMMIT, ROLLBACK, and SAVEPOINT, ensuring ACID properties during data modifications.

**Q4: Is SELECT part of DML or DQL? How would you answer this in an interview?**
A4: There are two accepted conventions:
• Many educational resources classify SELECT as DQL because it retrieves data without modifying it.
• Some SQL standards and DBMS documentation include SELECT under DML because it operates on stored data.

In interviews, acknowledge both conventions and explain that the classification depends on the reference being used.

**Q5: Which DBMS components are primarily involved when executing: CREATE TABLE, INSERT, SELECT, GRANT, COMMIT**
A5:
• CREATE TABLE → Query Processor, Catalog Manager, Storage Manager
• INSERT → Query Processor, Storage Manager, Transaction Manager
• SELECT → Query Processor, Query Optimizer, Buffer Manager
• GRANT → Authorization Manager
• COMMIT → Transaction Manager, Recovery Manager

**Q6: A user executes:**
```sql
CREATE TABLE Employees (...)
INSERT INTO Employees VALUES (...)
GRANT SELECT ON Employees TO analyst
COMMIT
```
**Classify each statement and describe its primary effect on the database.**

A6:
• **CREATE TABLE Employees (...)** — DDL. Creates a new table and updates the system catalog (metadata).
• **INSERT INTO Employees VALUES (...)** — DML. Adds a new row to the table and updates indexes and transaction logs.
• **GRANT SELECT ON Employees TO analyst** — DCL. Grants read permission on the table to the specified user or role.
• **COMMIT** — TCL. Permanently commits all changes made in the current transaction.
