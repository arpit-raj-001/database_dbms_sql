<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🤝 Database Relationships</h1>
  <h2>Chapter 34</h2>
</div>

---

## 🧐 What is a Relationship?

> [!NOTE]
> **Definition:** A relationship is an association or connection between two or more entities that describes how they interact with each other.

| Entity | Relationship |
| :--- | :--- |
| Represents an object | Represents an association |
| Usually a noun | Usually a verb |
| Has attributes | Can also have attributes |
| Rectangle (Chen notation) | Diamond (Chen notation) |

**Example:** `Student` (Entity), `Enrolls` (Relationship).

### Characteristics of Relationships
- Connects two or more entities.
- Has a name.
- May have attributes.
- Has a degree *(Unary, Binary, etc.)*.
- Has cardinality constraints *(covered in the next chapter)*.
- Has participation constraints *(covered later)*.

---

## 🕸️ Relationship Set

> [!TIP]
> **Definition:** A relationship set is the collection of all relationships of the same type.

---

## 🔢 Degree of Relationship

The degree of a relationship tells us: **How many entity types participate in the relationship.**

### 1️⃣ Unary Relationship
**Definition:** A relationship involving one entity type. The entity relates to itself.
**Example:** Employee manages Employee.

### 2️⃣ Binary Relationship
**Definition:** A relationship involving two entity types. This is the most common relationship in databases.

### 3️⃣ Ternary Relationship
**Definition:** A relationship involving three entity types simultaneously.
**Example:** Doctor prescribes Medicine to Patient.

### 4️⃣ Recursive Relationship
A recursive relationship is a relationship where an entity relates to another instance of the same entity type. This is essentially a special case of a unary relationship. Every recursive relationship is unary because only one entity type participates. Use relationship attributes only when they describe the association itself.

---

## 📝 Practice Questions

<details>
<summary><b>Q6. Why can't every ternary relationship be replaced by binary relationships?</b></summary>
<br>
<b>Answer:</b> Because a ternary relationship captures a single association involving three entities simultaneously. Splitting it into binary relationships may lose important business meaning.
</details>
