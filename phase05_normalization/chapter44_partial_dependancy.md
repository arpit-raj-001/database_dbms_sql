<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧩 Partial Dependency</h1>
  <h2>Chapter 44</h2>
</div>

---

## 🧐 What is Partial Dependency?

> [!NOTE]
> **Formal Definition:** A Partial Dependency exists when a non-prime attribute is functionally dependent on only a **part** of a composite Candidate Key, instead of the entire key.

### Example:
Candidate Key: `(StudentID, CourseID)`
Suppose: `StudentID → StudentName`

`StudentName` does not need `CourseID`. Therefore, `StudentName` is only *partially dependent*.

---

## ❓ Why is this a Problem?

Because `StudentName` will be repeated for every course taken by the same student.

| StudentID | CourseID | StudentName |
| :--- | :--- | :--- |
| `101` | `DB101` | `aadz` |
| `101` | `OS201` | `aadz` |
| `101` | `CN301` | `aadz` |

> [!WARNING]
> The value "aadz" is stored repeatedly. This causes: **Redundancy, Update anomalies, Storage waste.**

---

## 📜 Important Rules

- Partial Dependency can **only** occur when there is a Composite Candidate Key.
- Partial Dependency **always** involves a Non-prime Attribute.

---

## 🔎 Detecting Partial Dependency

1. **Step 1:** Find the Candidate Key.
2. **Step 2:** Check if it is Composite. If not, stop. No Partial Dependency exists.
3. **Step 3:** Check every Functional Dependency. Ask: *"Does the dependent attribute depend on the entire key or only part of it?"*

---

## 🛠️ Removing Partial Dependency (Converting to 2NF)

Partial Dependencies are removed by decomposing the table.

**Table 1 (Student):** `| StudentID | StudentName |`
**Table 2 (Course):** `| CourseID | CourseName |`
**Table 3 (Enrollment):** `| StudentID | CourseID | Grade |`

**Now:**
- `StudentName` depends entirely on `StudentID`.
- `CourseName` depends entirely on `CourseID`.
- `Grade` depends entirely on `(StudentID, CourseID)`.

> [!TIP]
> No Partial Dependency remains. This satisfies **Second Normal Form (2NF)**.

---

### Example 4 – Hospital

**Table:**
`| PatientID | DoctorID | PatientName | Diagnosis |`

**FDs:** 
- `(PatientID, DoctorID) → Diagnosis`
- `PatientID → PatientName`

**Partial Dependency:** `PatientID → PatientName`
