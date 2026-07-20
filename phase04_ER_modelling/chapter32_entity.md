<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧠 Entity Relationship Concepts</h1>
  <h2>Chapter 32</h2>
</div>

---

## 1️⃣ Formal Definition

> [!NOTE]
> An **Entity** is any real-world object, person, place, event, concept, or thing about which the database stores information.

---

## 2️⃣ Characteristics of an Entity

Every entity has certain properties:

1. **It has an identity:** Each entity can be uniquely identified.
2. **It has attributes:** Every entity has properties.
   *(Example Student: Name, Age, Roll Number)*
3. **It exists independently in the problem domain:** A student exists whether or not they are enrolled in a course.
4. **Multiple instances may exist:**
   - *Entity:* Student
   - *Instances:* Arpit, Aadz.

---

## 3️⃣ Entity Set

> [!TIP]
> An **Entity Set** is a collection of similar entities of the same type having the same attributes.
> So, `arpit` + `aadz` = `student entity set`.

---

## 4️⃣ Entity Types

### 💪 Strong Entity
A Strong Entity can exist independently and has its own primary key.

### 🥀 Weak Entity
A Weak Entity is an entity that cannot be uniquely identified by its own attributes alone and depends on another (owner) entity for its identity and existence.
