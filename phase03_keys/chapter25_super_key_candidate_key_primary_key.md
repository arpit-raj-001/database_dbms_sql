<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗝️ Super Key, Candidate Key, Primary Key</h1>
  <h2>Chapter 25</h2>
</div>

---

## 🦸‍♂️ Super Key

> [!NOTE]
> **Definition:**
> A Super Key is a set of one or more attributes (columns) that can uniquely identify every row in a table.
> 
> *In simple words, any combination of columns that guarantees uniqueness is called a Super Key.*

### Characteristics of a Super Key:
- ✅ **UNIQUENESS**
- ✅ **NO DUPLICATE VALUES**
- ✅ **CAN CONTAIN EXTRA ATTRIBUTES**
- ✅ **CAN HAVE MANY SUPER KEYS**

---

## 🌟 Candidate Key

> [!NOTE]
> **Definition:**
> A Candidate Key is a **minimal Super Key**.
> 
> *In other words, a Candidate Key is the smallest set of attributes that can uniquely identify every row in a table.*

It has two important properties:
1. It uniquely identifies every row.
2. It contains **no unnecessary attributes**.

### Characteristics of a Candidate Key:
- ✅ **UNIQUENESS**
- ✅ **MINIMALITY**
- ✅ **NO REDUNDANT ATTRIBUTES**
- ✅ **MULTIPLE** (A table can have more than one)

> [!TIP]
> **Why is minimality important?**
> Minimality ensures that a key contains only the attributes required for uniqueness. Removing redundant attributes reduces storage overhead, simplifies relationships, and results in cleaner database design.

---

## 👑 Primary Key

> [!NOTE]
> **Definition:**
> A Primary Key (PK) is the Candidate Key chosen to uniquely identify every row in a table.
> 
> *Every table should have exactly one Primary Key.*

### Every Primary Key must be:
- 📌 **UNIQUE**
- 📌 **NOT NULL**
*(Both are mandatory).*

A Primary Key can never contain `NULL` because every row must have a valid and unique identifier. This is required by the **Entity Integrity** rule.

> [!WARNING]
> Changing a Primary Key is strongly discouraged because it can require updating all related foreign keys and may affect indexes and application logic. Therefore, Primary Keys should be chosen to be **stable**.

A table may have multiple Candidate Keys, but having **one** designated Primary Key provides a single, consistent identifier for relationships, indexing, and database design. The remaining Candidate Keys become **Alternate Keys**.
