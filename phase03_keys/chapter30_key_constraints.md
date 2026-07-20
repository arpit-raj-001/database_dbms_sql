<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔒 SQL Constraints and Key Selection</h1>
  <h2>Chapter 30</h2>
</div>

---

## 🎯 Primary Key Selection Rules

1. **Must be Unique:** Every row should have a different value.
2. **Must Never Be NULL:** Primary Keys cannot contain NULL values.
3. **Should Be Stable:** Changing a Primary Key affects all related Foreign Keys. Stable keys reduce maintenance.
4. **Keep It Small:** Smaller keys mean Smaller indexes, Faster joins, Lower storage requirements.
5. **Avoid Business Information:** Business rules change. Keys should not. *(Example: Avoid EmployeeEmail, Prefer EmployeeID)*.
6. **Prefer Single Column:** Single-column Primary Keys are easier to reference and index. Composite Primary Keys should be used only when the natural identity truly requires multiple columns.

---

## ⚙️ Key Constraints in SQL

### PRIMARY KEY Constraint
A Primary Key constraint ensures: uniqueness, NOT NULL, row identification.

**Column-Level Syntax:**
```sql
CREATE TABLE Student
(
 StudentID INT PRIMARY KEY,
 Name VARCHAR(50)
);
```

**Table-Level Syntax:**
```sql
CREATE TABLE Student
(
 StudentID INT,
 Name VARCHAR(50),
 PRIMARY KEY(StudentID)
);
```

**Composite Primary Key:**
```sql
CREATE TABLE Enrollment
(
 StudentID INT,
 CourseID INT,
 PRIMARY KEY(StudentID, CourseID)
);
```

---

### FOREIGN KEY Constraint
A Foreign Key creates a relationship between two tables.

```sql
CREATE TABLE Department
(
 DepartmentID INT PRIMARY KEY,
 DepartmentName VARCHAR(50)
);

CREATE TABLE Employee
(
 EmployeeID INT PRIMARY KEY,
 DepartmentID INT,
 FOREIGN KEY (DepartmentID) REFERENCES Department(DepartmentID)
);
```

---

### UNIQUE Constraint
A Unique constraint prevents duplicate values.

```sql
CREATE TABLE Employee
(
 EmployeeID INT PRIMARY KEY,
 Email VARCHAR(100) UNIQUE
);

/* Composite UNIQUE */
CREATE TABLE Student
(
 RollNo INT,
 Semester INT,
 UNIQUE(RollNo, Semester)
);
```

---

## 🛠️ Managing Constraints (ALTER TABLE)

Sometimes, tables already exist. You need to add constraints later.

**Add Primary Key:**
```sql
ALTER TABLE Student
ADD PRIMARY KEY(StudentID);
```

> [!NOTE]
> **Can a PRIMARY KEY be added later?** Yes. Use `ALTER TABLE`. However, existing data must satisfy the rules (no duplicate values, no NULL values).

```sql
ALTER TABLE Student
ADD CONSTRAINT PK_Student
PRIMARY KEY(StudentID);
```

---

### REFERENCES Clause
The `REFERENCES` keyword tells SQL which table and which column is being referenced.

```sql
FOREIGN KEY(CustomerID)
REFERENCES Customer(CustomerID)
```
*Meaning: CustomerID in this table must already exist in Customer.*

### Removing a Foreign Key
A Foreign Key constraint can be removed using `ALTER TABLE` with the appropriate `DROP CONSTRAINT`.

```sql
ALTER TABLE Employee
DROP CONSTRAINT FK_Department;
```
