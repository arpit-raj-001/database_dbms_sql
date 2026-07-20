<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧩 Database Core Concepts</h1>
  <h2>Chapter 16</h2>
</div>

---

## 🗣️ In SQL Terminology:

| Relational Theory Term | SQL Term |
| :--- | :--- |
| Relation | **Table** |
| Tuple | **Row** |
| Attribute | **Column** |
| Cell | **Individual Value** |

> [!NOTE]
> A **table** is a logical database object consisting of rows and columns that stores information about a particular entity or concept.

---

## 🪞 Logical Table vs Physical Table

### Logical Table
This is how users think about the data. You simply see rows and columns.
**You don't care:**
- Where it's stored.
- How it's stored.

### Physical Table
Internally, the DBMS stores the same data using:
- 📄 Pages
- 🧱 Blocks
- 📝 Records
- 🏊 Buffer pool
- 🌳 B+ Trees
- 🗄️ Heap files

---

## 🏃 Execution Perspective

When you execute:
```sql
SELECT * FROM Students;
```

**You think:**
> *"Read the Students table."*

**The DBMS thinks:**
> *"Locate the relevant pages, choose an index scan or sequential scan, fetch records into memory, apply predicates, and return the result."*

---

## 📖 Definitions: Rows, Columns, and Cells

### What is a Row?
A row represents one complete record. A **tuple** is a single element of a relation consisting of one value for every attribute. An ordered collection of attribute values is a tuple. When we say tuples are unordered it means there is no imposition of order on its rows.

### What is a Column?
A column stores one type of information for all records. An **attribute** is a property or characteristic of an entity represented within a relation.

**Example: Student entity Properties:**
- StudentID
- Name
- CGPA
- Email

*(Every attribute has a name, domain, data type, meaning.)*

### What is a Cell?
A cell is the smallest logical unit of data in a relation and contains exactly **one value** for a specific tuple and attribute. It must contain one atomic value so that the relation satisfies the requirements of the relational model and First Normal Form (1NF).

---

## ⚖️ Theory vs. Implementation

> [!IMPORTANT]
> **Tuples and attributes** are relational theory terms.
> **Rows and columns** are SQL implementation terms and represent the actual table stored in the DBMS.

Relations are defined as sets of tuples, and sets are inherently unordered. Therefore, the sequence in which tuples appear has no logical meaning. SQL guarantees a specific row order **only** when an `ORDER BY` clause is used.
