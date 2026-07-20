<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🥇 First and Second Normal Form (1NF & 2NF)</h1>
  <h2>Chapter 48</h2>
</div>

---

## 1️⃣ First Normal Form (1NF)

> [!NOTE]
> A relation is in First Normal Form (1NF) if:
> - Every attribute contains atomic (indivisible) values.
> - Every row is unique.
> - There are no repeating groups.
> - Every column stores values of a single type.

### Repeating Groups
**Definition:** A repeating group occurs when the same type of information is stored multiple times in one row.

**Example (Bad Design):**
| StudentID | Phone1 | Phone2 | Phone3 |
| :--- | :--- | :--- | :--- |
| `101` | `9876` | `9123` | `8456` |

**Why are Repeating Groups Bad?**
Suppose aadz buys five books. The table only has `Book1`, `Book2`, `Book3`. Where will Books 4 and 5 go? The design is inflexible.

### Convert to 1NF

**Original Table:**
| StudentID | Skills |
| :--- | :--- |
| `101` | `C++, Python` |

**Solution (1NF):**
| StudentID | Skill |
| :--- | :--- |
| `101` | `C++` |
| `101` | `Python` |

> [!TIP]
> - Repeated rows are acceptable.
> - Only multiple values inside *one cell* violate 1NF.

---

## 2️⃣ Second Normal Form (2NF)

> [!NOTE]
> A relation is in Second Normal Form (2NF) if:
> 1. It is already in 1NF.
> 2. Every non-prime attribute is fully functionally dependent on the entire Candidate Key. *(i.e., partial dependency must be removed)*

**2NF mainly applies when a table has a Composite Candidate Key.**

### Converting to 2NF

**Original Table (Not 2NF):**
| StudentID | CourseID | StudentName | Grade |
| :--- | :--- | :--- | :--- |
| *value* | *value* | *value* | *value* |

**Split into:**

**Student Table:**
| StudentID | StudentName |
| :--- | :--- |

**Enrollment Table:**
| StudentID | CourseID | Grade |
| :--- | :--- | :--- |

> [!TIP]
> **Now:**
> - `StudentName` depends entirely on `StudentID`.
> - `Grade` depends entirely on `(StudentID, CourseID)`.
> - No Partial Dependency remains.
> 
> *A single-column Primary Key automatically satisfies 2NF.*
