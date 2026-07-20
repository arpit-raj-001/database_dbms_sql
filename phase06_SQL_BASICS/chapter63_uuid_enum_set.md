<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🆔 UUID, ENUM, AND SET</h1>
  <h2>Chapter 63</h2>
</div>

---

## 🧐 What is UUID?

UUID stands for **Universally Unique Identifier**.
It is a 128-bit value designed so that the probability of two independently generated UUIDs being identical is extremely low.

Unlike `AUTO_INCREMENT` (`1, 2, 3, 4, 5`), a UUID looks like:
`550e8400-e29b-41d4-a716-446655440000`

---

## ❓ Why Do We Need UUID?

Suppose Amazon has servers in:
- Mumbai
- Delhi
- Bengaluru
- Chennai

Each server creates orders independently.
If every server uses `AUTO_INCREMENT`, then Mumbai generates `1, 2, 3` and Delhi generates `1, 2, 3`.
**Result:** Both generate Order ID 1. **Collision.**

**With UUID:**
- Mumbai: `550e8400-e29b-41d4-a716-446655440000`
- Delhi: `9d4a2b60-80b3-4a73-a5d1-52d4d63ef001`
**Result:** No collision.

---

## 🏗️ UUID Structure

**Example:** `550e8400-e29b-41d4-a716-446655440000`
Broken into groups: `8-4-4-4-12`
- 32 hexadecimal digits
- or 128 bits

### Storage
There are two common ways to store UUIDs:

**As CHAR:**
```sql
UserID CHAR(36)
```
- **Storage:** 36 characters (including hyphens).

**As BINARY:**
```sql
UserID BINARY(16)
```
- **Storage:** 16 bytes.
- *Much smaller. Much faster.*

> [!TIP]
> Many production systems store UUIDs in binary form and convert them to text only when needed.

---

### Insert Example
```sql
CREATE TABLE Users(
 UserID CHAR(36),
 Name VARCHAR(100)
);

INSERT INTO Users VALUES (UUID(), 'aadz');
```

---

## 🔢 UUID Versions

There are multiple versions.

### Version 1
Based on: timestamp, machine identifier, clock sequence.
- **Advantages:** Time ordered.
- **Disadvantages:** Can reveal machine/time information.

### Version 3
Generated using: namespace, MD5 hash.
- **Advantages:** Deterministic.

### Version 4
Generated using random numbers. *(Most common)*.
- **Example:** `9f4fd9d2-6f4e-4d49-b71d-8c34f4c7d2ab`
- *Almost every backend project uses Version 4 unless ordering is important.*

### Version 5
Uses SHA-1 hashing. Deterministic.

---

## ⚡ UUID Indexing Impact

UUID values are completely random (e.g. `9f...`, `2b...`, `8d...`, `a1...`).
This means they get inserted into random positions in the index.

**This causes:**
- page splits,
- index fragmentation,
- increased I/O.
> [!WARNING]
> This is one of the biggest disadvantages of random UUIDs (especially Version 4).

### Security Benefit
If a hacker sees `/users/5`, they can guess `/users/6`, `/users/7`, `/users/8`. This is called **ID enumeration**.
If they see `/users/550e8400...`, it's impossible to guess by simple incrementing. Much better for public-facing APIs.

---

## 🔽 What is ENUM?

**ENUM** stands for Enumeration.
It allows a column to store only one value from a predefined list. Think of it as a drop-down menu.

**Syntax:**
```sql
CREATE TABLE Student(
 StudentID INT PRIMARY KEY,
 Name VARCHAR(100),
 Gender ENUM('Male','Female','Other')
);
```

**Insert:**
```sql
INSERT INTO Student VALUES (1, 'aadz', 'Female');
```

**Internally:**
MySQL stores:
- `Male → 1`
- `Female → 2`
- `Other → 3`

The database stores the numeric index, not the full text, while displaying the corresponding string. Only **1 byte** is needed. Very efficient!

### Number of ENUM Values
| Number of Values | Storage |
| :--- | :--- |
| Up to 255 | 1 byte |
| 256–65,535 | 2 bytes |

---

## 🗃️ What is SET?

**SET** is another MySQL-specific datatype. Unlike ENUM, a row can contain **multiple values** from the predefined list.

**Example:**
```sql
CREATE TABLE Employee(
 Skills SET('C++', 'Python', 'SQL', 'Java')
);

INSERT INTO Employee VALUES ('C++,SQL');
```

**Internal Representation of SET:**
Internally it uses bitmasks.
- `C++` -> `0001`
- `Python` -> `0010`
- `SQL` -> `0100`
- `Java` -> `1000`

---

## ⚖️ Advantages and Disadvantages

### ENUM
- **Advantages:** Prevents invalid values. Compact storage. Easy to understand. Fast comparisons. Simple schema for fixed choices.
- **Disadvantages:** Adding values requires a schema change. MySQL-specific (less portable to other databases).

### SET
- **Advantages:** Stores multiple selections. Efficient bitmask storage. Convenient for a small, fixed set of options.
- **Disadvantages:** MySQL-specific. Harder to query and maintain than a normalized many-to-many relationship.
