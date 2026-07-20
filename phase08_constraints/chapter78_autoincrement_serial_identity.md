<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔢 DATABASE AUTO-INCREMENT NOTES</h1>
  <h2>Chapter 78</h2>
</div>

---

## 🎯 Specify Starting Value

```sql
ALTER TABLE Students
AUTO_INCREMENT = 1000;
```
**Next insert:** `1000`

---

## 🐘 SERIAL (PostgreSQL)

PostgreSQL historically uses `SERIAL`.

**Example:**
```sql
CREATE TABLE Students(
 StudentID SERIAL PRIMARY KEY,
 Name TEXT
);
```

Looks like a datatype. Actually it expands into:
`Integer + Sequence + Default NEXTVAL()`

**Internally it creates:**
- an integer column,
- a sequence object,
- a default that fetches the next sequence value.

### BIGSERIAL
Same idea. But uses `BIGINT` instead of `INT`.

---

## 🪟 IDENTITY (SQL Server)

SQL Server uses `IDENTITY`.

**Example:**
```sql
CREATE TABLE Students(
 StudentID INT IDENTITY(1,1) PRIMARY KEY,
 Name VARCHAR(100)
);
```
**Meaning:**
- Start = 1
- Increment = 1

---

## 🐬 MySQL

MySQL's `AUTO_INCREMENT` is managed internally by the storage engine (such as InnoDB). You don't interact with a separate sequence object the way you do in PostgreSQL.

### MySQL LAST_INSERT_ID()
```sql
INSERT INTO Students(Name) VALUES ('aadz');
SELECT LAST_INSERT_ID();
```
**Returns:** `1`

> [!TIP]
> It is connection/session-specific, making it safe even when many users are inserting simultaneously.

---

## 🔄 Resetting Counters

**Current:** `100`
**Need:** `1`

**MySQL:**
```sql
ALTER TABLE Students
AUTO_INCREMENT = 1;
```

> [!WARNING]
> **Important:**
> If rows already exist with IDs greater than or equal to the requested value, MySQL will not reuse those IDs. The next generated value will still be greater than the current maximum.

---

## ❓ Can AUTO_INCREMENT guarantee consecutive IDs?

**Answer:** No.
It guarantees unique IDs, not gap-free IDs.

**Rollback, DeleteRows:**
1. IDs: `1 2 3`
2. Delete `2`
3. Now: `1 3`
4. Next insert: `4` (Not `2`)
