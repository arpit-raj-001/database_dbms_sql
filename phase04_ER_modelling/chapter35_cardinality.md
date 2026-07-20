<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧮 Database Cardinality</h1>
  <h2>Chapter 35</h2>
</div>

---

## 🧐 What is Cardinality?

> [!NOTE]
> **Definition:** Cardinality specifies the maximum number of instances of one entity that can be associated with an instance of another entity through a relationship.

*Example:* One customer can place how many orders? One order belongs to how many customers?

### Cardinality helps determine:
- Foreign key placement
- Number of tables
- Whether a junction table is required
- Database constraints
- Business rules

---

## 🗂️ Types of Cardinality

```text
Cardinality
│
├── One-to-One (1:1)
├── One-to-Many (1:N)
├── Many-to-One (N:1)
└── Many-to-Many (M:N) 
```

### 1️⃣ One-to-One (1:1)

**Definition:** One instance of Entity A is related to at most one instance of Entity B, and vice versa.
*Example: Person and Passport.*

| Person | Passport |
| :--- | :--- |
| PersonID: 1, Name: aadz | PassportNo: P101, PersonID: 1 |

> [!TIP]
> **Implementation:** Usually Foreign Key + UNIQUE constraint.

### 2️⃣ One-to-Many (1:1)
*(Note: Typo in original material, this is 1:N)*

**Definition:** One instance of Entity A can relate to many instances of Entity B. Each Entity B instance relates to only one Entity A.

| Department | Employee |
| :--- | :--- |
| DeptID: 10, Name: CS | EmpID: 1, Name: arpit, DeptID: 10 |
| | EmpID: 2, Name: aadz, DeptID: 10 |

> [!TIP]
> **Implementation:** Foreign key goes on the many side.

### 3️⃣ Many-to-One (N:1)

This is simply the reverse view of One-to-Many. 
*Example:* Many Employees → One Department. Same relationship, different viewpoint.

### 4️⃣ Many-to-Many (M:N)

**Definition:** Many instances of Entity A can relate to many instances of Entity B.
**Diagram:** `Student M -------- N Course`

**Implementation:**
| Student | Course | Enrollment (Junction Table) |
| :--- | :--- | :--- |

| StudentID: 101, Name: DBMS | CourseID: 101, Name: OS | StudentID: 101, CourseID: 101 |
| | | StudentID: 102, CourseID: 101 |

> [!WARNING]
> **Enrollment contains:** FK → Student, FK → Course. Primary Key: `(StudentID, CourseID)`.

---

## ❓ Why are M:N relationships special?

In an M:N relationship, each entity can be associated with multiple instances of the other entity. A single foreign key cannot represent multiple values without violating normalization. The correct solution is to introduce a **junction table** such as `Enrollment`, which stores pairs of keys.
