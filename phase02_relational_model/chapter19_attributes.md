<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🏷️ What is an Attribute?</h1>
  <h2>Chapter 19</h2>
</div>

---

### Definition

> [!NOTE]
> An **attribute** is a property or characteristic of an entity. In a relational database, attributes are represented as columns in a table.

Each attribute has a name and data type. The **data type** tells the DBMS:
- How much memory to allocate
- What operations are valid
- How the value should be stored

Then the attribute has **constraints**. Constraints enforce business rules.
**Common constraints:**
- `NOT NULL`
- `UNIQUE`
- `CHECK`
- `DEFAULT`
- `PRIMARY KEY`
- `FOREIGN KEY`

---

## 1️⃣ Simple Attribute

**Definition:**
An attribute that cannot be meaningfully divided into smaller subparts.

## 2️⃣ Composite Attribute

**Definition:**
An attribute that can be logically divided into smaller attributes.

> **Example:**
> `Name : aadz raz -> aadz + raz (fname + lname)`

## 3️⃣ Single-Valued Attribute

**Definition:**
An attribute that contains exactly one value for each entity.

## 4️⃣ Multi-Valued Attribute

**Definition:**
An attribute that may contain multiple values for a single entity.

> **Example:**
> `Phone Numbers`

> [!WARNING]
> **Problems with this multi-valued attribute:**
> - Difficult to search
> - Difficult to index
> - Violates atomicity
> - Violates First Normal Form (1NF)
> - Hard to update

**Hence BAD: Suppose we store:**
| Student | Phones |
| :--- | :--- |
| `Aadz` | `77276, 79870` |

### ✅ Correct Design Instead:

**Students Table:**
| StudentID | Name |
| :--- | :--- |
| `101` | `Aadz` |

**Phones Table:**
| StudentID | Phone |
| :--- | :--- |
| `101` | `9876543210` |
| `101` | `9123456789` |

---

## 5️⃣ Stored Attribute

**Definition:**
An attribute whose value is physically stored in the database.

## 6️⃣ Derived Attribute

**Definition:**
An attribute whose value is computed from other stored attributes.

## 7️⃣ Mandatory Attribute

*(Also called `NOT NULL` attribute)*

**Definition:**
Every tuple **must** contain a value.

Mandatory attributes usually include:
- `Primary Key`
