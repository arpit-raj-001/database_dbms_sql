<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📦 Database Modeling: Aggregation</h1>
  <h2>Chapter 38</h2>
</div>

---

## 🧩 The Problem Aggregation Solves

Consider these facts:
- **Employee:** Aadz, Arpit
- **Project:** Database Project, AI Project
- **Relationship:** `Employee ---- Works_On ---- Project`

Now suppose: Manager Neha supervises Aadz working on the Database Project.
- She is not supervising Aadz everywhere.
- She is not supervising the Database Project itself.
- She supervises: `(Aadz Works_On Database Project)`

> [!WARNING]
> That entire relationship must now participate in another relationship. This **cannot** be modeled using ordinary ER relationships.

---

## 🛠️ What is Aggregation?

> [!NOTE]
> **Definition:** Aggregation is an abstraction in ER modeling where a relationship set is treated as a higher-level entity so that it can participate in another relationship.
> 
> **Simply:** Aggregation allows a relationship to behave like an entity.

### Easy Analogy
Imagine a cricket tournament.
- **Normal relationship:** `Player ---- Plays ---- Match`
- Now suppose: An Umpire supervises a Player playing a Match. The umpire isn't supervising the Player alone or the Match alone.

---

## 🔗 Relationship Between Relationships

This is the defining feature of Aggregation.
- **Normally:** Entities participate in relationships.
- **Aggregation allows:** A relationship to participate in another relationship.

### Example Visualization
`Manager ---- Supervises ---- (Employee ---- Works_On ---- Project)`

### Aggregation Notation
In Chen notation, aggregation is represented by enclosing a relationship (along with the participating entities) inside a rectangle (box).

```text
+--------------------------------------+
| Employee ---- Works_On ---- Project  |
+--------------------------------------+
                |
           Supervises
                |
             Manager 
```

---

## 💾 Mapping Aggregation to Relational Tables

Aggregation has no special SQL construct. Instead, it is implemented using additional tables and foreign keys.

### Example Tables

**Employee**
| EmpID | Name |
| :--- | :--- |
| `1` | `Aadz` |

**Project**
| ProjectID | Name |
| :--- | :--- |
| `10` | `AI Recommendation` |

**Works_On**
| EmpID | ProjectID | Hours |
| :--- | :--- | :--- |
| `1` | `10` | `30` |

**Manager**
| ManagerID | Name |
| :--- | :--- |
| `5` | `Rohan` |

**Supervises**
| ManagerID | EmpID | ProjectID |
| :--- | :--- | :--- |
| `5` | `1` | `10` |

> [!IMPORTANT]
> Notice that `Supervises` references the `Works_On` relationship through the `(EmpID, ProjectID)` pair.

---

## ❓ How do you identify when aggregation is required?

**Answer:** Ask this question:
*"Does the new relationship involve an existing relationship rather than a single entity?"*

If yes, aggregation is likely required.
