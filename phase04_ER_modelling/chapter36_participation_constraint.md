<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🚪 Participation Constraints in Database Design</h1>
  <h2>Chapter 36</h2>
</div>

---

## 📖 Definition

> [!NOTE]
> A **Participation Constraint** specifies whether the participation of an entity in a relationship is mandatory or optional.

### Example
`Employee works in Department.`

**Business Rule:**
- Every employee must belong to a department.
- **Employee participation:** Mandatory
- **Department participation:** May be optional *(a newly created department might temporarily have no employees)*.

> [!TIP]
> **Participation constraints help enforce:**
> - Business rules
> - Data consistency
> - Database integrity

---

## ⚖️ Participation vs Cardinality
*This is one of the most frequently asked interview questions.*

| Feature | Cardinality | Participation |
| :--- | :--- | :--- |
| **Answers** | How many? | Must participate? |
| **Types** | `1:1`, `1:N`, `M:N` | `Total`, `Partial` |
| **Represents** | Maximum participation | Minimum participation |
| **Nature** | Quantitative | Mandatory/Optional |

### Example
`Department -------- Employee`

- **Cardinality:** `1 : N`
- **Participation:**
  - `Employee` → Total
  - `Department` → Partial

*These are entirely different constraints.*

---

## 💯 Total Participation

> [!NOTE]
> **Definition:** An entity has Total Participation if every instance of that entity **must** participate in at least one relationship instance.

- **Diagram (Chen Notation):** Double line `(Employee || Works In | Department)`
- A double line indicates Mandatory participation.

---

## 🌓 Partial Participation

> [!NOTE]
> **Definition:** An entity has Partial Participation if some instances **may not** participate in the relationship.
> *(Also called: Optional Participation)*

- **Diagram:** Single line `(Department | Works In || Employee)`

---

## 🛠️ How is Total Participation Implemented in SQL?

**Answer:** Typically by using a **Foreign Key** combined with a **NOT NULL** constraint.

### Example:
```sql
CREATE TABLE Employee (
 EmpID INT PRIMARY KEY,
 Name VARCHAR(100),
 DeptID INT NOT NULL,
 FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
);
```

> [!IMPORTANT]
> The `NOT NULL` constraint ensures that every employee **must** reference a department (i.e. Total Participation).
