<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔑 Database Keys</h1>
  <h2>Chapter 20</h2>
</div>

---

## 🎯 Three Fundamental Properties of Keys

Every key is built around three ideas:

```mermaid
graph TD
    A[Good Key] --> B[Uniqueness]
    A --> C[Identification]
    A --> D[Minimality]
```

### 1️⃣ Uniqueness
A key must uniquely identify every tuple.

**Example:**
| StudentID | Name |
| :--- | :--- |
| `101` | `Aadz` |
| `102` | `Arpit` |
| `103` | `Aadz` |

- `StudentID` is unique.
- `Name` is not.

> [!WARNING]
> Without uniqueness, you cannot reliably identify rows.

### 2️⃣ Identification
A key answers: *"Which exact row are we talking about?"*

> **Imagine a bank.**
> Two customers may have:
> - Same name
> - Same city
> - Same age
> 
> But they will never have the same **Account Number**. The account number identifies the customer.

### 3️⃣ Minimality
This is one of the most misunderstood concepts.
A key should contain **only** the attributes necessary to uniquely identify a tuple.

**Example:** `| StudentID | Name |`

Suppose `StudentID` alone is unique. Then `(StudentID)` is a key.

What about `(StudentID, Name)`? Is it also unique?
**Yes.**
But `Name` is unnecessary. Therefore, `(StudentID, Name)` is **not minimal**.

---

## 🗂️ Types of Keys

### 1. Super Key

**Definition:**
A Super Key is any set of one or more attributes that uniquely identifies each tuple in a relation.

**Example Students:**
| StudentID | Email | Name |
| :--- | :--- | :--- |
| `101` | `a@gmail.com` | `Aadz` |
| `102` | `b@gmail.com` | `Arpit` |

**Possible Super Keys:**
- `StudentID`
- `Email`
- `(StudentID, Name)`
- `(StudentID, Email)`
- `(Email, Name)`
- `(StudentID, Email, Name)`

*All of these uniquely identify a row.*

### 2. Candidate Key

**Definition:**
A Candidate Key is a **minimal Super Key**.
Minimal means: Remove any attribute, and uniqueness disappears.

**Possible Super Keys:**
- `StudentID` ✅
- `Email` ✅
- `(StudentID, Name)` ❌ *(Not minimal)*
- `(Email, Name)` ❌ *(Not minimal)*
- `(StudentID, Email)` ❌ *(Not minimal)*

**Candidate Keys:**
- `StudentID`
- `Email`

> [!NOTE]
> Because both are Unique and Minimal, every Candidate Key is a Super Key.
> But **not** every Super Key is a Candidate Key. The candidate key is a subset of super keys.

### 3. Primary Key

**Definition:**
A Primary Key is the Candidate Key chosen by the database designer to uniquely identify tuples.

**Characteristics of a Primary Key:**
- Must be unique (only one per table)
- Cannot be NULL
- Must remain stable
- Should be minimal

| Candidate Key | Primary Key |
| :--- | :--- |
| Potential identifier | **Chosen identifier.** |

### 4. Alternate Key

**Definition:**
Alternate Keys are Candidate Keys that were **not** selected as the Primary Key.

### 5. Composite Key

**Definition:**
A Composite Key consists of two or more attributes used together to uniquely identify a tuple.

**Example (Course Enrollment):**
| StudentID | CourseID | Grade |
| :--- | :--- | :--- |
| `101` | `CS101` | `A` |
| `101` | `CS102` | `B` |
| `102` | `CS101` | `A` |

Neither `StudentID` nor `CourseID` alone is unique.
**Together:** `(StudentID, CourseID)` is unique.

**Can a Primary Key Be Composite?** Yes.

### 6. Natural Key

**Definition:**
A Natural Key is a key that already exists in the real world.
**Examples:** Passport Number, Aadhaar Number.

- ✅ **Advantages:** Meaningful, No extra column needed.
- ❌ **Disadvantages:** May change, Business rules can change, Privacy concerns.

### 7. Surrogate Key

**Definition:**
A Surrogate Key is an artificial key created solely for database identification.
**Example:** `StudentID = 101`

The number has no business meaning. It exists only to identify rows.
Usually implemented using: `AUTO_INCREMENT`, `IDENTITY`, `SERIAL`, `UUID`

- ✅ **Advantages:** Stable, Never changes, Small, Efficient for joins and indexes.

### 8. Foreign Key (Basic Introduction)

**Definition:**
A Foreign Key is an attribute in one table that references the Primary Key of another table.

**Example:**
`Orders.StudentID` references `Students.StudentID`.
This establishes a relationship between the tables. Foreign keys also help enforce referential integrity, ensuring references point to valid rows.

---

## ⚖️ Comparisons & Concepts

### Super Key vs Candidate Key
A **Super Key** uniquely identifies tuples but may contain unnecessary attributes.
A **Candidate Key** is a minimal Super Key with no redundant attributes.

> [!WARNING]
> **Can a Candidate Key Contain NULL?**
> **Interview Answer:** No, in the relational model a Candidate Key cannot contain NULL because it must uniquely identify every tuple.
> 
> *A Candidate Key must uniquely identify every tuple, so it cannot contain NULL under relational theory. In SQL, remember that UNIQUE constraints alone may permit NULL depending on the DBMS, so a true candidate key should effectively be UNIQUE and NOT NULL.*
