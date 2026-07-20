Authored by : Arpit Raj , Lnmiit jaipur

# Ch-4 Data

## Core Concepts

**data** : collection of fact, values, observations that represent events or attribute but do not convey meaning

**Information** : processed, organized and contextualized data which conveys meaning

*Eg.*

| Stud ID | Name | City |
| :--- | :--- | :--- |
| 006 | Aadz | Gwalior |

**Knowledge** : Information combined with rules, patterns, or interpretations that enables decisions

**Data → Information → Knowledge**

---

## Metadata

**Metadata** : basically data about data, it provides context about other data

**It tells us →**
• what the data means
• how it's stored
• what type it is
• constraints
• relations
• ownership

> *Eg.* Table box: `ID: 006 | Name: Aadz` (Data)
> 
> **Metadata:** Table name, Columns, primary key, Data types of columns
> 
> every DBMS maintains metadata describing schemas, tables, columns, indexes, constraints, permissions ;
> 
> metadata is stored in systems catalog

> **CREATE TABLE students** → the actual data
> 
> DBMS stores actual rows and metadata like students, columns, data type, constraints, privileges, indexes, storage info
> 
> **hence [ user data + metadata ]**

---

## Data

| Structured | semi structured | unstructured |
| :--- | :--- | :--- |
| • fixed schema<br>• rows, columns<br>• SQL friendly<br>• stored in relational database | • some organization<br>• flexible schemas<br>• diff documents may have diff fields<br>• stored in MongoDB | • no schema<br>• stored outside relational DB with metadata stored in DB<br>• Eg. images/video etc |

---

## Practice Questions

**Q1. Differentiate between Data, Information, and Knowledge.**
A1. Data consists of raw facts or values without inherent context (e.g., 21). Information is processed and contextualized data that conveys meaning (e.g., Arpit is 21 years old). Knowledge is information combined with experience or reasoning to support decisions (e.g., recognizing seasonal sales patterns and adjusting inventory).

**Q2. What is metadata? Give examples stored by a DBMS.**
A2. Metadata is descriptive information about data. It defines the structure, meaning, constraints, and organization of stored data.
Examples include: Table names, Column names, Data types, Primary and foreign keys, Indexes, Constraints, Views, User privileges, Storage information.

**Q3. Why is metadata necessary for a database?**
A3. Metadata allows the DBMS to understand how data is organized and how it should be interpreted. It enables schema validation, query optimization, constraint enforcement, security, and storage management. Without metadata, the DBMS cannot correctly process user queries.

**Q4. Differentiate between structured, semi-structured, and unstructured data. Give one example of each.**
A4.
- Structured Data: Fixed schema, organized into rows and columns (e.g., an Employees table).
- Semi-Structured Data: Flexible schema with self-describing fields (e.g., JSON documents).
- Unstructured Data: No predefined schema (e.g., images, videos, PDFs).

**Q5. Why do applications work with logical data instead of physical storage structures?**
A5. Applications interact with logical data because it abstracts away low-level storage details. The DBMS maps logical structures to physical storage, providing physical data independence, improving portability, and allowing storage optimizations without changing application code.

**Q6. Suppose the DBMS loses all metadata but still has the raw table pages. Can it correctly interpret the stored data? Why or why not?**
A6. No. Without metadata, the DBMS loses the description of the stored bytes. It would not know: Where records begin and end, Column boundaries, Data types, Constraints, Relationships, Index definitions. The raw bytes may still exist on disk, but they cannot be interpreted correctly without the associated metadata. This is why protecting the system catalog (data dictionary) is critical for any DBMS.
