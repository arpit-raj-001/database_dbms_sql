<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔗 What is a Dependency?</h1>
  <h2>Chapter 41</h2>
</div>

---

## 📖 Definition

> [!NOTE]
> A **dependency** is a relationship between attributes where the value of one attribute determines the value of another attribute. 

Think of an Aadhaar number. If someone tells you an Aadhaar number, you can identify exactly one person's details.

---

## 🚫 Without Dependencies

Without dependencies, you cannot:
- Find Candidate Keys.
- Normalize tables.
- Detect Partial Dependencies.
- Detect Transitive Dependencies.
- Verify BCNF.

> [!IMPORTANT]
> **Dependencies are the language of normalization.**

---

## 🎯 Determinant

> [!NOTE]
> **Definition:** A Determinant is the attribute (or set of attributes) that determines another attribute.

**Dependency:** `RollNo` → `Student`
**Determinant:** `RollNo`

### Composite Determinant
Sometimes multiple attributes together determine another.
**Example:** `(StudentID, CourseID)` → `Grade`

### Dependent Attribute
> [!NOTE]
> **Definition:** A Dependent Attribute is the attribute whose value is determined by another attribute.
> 
> *In the example above, `Grade` is the dependent attribute.*

---

## 🗂️ Types of Dependencies

Normalization uses several kinds of dependencies. We'll study each briefly here and then dedicate full chapters to them later.

### 1. Functional Dependency (FD)
**Definition:** One attribute uniquely determines another.
**Notation:** `X → Y`
**Meaning:** If two rows have the same `X`, they must also have the same `Y`.
**Example:** `StudentID → Name`

### 2. Full Functional Dependency
An attribute depends on the **entire** composite key.
**Example:** `(StudentID, CourseID) → Grade`

### 3. Partial Dependency
An attribute depends on only **part** of a composite key.
**Example:** 
- Composite Key: `(StudentID, CourseID)`
- Dependencies: `StudentID → StudentName`

`StudentName` depends *only* on `StudentID`. It does not require `CourseID`. This is a Partial Dependency.

### 4. Transitive Dependency
One non-key attribute determines another non-key attribute.
**Example:**
- `EmployeeID → DepartmentID`
- `DepartmentID → Manager`
- **Therefore:** `EmployeeID → Manager`

> [!WARNING]
> - Partial dependencies violate Second Normal Form (2NF).
> - Transitive dependencies violate Third Normal Form (3NF).

### 5. Multivalued Dependency (MVD)
One attribute independently determines multiple values of another attribute.
**Notation:** `X ↠ Y`

### 6. Join Dependency (JD)
A table can be decomposed into multiple tables and reconstructed using joins without losing information. This forms the basis of Fifth Normal Form (5NF).

---

## 🌳 Dependency Hierarchy

```text
Dependency
│
├── Functional Dependency (FD)
│   │
│   ├── Full Functional Dependency
│   ├── Partial Dependency
│   └── Transitive Dependency
│
├── Multivalued Dependency (MVD)
│
└── Join Dependency (JD)
```

> [!TIP]
> A **relationship** connects entities. A **dependency** connects attributes.
