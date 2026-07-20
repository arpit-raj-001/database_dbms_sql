<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🛡️ Database Constraints</h1>
  <h2>Chapter 74</h2>
</div>

---

A constraint is a database rule that enforces data integrity by restricting the values that can be inserted, updated, or deleted.

**Example With Constraints:**
```sql
CREATE TABLE Students(
 StudentID INT PRIMARY KEY,
 Name VARCHAR(100) NOT NULL,
 Age INT CHECK(Age>=0)
);
```

---

## ⚖️ Business Rules vs Database Rules

| Business Rule | Database Rule |
| :--- | :--- |
| Every employee must have a salary. | `Salary NOT NULL` |
| Email should be unique. | `Email UNIQUE` |
| Age must be positive. | `CHECK(Age>0)` |

---

## 🎯 Constraint Levels

### Column-Level Constraints
Applied directly to one column.

**Example:**
```sql
CREATE TABLE Students(
 Name VARCHAR(100) NOT NULL
);
```

### Table-Level Constraints
Defined after all columns.

**Example:**
```sql
CREATE TABLE Students(
 StudentID INT,
 CourseID INT,
 PRIMARY KEY(StudentID, CourseID)
);
```

---

## 🔄 During UPDATE

Suppose:
```sql
UPDATE Students SET Age=-5 WHERE StudentID=1;
```

**Flow:**
1. Receive UPDATE
2. Modify row temporarily
3. Check constraints
4. Valid?
5. If yes → Store changes
6. Else → rollback statement

---

## 🛠️ Modifying Constraints

Constraints don't have to exist from day one. They can be created, modified, or removed over time.

### Adding Constraints
```sql
ALTER TABLE Students ADD CONSTRAINT uq_email UNIQUE(Email);
```

### Dropping Constraints
Suppose you no longer need a constraint.
```sql
ALTER TABLE Students DROP INDEX uq_email;
```

### Renaming Constraints
Unlike SQL Server or PostgreSQL, MySQL does not provide a general `RENAME CONSTRAINT` statement.

**Typical approach:**
1. Drop the old constraint.
2. Recreate it with the new name.

> [!NOTE]
> Some constraint types (like indexes backing `UNIQUE`) can be renamed separately using index-related commands.
