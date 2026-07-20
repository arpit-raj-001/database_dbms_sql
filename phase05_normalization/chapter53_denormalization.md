<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔙 What is Denormalization?</h1>
  <h2>Chapter 53</h2>
</div>

---

## 📖 Definition

> [!NOTE]
> **Denormalization** is the process of intentionally adding redundancy to a normalized database to improve query performance, especially for read-heavy workloads.

---

## 🏢 Example

### Normalized Design

**Student**
- `StudentID`
- `Name`
- `DepartmentID`

**Department**
- `DepartmentID`
- `DepartmentName`
- `HOD`

To display a student with their department:
```sql
SELECT Student.Name, Department.DepartmentName
FROM Student
JOIN Department
ON Student.DepartmentID = Department.DepartmentID;
```
*Every query requires a JOIN.*

### Denormalized Design

**Student**
- `StudentID`
- `Name`
- `DepartmentID`
- `DepartmentName` *(Added Redundancy!)*

Now:
```sql
SELECT Name, DepartmentName
FROM Student;
```
> [!TIP]
> No JOIN needed. Much faster for reading.

---

## 🐢 JOIN operations become expensive as databases grow.

Large companies often have:
- Billions of rows
- Hundreds of tables
- Thousands of queries per second

Sometimes performing multiple JOINs becomes slower than storing duplicate data.

Suppose every time aadz opens the college portal, the application executes:
- Student
- `JOIN` Department
- `JOIN` Faculty
- `JOIN` Semester
- `JOIN` Classroom
- `JOIN` Attendance
- `JOIN` Grades 

*Six joins for every request. Millions of users. Very expensive.*
**Instead, the system may store frequently accessed information together.**

---

## 📖 Read-Heavy Systems vs Write-Heavy Systems

### Read-Heavy Systems
Some systems perform:
- 99% reads
- 1% writes

**Examples:** News websites, Netflix homepage, Amazon product pages.
*These benefit greatly from denormalization.*

### Write-Heavy Systems
Some systems perform frequent updates.
**Examples:** Banking, Payment systems, Airline booking, Inventory management.
*These usually remain normalized because duplicate data makes updates difficult.*

---

## 🗄️ Materialized Views & Data Warehouses

### Materialized Views
**Definition:** A Materialized View stores the result of a query physically. Unlike a normal view, the result is already computed.

**Normal View:** Every query recomputes the result.
**Materialized View:** Stores the result physically. No computation needed during reads.
*Used in:* Analytics, Dashboards, Reporting.

### Data Warehouses
Data warehouses are mostly:
- Read-only
- Historical
- Huge
*Denormalization is extremely common (e.g., Star Schema).*

---

## ⚖️ Advantages vs Disadvantages

| ✅ Advantages of Denormalization | ❌ Disadvantages of Denormalization |
| :--- | :--- |
| **1. Faster Reads:** Fewer joins. Lower execution time. | **1. Data Redundancy:** Duplicate data everywhere. |
| **2. Fewer JOIN Operations:** Most queries become simpler. | **2. Update Anomalies:** Updating thousands of rows instead of one. |
| **3. Better Reporting:** Reports execute much faster. | **3. Storage Overhead:** Consumes more disk space. |
| **4. Simpler Queries:** Simple SELECT instead of multi-JOIN. | **4. Inconsistent Data:** Copies might get out of sync. |
| **5. Lower Query Latency:** Crucial for Web apps/Mobile apps. | **5. Complex Updates:** Harder to keep copies synchronized. |

---

## ❓ FAQs

<details>
<summary><b>Can OLTP systems use denormalization?</b></summary>
<br>
<b>Answer: Yes, but carefully.</b><br>
Most OLTP systems are primarily normalized (often up to 3NF). However, they may selectively denormalize specific tables or add precomputed columns, summary tables, or caches for performance-critical queries.
</details>
