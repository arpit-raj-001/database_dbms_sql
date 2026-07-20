<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🚀 SQL: Database Fundamentals</h1>
  <h2>Chapter 1: Why Database?</h2>
</div>

---

## 📁 File System vs 🗄️ DBMS

> [!NOTE]
> A **file system** (managed by OS) is responsible for creating/deleting files or reading/writing bytes and managing storage on disk.

Before databases, most software stored info directly in OS files. But the OS manages **files**, not the **data semantics**. 

Concepts like tables, relationships, keys, constraints, and transactions are implemented by a **DBMS**, not the file system.

> [!WARNING]
> Flat files like `.csv` and `.txt` can store data, but they **cannot** efficiently manage data at scale. 

---

## 🧠 What is a DBMS?

A **Database Management System (DBMS)** is software that provides an abstraction layer between the application and physical storage. It emphasizes *management*, not just storage.

### Key Features it Provides:

1. 💾 **Persistent Storage** 
   *Data survives after the program exits.*
2. ⚡ **Efficient Retrieval**
   *Instead of scanning every record, a DBMS uses indexes and access paths to retrieve data quickly.*
3. 🛡️ **Data Integrity**
   *Constraints ensure invalid data cannot be stored.*
4. 🔄 **Concurrency Control**
   *Thousands of users can read and modify data at the same time without corrupting it.*
5. 📦 **Transaction Management**
   *A group of related operations either all succeed or fail together.*
6. 🚑 **Crash Recovery**
   *If the system crashes after a transaction is committed, the DBMS can recover committed data and roll back incomplete work.*
7. 🔒 **Security**
   *Different users have access to different info based on permissions.*
8. 🧩 **Data Independence**
   *Interact with a logical view of data without worrying about how it's stored on disk.*

---

## 📈 Scaling & File-Based Systems

For simple sequential storage, flat files can be faster as they have less overhead. But as **data volume, complexity, and concurrency** increase, a DBMS performs better due to indexing, buffering, caching, and query optimization.

> [!WARNING]
> **File-Based Systems become difficult to maintain** as the app grows. Multiple programs have their own copies of data and implement their own logic, leading to:
> - ❌ Data redundancy
> - ❌ Data inconsistency
> - ❌ Lack of concurrency control
> - ❌ Poor scalability
> - ❌ Complex recovery mechanisms

> [!TIP]
> **DBMS gives ↓**
> Centralized organization of data, efficient querying, indexing, query optimization, data integrity via constraints, transaction management, concurrency control, crash recovery, security, data independence, and recovery mechanisms.
