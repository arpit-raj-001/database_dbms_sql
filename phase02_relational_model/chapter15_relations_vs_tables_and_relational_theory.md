<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧠 Relational Theory & Concepts</h1>
  <h2>Chapter 15</h2>
</div>

---

> [!NOTE]
> A **relation** is a mathematical set of tuples that all share the same set of attributes. 
> A relation consists of:
> - **Attributes** (columns)
> - **Tuples** (rows)

**In relational theory:**
A relation $R$ over attributes $A_1, A_2, A_3, ..., A_n$ is a finite set of tuples, where every tuple contains one value for each attribute, and every value belongs to the attribute's domain.

---

## 📏 4. Properties of a Relation

A relation must satisfy several mathematical properties.

### Property 1 — Every Relation Has a Unique Name
**Example:**
- `Students`
- `Departments`
- `Courses`

*Each relation in a database must be uniquely identifiable.*

### Property 2 — Every Attribute Has a Unique Name
**Correct:**
| StudentID | Name | CGPA |
| :--- | :--- | :--- |

### Property 3 — All Tuples Have the Same Attributes
Every row follows the exact same structure.

**Correct:**
| ID | Name | CGPA |
| :--- | :--- | :--- |
| `006` | `aadz` | `7.97` |

Every row has exactly three values. i.e `id`, `name`, `cg`. (e.g., `006`, `aadz`, `7.97`). Different rows cannot have different structures within the same relation.

### Property 4 — Every Cell Contains a Single (Atomic) Value

### Property 5 — All Values in a Column Come from the Same Domain
**Example:** `Age` column cannot contain names.

### Property 6 — Duplicate Tuples Are Not Allowed
Because a relation is a mathematical set. A set never contains duplicate elements.

> [!WARNING]
> **SQL allows duplicates. Doesn't that contradict the relational model?**
> Yes. SQL is not a pure mathematical implementation of relational theory.

### Property 7 — Order of Rows Does Not Matter
Without an `ORDER BY` clause, SQL does not guarantee row order. The database may return rows in any sequence.

### Property 8 — Order of Columns Does Not Matter
However, in SQL, column order affects how results are displayed and how `SELECT *` expands, so implementations preserve an order.

### Property 9 — A Relation Is Finite
A database relation contains a finite number of tuples. Codd's relational model assumed every attribute had a valid value from its domain. The treatment of missing information became a subject of later debate, which introduced `NULL`, supported by SQL.

---

## 🥊 10. Relation vs SQL Table

| Relational Theory | SQL Table |
| :--- | :--- |
| No duplicate tuples | Duplicate rows are allowed unless prevented by constraints or queries |
| Unordered rows | Row order is not guaranteed unless `ORDER BY` is used |
| Unordered attributes (theoretical) | Columns have a defined display order |
| Pure mathematical set | Practical implementation |
| Originally did not include SQL-style `NULL` semantics | Supports `NULL` values. |

---

> [!IMPORTANT]
> **Is a relation exactly the same as a table?**
> **No.**
> A table is the SQL implementation of the relational concept. While they are closely related, SQL tables allow features such as duplicate rows and NULL values that differ from the original mathematical definition of a relation.
> 
> SQL is a practical implementation designed for real-world systems. Features such as duplicate rows, NULL values, and implementation-specific behaviors improve usability and flexibility, even though they differ from the original mathematical relational model.
