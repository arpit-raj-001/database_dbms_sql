<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧹 Why Normalization Exists</h1>
  <h2>Chapter 40</h2>
</div>

---

Before normalization existed, databases often stored the same information repeatedly.
**This led to:**
- 🗑️ Wasted storage
- ❌ Incorrect updates
- 🔎 Missing information
- 📉 Inconsistent data
- 🛠️ Difficult maintenance

> [!NOTE]
> **Normalization** provides a systematic process for organizing data so that:
> - Each fact is stored only once.
> - Relationships between data are maintained correctly.
> - Redundancy is minimized.
> - Data remains consistent and reliable.

---

## 🔁 What is Redundancy?

> [!NOTE]
> **Definition:** Redundancy is the unnecessary duplication of the same data in multiple places within a database.

### Example
| Student | Department | HOD |
| :--- | :--- | :--- |
| `Aadz` | `CSE` | `Dr. Sharma` |
| `arpit` | `CSE` | `Dr. Sharma` |
| `aadz` | `CSE` | `Dr. Sharma` |
| `aadz raz` | `ECE` | `Dr. Mehta` |

**Why is it a problem?**
Suppose Dr. Sharma is replaced by Dr. Singh. You now need to update **every** CSE student's record.

**Redundancy causes:**
- Wasted Storage
- More Updates
- Slower Maintenance
- Inconsistent Information

---

## 🚨 Data Anomalies

### A. Update Anomaly
**Definition:** Updating one fact requires updating multiple rows.

### B. Insertion Anomaly
**Definition:** You cannot insert certain information without inserting unrelated information.

**Example:**
Suppose a new department is created: `AI Department`, `HOD = Dr. Kapoor`.
But no students have joined yet.
If the table only stores department details along with students, you cannot record the new department. This is an Insertion Anomaly.

### C. Deletion Anomaly
**Definition:** Deleting one piece of data unintentionally deletes other important information.

**Example:**
| Student | Department | HOD |
| :--- | :--- | :--- |
| `Aadz` | `AI` | `Dr. Kapoor` |

If Aadz graduates and her row is deleted:
```sql
DELETE FROM Student; 
```
> [!WARNING]
> The only information about the AI department is also lost! This is a **Deletion Anomaly**.

---

## 🛡️ Database Consistency & Integrity

### Database Consistency
**Definition:** Consistency means the database always represents the same real-world truth. Normalization stores each fact once, making it easier to keep the database consistent.

### Data Integrity
**Definition:** Data integrity is the accuracy, correctness, and reliability of the data stored in a database.

**Types of Integrity:**
- **Entity Integrity:** Every row has a unique primary key.
- **Referential Integrity:** Foreign keys reference valid rows.
- **Domain Integrity:** Values follow valid formats.
- **User-defined Integrity:** Custom business rules.

*Normalization supports integrity by reducing duplication and making constraints easier to enforce.*

---

## 💾 Storage Efficiency

Suppose there are:
- `100,000` students
- `50` departments

**Without normalization:**
Each student stores: `Department Name`, `HOD`, `Building`, `Office Number`
This information is repeated 100,000 times!

---

## 🎯 Goals of Normalization

Normalization aims to:
- Reduce redundancy.
- Eliminate anomalies.
- Improve consistency.
- Improve integrity.
- Simplify updates.
- Save storage.
- Make maintenance easier.

### Is redundancy always bad?

> [!TIP]
> **Answer:** No.
> 
> In **OLTP systems**, redundancy is generally minimized through normalization. However, in **data warehouses**, reporting systems, analytics platforms, and caching layers, controlled redundancy (called **denormalization**) is intentionally introduced to improve read performance. We'll study this in Chapter 53 — Denormalization.
