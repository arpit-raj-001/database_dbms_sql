<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔗 Database Dependencies & Join Dependency</h1>
  <h2>Chapter 46</h2>
</div>

---

## 🏗️ The Decomposition Problem

Suppose a company stores information about:
- Suppliers
- Parts
- Projects

**Initially, they create one table:**
| Supplier | Part | Project |
| :--- | :--- | :--- |
| S1 | P1 | X |
| S1 | P2 | X |
| S2 | P1 | Y |

As the database grows, redundancy increases.
Instead of storing everything in one table, we split it into smaller tables:

**Supplier-Part**
| Supplier | Part |
| :--- | :--- |
| S1 | P1 |
| S1 | P2 |
| S2 | P1 |

**Supplier-Project**
| Supplier | Project |
| :--- | :--- |
| S1 | X |
| S2 | Y |

**Part-Project**
| Part | Project |
| :--- | :--- |
| P1 | X |
| P1 | Y |
| P2 | X |

> [!WARNING]
> **Now the question becomes:**
> Can we reconstruct the original table by joining these smaller tables?
> If yes, we have a **Lossless Join**, and this situation is described by a **Join Dependency**.

---

## 🧩 What is Join Dependency?

> [!NOTE]
> **Definition:** A Join Dependency (JD) states that a relation can be decomposed into two or more smaller relations and reconstructed exactly by joining them.

**Simple Definition:**
Instead of storing everything in one large table `ABC`, we store `AB`, `BC`, `AC`. 
Later, joining them gives back `ABC` without losing or creating extra data.

---

## ✅ Lossless Join

> [!TIP]
> **A Lossless Join means:**
> - After decomposition, joining all decomposed tables recreates exactly the original table.
> - Nothing is lost.
> - Nothing extra is created.

---

## ❌ Lossy Join

> [!WARNING]
> **Definition:** A Lossy Join occurs when decomposition cannot reconstruct the original relation correctly.
> It either:
> - Loses tuples, or
> - Creates extra tuples (spurious tuples).

### Example of Lossy Join

**Original**
| Student | Club |
| :--- | :--- |
| `Aadz` | `Robotics` |
| `Arpit` | `Coding` |

**Suppose we decompose into:**
**Student**
| Student |
| :--- |
| `Aadz` |
| `Arpit` |

**Club**
| Club |
| :--- |
| `Robotics` |
| `Coding` |

**Joining these tables gives:**
| Student | Club |
| :--- | :--- |
| `Aadz` | `Robotics` |
| ❌ **`Aadz`** | ❌ **`Coding`** |
| ❌ **`Arpit`** | ❌ **`Robotics`** |
| `Arpit` | `Coding` |

*Two extra rows appear that never existed. This is a Lossy Join.*

---

## 🔄 Reconstruction

> [!NOTE]
> **Definition:** Reconstruction means recreating the original relation by joining decomposed relations.

### Normal Form Removal Mapping

| Normal Form | Removes |
| :--- | :--- |
| **2NF** | Partial Dependency |
| **3NF** | Transitive Dependency |
| **BCNF** | Non-candidate determinants |
| **4NF** | Multivalued Dependency |
| **5NF** | Join Dependency |
