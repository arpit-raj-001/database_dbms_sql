<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔄 Transitive & Multivalued Dependency</h1>
  <h2>Chapter 45</h2>
</div>

---

## 🧐 What is Transitive Dependency?

> [!NOTE]
> Transitive dependency occurs when one **non-key attribute** determines another **non-key attribute**.

### Example:
- `EmpID → DepartmentID`
- `DepartmentID → Manager`

**Therefore:** `EmpID → Manager`

> [!WARNING]
> - Partial dependencies violate Second Normal Form (2NF).
> - Transitive dependencies violate Third Normal Form (3NF).

---

## 🚨 Why is Transitive Dependency Bad?

Suppose we have this table:

| EmpID | DepartmentID | Manager |
| :--- | :--- | :--- |
| `1` | `ECE` | `Dr. Sharma` |
| `2` | `ECE` | `Dr. Sharma` |
| `3` | `ECE` | `Dr. Sharma` |

**Manager name repeats many times.**

**Problems:**
- Redundancy
- Update anomaly
- Storage waste
- Data inconsistency

---

## 🛠️ Removing Transitive Dependency (3NF)

**Original Table**
`| EmpID | EmployeeName | DepartmentID | Manager |`

**Split into:**

**Employee**
`| EmpID | EmployeeName | DepartmentID |`

**Department**
`| DepartmentID | Manager |`

> [!TIP]
> Now `EmpID → DepartmentID` and `DepartmentID → Manager`. No redundant manager data exists. This satisfies **Third Normal Form (3NF)**.

---

## 🔀 What is a Multivalued Dependency (MVD)?

> [!NOTE]
> **Definition:** A Multivalued Dependency (MVD) exists when one attribute determines **multiple independent values** of another attribute.

### Independent Multivalued Attributes

**Consider a Student:**
- Programming Language
- Spoken Language

**Programming Languages:** `C++`, `Python`
**Spoken Languages:** `English`, `Hindi`

Learning Python does not affect which spoken languages Aadz knows.
Therefore, the two multivalued attributes are **independent**.

---

## 📉 Why MVD Causes Redundancy

**Original Table:**
| Student | Skill | Language |
| :--- | :--- | :--- |
| `Aadz` | `C++` | `English` |
| `Aadz` | `C++` | `Hindi` |
| `Aadz` | `Python` | `English` |
| `Aadz` | `Python` | `Hindi` |

**Rows required:**
`Skills × Languages = 2 × 2 = 4`

If Aadz later learns Java, the rows become:
`3 × 2 = 6`
> [!WARNING]
> Growth is multiplicative!

---

## 🛠️ Removing MVD (4NF)

**Split into:**

**StudentSkill**
| Student | Skill |
| :--- | :--- |

**StudentLanguage**
| Student | Language |
| :--- | :--- |

> [!TIP]
> Now no unnecessary combinations exist. This satisfies **Fourth Normal Form (4NF)**.

---

## 📊 Complete Comparison

| Feature | Partial Dependency | Transitive Dependency | Multivalued Dependency |
| :--- | :--- | :--- | :--- |
| **Depends on** | Part of a Composite Key | Another non-key attribute | Multiple independent values |
| **Violates** | `2NF` | `3NF` | `4NF` |
| **Requires Composite Key?**| ✅ Yes | ❌ No | ❌ No |
| **Main Problem** | Redundancy due to partial determination | Redundancy due to indirect determination | Redundancy due to independent multivalued facts |
| **Example** | `StudentID → StudentName` *(where key is StudentID, CourseID)* | `DepartmentID → HOD` | `Student ↠ Skill`, `Student ↠ SpokenLanguage` |
