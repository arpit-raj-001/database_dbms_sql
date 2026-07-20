<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>💪 Strong and Weak Entities</h1>
  <h2>Chapter 22</h2>
</div>

---

## 🏋️ What is a Strong Entity?

> [!NOTE]
> **Definition:**
> A Strong Entity is an entity that can be uniquely identified by its own attributes and can exist independently of other entities.
> 
> - It has its own **primary key**.
> - It does **not depend** on another entity for identification.

**A strong entity:**
- ✅ Has its own Primary Key
- ✅ Exists independently
- ✅ Can be uniquely identified
- ✅ Does not require another entity to exist (e.g., `StudentID`)

> [!TIP]
> Even if there are no courses, departments, or enrollments, students can still exist.
> **Student is a Strong Entity.**

---

## 🥀 What is a Weak Entity?

> [!NOTE]
> **Definition:**
> A Weak Entity is an entity that cannot be uniquely identified by its own attributes alone and whose existence depends on another (strong) entity.
> 
> - It **always depends** on a strong entity.

### Example 1: Company Dependents

**Employees (Strong Entity):**
| EMPLOYEEID | NAME |
| :--- | :--- |
| `101` | `Aadz` |
| `102` | `Arpit` |

*Each employee has family members.*

**Dependents:**
| NAME | RELATION |
| :--- | :--- |
| `Riya` | `Sister` |
| `Riya` | `Mother` |
| `Aman` | `Brother` |

**Can you uniquely identify a dependent using only the dependent's name?**
**No.** Many employees may have a dependent named "Riya."
Therefore, **Dependent is a Weak Entity**.

### Example 2: Hotel Rooms

Sometimes, an object only makes sense in the context of another object. Without the parent entity, these objects lose their identity.

- Suppose a hotel has: `Room Number: 101`
- Another hotel also has: `Room Number: 101`

**Can the room number alone identify the room?**
**No.** You need: `HotelID + RoomNumber`

> [!WARNING]
> Room depends on Hotel.
> **Room is a Weak Entity.**

---

## 🔑 Key Relationships & Identifiers

1. **Identifying Relationship:** This is the relationship through which a weak entity is identified using the primary key of its owner (strong entity).
2. **Partial Key (Discriminator):** This is an attribute of a weak entity that uniquely identifies it *only within the context of its parent strong entity*. 
   *(e.g., Room number uniquely identifies a room within a hotel, but without the hotel number, the room number doesn't uniquely identify it globally).*

> [!IMPORTANT]
> - Without the employee, the dependent no longer makes sense.
> - Weak entities have **existence dependency** on strong entities.
> - A weak entity depends on a strong entity for both **identification** and **existence**.
> - If the owner entity is deleted, the weak entity typically loses its meaning and is also removed *(often via cascading delete)*.

---

## ❓ Why Can't Weak Entities Have Their Own Primary Key?

A weak entity does not have enough attributes to uniquely identify itself globally. Its attributes are only unique within the context of the owning entity.

Weak entities exist because some real-world objects only make sense in relation to another entity and cannot be uniquely identified on their own.
