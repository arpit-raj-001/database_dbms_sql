<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗝️ Database Key Concepts</h1>
  <h2>Chapter 43</h2>
</div>

---

## 🦸‍♂️ Super Key

> [!NOTE]
> **Definition:** A Super Key is any attribute or set of attributes that uniquely identifies every tuple (row) in a relation.

**In terms of Functional Dependencies:**
`X` is a Super Key if `X⁺` contains all attributes of the relation. A Super Key may contain unnecessary attributes; it's not minimal.

---

## 🌟 Candidate Key

A Candidate Key is the smallest (minimal) Super Key. It satisfies two properties:
1. **Uniqueness:** Determines all attributes.
2. **Irreducibility:** No attribute can be removed.

---

## 🗂️ Attributes Classification

### Prime Attributes
**Definition:** A Prime Attribute is an attribute that belongs to at least one Candidate Key.

### Non-prime Attributes
**Definition:** A Non-prime Attribute is an attribute that is not part of any Candidate Key.

---

## ⚙️ Algorithm to Find Candidate Keys

**Given:** Relation R, Set of Functional Dependencies F

- **Step 1:** List all attributes.
- **Step 2:** Identify attributes that never appear on the RHS of any FD. These **must** be included in every Candidate Key.
- **Step 3:** Compute the closure of these attributes.
- **Step 4:** If the closure contains all attributes, you have a Candidate Key. Otherwise, add more attributes and repeat.
- **Step 5:** Check minimality. Remove each attribute one at a time.

---

### Example

**Relation:** `R(A, B, C)`
**FDs:** `A → B`, `B → C`

**Find Candidate Key:**

1. **Compute:** `A⁺`
2. **Start:** `{A}`
3. **Apply `A → B`:** `{A, B}`
4. **Apply `B → C`:** `{A, B, C}`

**Candidate Key: `A`**
