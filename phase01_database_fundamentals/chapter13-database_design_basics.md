<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📐 Database Design Basics</h1>
  <h2>Chapter 13</h2>
</div>

---

> [!NOTE]
> **Database design** is the process of organizing data, defining relationships, enforcing constraints, and structuring the database to ensure correctness, efficiency, scalability, and maintainability.

### 🌟 Pillars of Good Database Design
1. **Integrity**
2. **Redundancy**
3. **Relationships**
4. **Constraints**
5. **Normalization**

---

## 🛡️ What is Integrity?
**Integrity means:** The data stored in the database must always remain correct, valid, and follow constraints and business logic.

## 🔁 What is Redundancy?
**Redundancy means:** The unnecessary duplication of the same data in multiple places. 

> [!WARNING]
> If there is a partial updation of data, one row will contradict the other, and inconsistency is introduced.

### Why Reduce Redundancy?
It helps:
- 💾 Saves storage
- 🛑 Prevents inconsistencies
- ⚡ Simplifies updates
- 🛠️ Improves maintainability

---

## 🔗 Relationships
Relationships model real-world associations between entities.
**Benefits:**
- Better organization
- Less duplication
- Easier maintenance

---

## 🚧 Constraints
Constraints are rules enforced by the DBMS to ensure the validity and integrity of stored data.
**Examples include:**
- `Primary Key`
- `Foreign Key`
- `NOT NULL`
- `UNIQUE`
- `CHECK`

---

## 🏗️ Normalization

> [!IMPORTANT]
> **Normalization** is the process of organizing data to reduce redundancy and improve data integrity.

### ❌ Poor Design

| Student | Course1 | Course2 | Course3 |
| :--- | :--- | :--- | :--- |
| Aadz | vlsi | dsa | mwe |

**Problems:**
- Limited number of course columns
- Many NULL values
- Difficult updates

### ✅ Better Design

**Students Table:**
| StudentID | Name |
| :--- | :--- |
| ... | ... |

**Courses Table:**
| CourseID | CourseName |
| :--- | :--- |
| ... | ... |

**Enrollments Table:**
| StudentID | CourseID |
| :--- | :--- |
| ... | ... |

**Now:**
- No repeated course columns
- Highly flexible
- Easier to maintain

### Why Normalize?
Normalization helps:
- Reduce redundancy
- Improve consistency
- Simplify updates
- Improve maintainability

> [!TIP]
> Normalization usually improves integrity and reduces redundancy. However, highly normalized schemas may require more joins, so performance depends heavily on the workload.

---

## 📝 Practice Questions

<details>
<summary><b>Q1. What is database design, and why is it important?</b></summary>
<br>
<b>A1:</b> Database design is the process of organizing data, defining relationships, and enforcing rules so that the database is correct, efficient, scalable, and maintainable. Good design reduces redundancy, preserves integrity, and supports future growth.
</details>

<details>
<summary><b>Q2. What is data integrity? Give two examples.</b></summary>
<br>
<b>A2:</b> Data integrity ensures that the database always contains valid and consistent data.<br>
Examples:<br>
• Preventing negative account balances using a CHECK constraint.<br>
• Ensuring every order references an existing customer through a foreign key.
</details>

<details>
<summary><b>Q3. Explain data redundancy with an example. Why is it problematic?</b></summary>
<br>
<b>A3:</b> Data redundancy is the unnecessary duplication of the same information in multiple places. For example, storing a customer's phone number in every order record leads to repeated data. If the phone number changes and not all copies are updated, inconsistent data results.
</details>

<details>
<summary><b>Q4. Why are relationships important in relational databases?</b></summary>
<br>
<b>A4:</b> Relationships model real-world associations between entities such as customers and orders or students and courses. They reduce duplication, improve organization, and enable efficient querying while preserving data integrity.
</details>

<details>
<summary><b>Q5. What are constraints, and why should they be enforced by the database?</b></summary>
<br>
<b>A5:</b> Constraints are rules enforced by the DBMS to ensure valid data. Examples include PRIMARY KEY, FOREIGN KEY, NOT NULL, UNIQUE, and CHECK. They protect the database even if applications contain bugs or users access the database directly.
</details>

<details>
<summary><b>Q6. What is normalization? What problems does it solve?</b></summary>
<br>
<b>A6:</b> Normalization is the process of organizing data to minimize redundancy and improve integrity. It helps eliminate update, insertion, and deletion anomalies while making the database easier to maintain.
</details>

<details>
<summary><b>Q7. A developer stores customer names and phone numbers in every order record instead of referencing a Customers table. Which database design principle is being violated, and what issues can arise?</b></summary>
<br>
<b>A7:</b> The principle of minimizing redundancy is being violated. Repeating customer details in every order wastes storage, increases update effort, and risks inconsistencies when customer information changes but not all records are updated.
</details>
