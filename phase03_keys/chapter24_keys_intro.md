<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔑 Why Keys are Needed</h1>
  <h2>Chapter 24</h2>
</div>

---

Keys solve many fundamental database problems.

## 👥 Problem 1 — Duplicate Records

| Without keys | With a key |
| :--- | :--- |
| The database cannot differentiate between rows. | Now every student is uniquely identifiable. |

---

## ⚡ Problem 2 — Fast Searching

| Without a key | With a key |
| :--- | :--- |
| Finding one student requires checking every row. | The DBMS can use indexes for extremely fast lookups. |

---

## 📝 Problem 3 — Updating Records

Suppose you want to update Rahul's phone number.

**Without a key:**
```sql
UPDATE Student
SET Phone='9999999999'
WHERE Name='Aadz';
```
> [!WARNING]
> **Oops.** Maybe 50 Aadz's get updated!

**With a key:**
```sql
UPDATE Student
SET Phone='9999999999'
WHERE RollNo=006;
```
> [!TIP]
> **Only one row changes.**

---

## 🗑️ Problem 4 — Deleting Records

**Without keys:**
```sql
DELETE FROM Student
WHERE Name='Aadz';
```
> [!WARNING]
> **Danger.** Multiple students may disappear!

**With keys:**
```sql
DELETE FROM Student
WHERE RollNo=006;
```
> [!TIP]
> **Exactly one row gets deleted.**

---

> [!IMPORTANT]
> **Without keys, relationships become impossible.**

---

## 💎 Uniqueness

- No two rows can have the same key value.
- Every table must have a primary key, and its value can never be `NULL`.

**Why?**
Because every row must be identifiable.

> [!NOTE]
> **Primary Key = Unique + NOT NULL**
> Both these conditions are required for **entity integrity**.

Without Entity Integrity:
- ❌ Duplicate identities
- ❌ Missing identities
- ❌ Impossible updates
- ❌ Impossible deletions

---

## 🔗 6. Referential Integrity

This integrity rule deals with relationships between tables.

**Suppose:**

**Student Table:**
| RollNo | Name |
| :--- | :--- |
| `101` | `arpit` |
| `102` | `aadz` |

**Fees Table:**
| Receipt | RollNo |
| :--- | :--- |
| `1` | `101` |
| `2` | `105` |

**The Problem:**
Student `105` doesn't exist! The Fees table now references a nonexistent student. This is called an **orphan record**.

> [!IMPORTANT]
> **Referential Integrity says:**
> Every foreign key value must either:
> 1. Match an existing primary key
> **OR**
> 2. Be `NULL` (if allowed).

---

## 🛠️ Normalization

Normalization removes redundancy and improves database design. Keys are **essential** for normalization.

Without keys, the DBMS cannot determine:
- Uniqueness
- Dependencies
- Relationships
- Decomposition rules.

### Keys are used for:
- ✅ Identification
- ✅ Relationships
- ✅ Constraints
- ✅ Indexing
- ✅ Normalization
- ✅ Data integrity & Referential integrity
- ✅ Safely executing Updates
- ✅ Safely executing Deletes
