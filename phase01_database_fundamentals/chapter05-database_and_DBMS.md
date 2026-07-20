AUTHORED BY : ARPIT RAJ , LNMIIT JAIPUR

# Ch 5 Database and DBMS

## Database

**Database:** an organized, logically related collection of data designed to support storage, retrieval, modification and management of info for one or more application

**ORGANIZED , LOGICALLY RELATED , PERSISTENT , SHARED , MANAGED**

> **database :- user data + metadata**
> ↪ supports storage, retrieval, modification, management of info

### Characteristic of database
• **integrated :** different piece of info interconnected
• **consistent :** stored data obey rules
• **persistent :** data survives even after termination
• **shared :** concurrent access
• **secure :** Authorization
• **scalable :** doesnt matter how much data volume increased
• **minimal redundancy :** via good schema design

---

## DBMS Overview

**a database is simply data, it does not**
• execute SQL
• optimize queries
• handle transaction
• manage locks
• recover crash

**} responsibilities of DBMS**

| database | DBMS |
| :--- | :--- |
| collection of related data | software that manages database |
| passive | active |
| stores info | performs operation on info |
| contain user data | provides querying, transaction, security and recovery |

**DBMS:**
a software that provides an interface b/w applications and databases, enables efficient storage, retrieval, modification, security, transaction process, concurrency, and recovery

App → SQL queries → **DBMS** → *abstraction layer* → OS → (SSD / HDD)

---

## Responsibilities of DBMS
*(We will discuss in detail in later chapters)*

**(A) data storage**
• organize records
• manage pages
• allocate storage
• free unused space

**(B) Query process**
• parse SQL
• validate syntax
• check schema
• create execution plan
• execute query

**(C) Query Optimization**
• estimate cost of each execution plan and execute the one with lower cost

**(D) Transaction management**
• guarantees ACID properties

**(E) Concurrency control**
• prevents lost updates
• prevents dirty reads
• prevent phantom reads

**(F) recovery management**
• write ahead logging (WAL)
• checkpoints
• recovery algorithms

**(G) security management**
• authentication
• authorization

**(H) integrity enforcement**
• constraint satisfaction
• unique primary key
• foreign key reference valid row

---

## DBMS Architecture Modules

**DBMS is not one large program , but a collection of multiple specialized modules**

| Module | Mapped Responsibility Point |
| :--- | :--- |
| Query processor | (B, C) points |
| Storage manager | (A) |
| Transaction manager | (D) |
| Buffer manager | - |
| recovery manager | (F) |
| Authorization manager | (G) |

### Buffer manager
↪ accessing disk is slow, so DBMS keeps frequently accessed pages in memory

**It decides:**
• which pages to load
• which to evict
• when modified pages should be written back

> whenever SQL is executed, DBMS consults the system catalogue where the metadata is stored

### Life of A SQL query
receive SQL → Parse → validate → optimize → choose execution plan → request pages → read disk → return results

---

## Practice Questions

**Q1: Why is a DBMS considered an abstraction layer between applications and storage?**
A1: A DBMS abstracts the complexity of physical storage by exposing a logical interface (SQL and schemas) to applications. Applications interact with tables and queries rather than files, pages, or disk blocks. The DBMS translates high-level operations into low-level storage operations, providing data independence.

**Q2: Differentiate between a database and a DBMS with examples.**
A2: A database is the persistent collection of related user data and metadata (e.g., CollegeDB containing Students and Courses tables). A DBMS is the software (e.g., MySQL or PostgreSQL) that manages the database by executing queries, enforcing constraints, handling transactions, controlling concurrency, and recovering from failures.

**Q3: List the primary responsibilities of a DBMS.**
A3: The primary responsibilities of a DBMS include:
• Data storage management
• Query processing
• Query optimization
• Transaction management
• Concurrency control
• Recovery management
• Security and authorization
• Integrity constraint enforcement
• Metadata management

**Q4: What is the role of the Query Processor?**
A4: The Query Processor parses SQL statements, validates their syntax and semantics, consults the system catalog, generates a logical execution plan, optimizes that plan, and coordinates its execution to produce the requested results.

**Q5: Why is a Buffer Manager necessary?**
A5: Disk access is several orders of magnitude slower than RAM access. The Buffer Manager caches frequently used disk pages in memory, reducing disk I/O and improving query performance. It also decides when modified pages should be flushed back to disk.

**Q6: Explain the purpose of the Transaction Manager and the Recovery Manager.**
A6: The Transaction Manager ensures that groups of operations satisfy the ACID properties by coordinating commits and rollbacks. The Recovery Manager restores the database to a consistent state after failures using logs and recovery algorithms, ensuring durability and consistency.

**Q7: What information is stored in the System Catalog (Data Dictionary)?**
A7: The System Catalog stores metadata, including schemas, table definitions, column definitions, data types, indexes, constraints, views, user accounts, privileges, and optimizer statistics. The DBMS relies on this information to interpret and execute queries correctly.

**Q8: Describe the journey of a SQL query from the moment it is submitted until the results are returned.**
A8: A SQL query passes through several stages:
1. The DBMS receives the SQL statement.
2. The Query Processor parses and validates it.
3. The Optimizer generates and evaluates execution plans.
4. The chosen plan is executed.
5. The Storage and Buffer Managers retrieve the necessary data pages.
6. The DBMS applies constraints and transaction rules as needed.
7. The requested results are returned to the application.
