<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔄 UPSERT</h1>
  <h2>Chapter 73</h2>
</div>

---

UPSERT is asked very frequently in backend and database interviews because it's used in almost every production application.

---

## 🤔 What is UPSERT?

> [!NOTE]
> **UPSERT = UPDATE + INSERT**

**It means:**
- If the row doesn't exist → **INSERT** it.
- If the row already exists → **UPDATE** it.

**Instead of writing:**
```sql
IF EXISTS
 UPDATE
ELSE
 INSERT
```
SQL provides a single statement.

---

### Suppose
**Students Table**
| StudentID | Name |
| :--- | :--- |
| `1` | `aadz` |

**UPSERT logic:**
```text
Already exists?
       ↓
      Yes
       ↓
   Update row
       ↓
    No error
```

---

## 🐘 INSERT ... ON CONFLICT (PostgreSQL)

```sql
INSERT INTO Students
VALUES (1, 'Arpit')
ON CONFLICT(ID)
DO UPDATE
SET Name='Arpit'
```

---

## 🐬 ON DUPLICATE KEY UPDATE (MySQL)

```sql
INSERT INTO Students(StudentID, Name)
VALUES (1, 'Arpit')
ON DUPLICATE KEY UPDATE
Name='Arpit';
```

---

## 📝 Updating Using the Inserted Values

**Suppose Products Table:**
| ID | Stock |
| :--- | :--- |
| `1` | `10` | *(Existing)* |

**Shipment:** 15 (Increase stock)

```sql
INSERT INTO Products(ProductID, Stock)
VALUES (1, 15)
ON DUPLICATE KEY UPDATE
Stock = Stock + VALUES(Stock);
```

---

## 🛑 DO NOTHING

Sometimes you don't want to update. You simply want to **ignore duplicates**.

**PostgreSQL:**
```sql
INSERT INTO Students
VALUES(1, 'aadz')
ON CONFLICT(ID)
DO NOTHING;
```

**Equivalent MySQL idea:**
```sql
INSERT IGNORE INTO Students
VALUES(1, 'aadz');
```

---

## 🔄 DO UPDATE

Instead of ignoring, Update.

**PostgreSQL:**
```sql
ON CONFLICT(ID)
DO UPDATE
SET Name='aadz';
```

**MySQL:**
```sql
ON DUPLICATE KEY UPDATE
Name='aadz';
```

---

## ⚠️ When Conflicts Happen

UPSERT only works when a conflict can occur.
**Conflicts happen on:**
- **Primary Key** (e.g., `StudentID`)
- **Unique Key** (e.g., `Email`)
- **Composite Key** (e.g., `StudentID, CourseID`)

**Example:**
```sql
INSERT INTO Users(UserID, LoginCount)
VALUES(10, 1)
ON DUPLICATE KEY UPDATE
LoginCount=LoginCount+1;
```

---

## 📚 UPSERT Multiple Rows

```sql
INSERT INTO Students
VALUES
(1, 'aadz'),
(2, 'Arpit'),
(3, 'bhaar')
ON DUPLICATE KEY UPDATE
Name=VALUES(Name);
```
Each row is checked independently. Some may insert. Some may update.

---

## 🏢 Real-World Examples

### Amazon
**Shopping cart:**
Product already exists?
- Yes → Increase quantity
- Else → Insert product

### WhatsApp
**Contact already exists?**
- Yes → Update
- Else → Insert

---

## 📋 Summary

### Why use UPSERT instead of SELECT + INSERT/UPDATE?
- Fewer queries
- Atomic
- Better performance

### MySQL syntax?
```sql
INSERT INTO table(...)
VALUES(...)
ON DUPLICATE KEY UPDATE
column = value;
```

### PostgreSQL syntax?
```sql
INSERT ...
ON CONFLICT (...)
DO UPDATE;
```

### SQL Server equivalent?
`MERGE`
