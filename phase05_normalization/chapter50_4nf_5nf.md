<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🍀 Fourth and Fifth Normal Form (4NF & 5NF)</h1>
  <h2>Chapter 50</h2>
</div>

---

## 4️⃣ What is Fourth Normal Form (4NF)?

> [!NOTE]
> **Definition:** A relation is in Fourth Normal Form (4NF) if:
> - It is already in BCNF.
> - It contains **no non-trivial Multivalued Dependencies (MVDs)**.

For every Multivalued Dependency: `X ↠ Y`
- `X` must be a Super Key.
- A table should not store multiple independent facts about the same entity.

---

## 🔀 Multivalued Dependency Review

Suppose an ECE student like Aadz has:
- **Skills:** `C++`, `MATLAB`
- **Languages:** `English`, `Hindi`

*These two facts are completely independent.*

**Bad Table Design:**
| Student | Skill | Language |
| :--- | :--- | :--- |
| `Aadz` | `C++` | `English` |
| `Aadz` | `C++` | `Hindi` |
| `Aadz` | `MATLAB` | `English` |
| `Aadz` | `MATLAB` | `Hindi` |

> [!WARNING]
> **Notice what happened.**
> Every skill combines with every language. This redundancy comes from two independent multivalued facts.

### Decompose into 4NF

**StudentSkill Table:**
| Student | Skill |
| :--- | :--- |
| `Aadz` | `C++` |
| `Aadz` | `MATLAB` |

**StudentLanguage Table:**
| Student | Language |
| :--- | :--- |
| `Aadz` | `English` |
| `Aadz` | `Hindi` |

> [!TIP]
> No unnecessary combinations remain. The relation is now in **4NF**.

---

## 5️⃣ What is Fifth Normal Form (5NF)?

> [!NOTE]
> **Definition:** A relation is in Fifth Normal Form (5NF), also called **Projection-Join Normal Form (PJNF)**, if:
> - It is already in 4NF.
> - Every non-trivial Join Dependency is implied by Candidate Keys.

### Projection Join Normal Form (PJNF)
5NF is also called PJNF because:
`Projection` ↓ `Smaller relations` ↓ `Join` ↓ `Original relation recovered.`

### Mathematical Condition for Lossless Join

Suppose: $R \rightarrow R_1 + R_2$

The decomposition is lossless if:
- $(R_1 \cap R_2) \rightarrow R_1$
**OR**
- $(R_1 \cap R_2) \rightarrow R_2$

> [!TIP]
> **In words:**
> The common attributes between the decomposed relations must functionally determine at least one of the decomposed relations.
