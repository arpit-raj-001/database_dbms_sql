<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔗 Composite Keys</h1>
  <h2>Chapter 27</h2>
</div>

---

## 📖 Definition

> [!NOTE]
> A **Composite Key** is a key formed by two or more columns together that uniquely identify a row in a table.

None of the individual columns can uniquely identify the row on their own, but their **combination** can.

```mermaid
graph TD
    A[StudentID] --> C(Together)
    B[CourseID] --> C
    C --> D[Unique Row]
```

Neither column is sufficient alone to represent an enrollment. This is extremely common in **Many-to-Many Relationships**.

### Students ↔ Courses
- One student can take many courses.
- One course can have many students.

**Enrollment table**
| StudentID | CourseID |
| :--- | :--- |
| `101` | `DBMS` |
| `101` | `OS` |

> [!TIP]
> `(StudentID, CourseID)`
> **Properties:** uniqueness, minimal, multiple columns
> A Composite Key can be selected as the Primary Key.

---

## 📉 Disadvantages of Composite Keys

### 1️⃣ Disadvantage 1 — Larger Indexes
Indexes store multiple columns. More storage is required.

### 2️⃣ Disadvantage 2 — Slower Joins
Joining tables on multiple columns is generally more expensive than joining on a single integer ID.

### 3️⃣ Disadvantage 3 — Larger Foreign Keys
If another table references this Composite Key, it must store **all** key columns.

**Example:**
Instead of `EnrollmentID`, another table must store `StudentID` AND `CourseID`.

---

## 🛠️ Creating a Composite Primary Key

```sql
CREATE TABLE Enrollment
(
 StudentID INT,
 CourseID INT,
 PRIMARY KEY(StudentID, CourseID)
);
```

---

## 🔗 Composite Foreign Key

Suppose another table stores marks.

```sql
CREATE TABLE Marks
(
 StudentID INT,
 CourseID INT,
 Marks INT,
 FOREIGN KEY(StudentID, CourseID)
 REFERENCES Enrollment(StudentID, CourseID)
);
```

> [!WARNING]
> Notice that the Foreign Key **must** reference both columns together!
