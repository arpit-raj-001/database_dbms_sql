<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>⚙️ Functional Dependency Notes</h1>
  <h2>Chapter 42</h2>
</div>

---

## 🧐 What is a Functional Dependency?

> [!NOTE]
> **FORMAL DEFINITION:**
> A Functional Dependency (FD) exists when the value of one attribute (or set of attributes) uniquely determines the value of another attribute (or set of attributes).

> [!TIP]
> Whenever two rows have the same value of **X**, they must have the same value of **Y**.

**Dependency:**
`RollNo → Name`
*If `RollNo` is 006, `Name` must always be aadz.*

---

## 🔑 Candidate Keys using Functional Dependencies

> [!NOTE]
> **DEFINITION:**
> A Candidate Key is the smallest set of attributes that can determine all attributes in a relation. We use Functional Dependencies to find candidate keys.

**Relation:** `RESULT(StudentID, CourseID, Grade)`
**FD:** `(StudentID, CourseID) → Grade`

Neither `StudentID` nor `CourseID` alone determines `Grade`.
Therefore, **Candidate Key:** `(StudentID, CourseID)`

> [!IMPORTANT]
> **RULE:** A Candidate Key is:
> - Unique *(identifies every row)*.
> - Minimal *(no unnecessary attributes)*.

---

## 🧮 Attribute Closure

> [!NOTE]
> **DEFINITION:**
> The closure of an attribute set **X**, denoted by **X⁺**, is the set of all attributes that can be functionally determined from X using the given Functional Dependencies.

**Relation:** `R(A, B, C)`
**FDs:**
- `A → B`
- `B → C`

**Find:** `A⁺`

### Solution
1. **Start:** `A⁺ = {A}`
2. **Apply `A → B`:** Now: `{A, B}`
3. **Apply `B → C`:** Final: `A⁺ = {A, B, C}`

> [!TIP]
> **TO COMPUTE X⁺:**
> 1. Start with X itself.
> 2. Look for any FD whose left side is already in the closure.
> 3. Add the right-side attributes.
> 4. Repeat until no new attributes can be added.

---

## 🗂️ Types of Functional Dependencies

### 1. Trivial Functional Dependency

**DEFINITION:** A Functional Dependency is trivial if the right-hand side is a subset of the left-hand side.
**Mathematically:** If `Y ⊆ X`, then `X → Y` is trivial.

**Example:**
`(StudentID, Name) → Name`
Since `Name` is already part of the left-hand side, the dependency is trivial.

### 2. Non-Trivial Functional Dependency

**DEFINITION:** A Functional Dependency is non-trivial if the right-hand side is not a subset of the left-hand side.

**Example:**
`StudentID → Name`
`Name` is not part of `StudentID`, so this is non-trivial.

### 3. Completely Non-Trivial Functional Dependency

**DEFINITION:** A Functional Dependency is completely non-trivial if the left-hand side and right-hand side have no common attributes.
`X ∩ Y = ∅`

---

## 📜 Armstrong's Axioms

### 1. Reflexivity
**RULE:** If `Y ⊆ X`, Then: `X → Y`.

### 2. Augmentation
**RULE:** If `X → Y`, Then for any attribute set `Z`: `XZ → YZ`.

**Example:**
Given: `StudentID → Name`
Add `CourseID` to both sides:
`(StudentID, CourseID) → (Name, CourseID)`.

### 3. Transitivity
**RULE:** If `X → Y` and `Y → Z`, Then: `X → Z`.

---

## 🔄 Derived Rules
*These are obtained from Armstrong's Axioms.*

### UNION (ADDITIVITY)
If: `X → Y` and `X → Z`
Then: `X → YZ`.

### DECOMPOSITION (PROJECTIVITY)
If: `X → YZ`
Then: `X → Y` and `X → Z`.

### COMPOSITION
If: `X → Y` and `A → B`
Then: `XA → YB`.

### PSEUDOTRANSITIVITY
**Rule:**
If: `X → Y` and `WY → Z`
Then: `WX → Z`.

**Example:**
Given:
`StudentID → Department`
`(Department, Semester) → Classroom`
**Infer:** `(StudentID, Semester) → Classroom`
