<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗑️ Database Deletion Operations</h1>
  <h2>Chapter 59</h2>
</div>

---

Suppose you have this table:
**Student**
| ID | Name | CGPA |
| :--- | :--- | :--- |
| `1` | `aadz` | `7.97` |
| `2` | `Arpit` | `7.70` |
| `3` | `Bhaar` | `8.92` |

**Now imagine different situations:**

### Situation 1
The college says: *"Remove Bhaar only."*
**Use:** `DELETE`

### Situation 2
The college says: *"New semester starts. Remove every student record. Keep the table."*
**Use:** `TRUNCATE`

### Situation 3
The college says: *"Student management system no longer exists. Remove the table itself."*
**Use:** `DROP TABLE`

### Situation 4
The college closes forever. Remove the whole database.
**Use:** `DROP DATABASE`

---

## ❌ DELETE

`DELETE` removes rows from a table. The table itself remains.

**Syntax:**
```sql
DELETE FROM Student;
DELETE FROM Student WHERE ID = 2;
```

**What happens?**
- Deletes every row (or matching rows).
- The table structure stays.
- Columns stay.
- Indexes stay.
- Constraints stay.
- Only data disappears.

### Internal Working
When MySQL executes `DELETE FROM Student WHERE ID=2;`, it generally:
- Finds matching rows.
- Marks those rows for deletion.
- Records changes in transactional logs (InnoDB).
- Updates indexes.
- Frees space for future use.

### Logging
`DELETE` is fully logged in InnoDB. Every deleted row is recorded so the operation can be rolled back if it hasn't been committed.

**Rollback Example:**
```sql
START TRANSACTION;
DELETE FROM Student WHERE ID=2;
ROLLBACK;
```

> [!WARNING]
> `DELETE` **does not** reset the `AUTO_INCREMENT` counter.
> If you have IDs 1, 2, 3 and delete 3, the next insert becomes 4.

---

## 🧹 TRUNCATE TABLE

`TRUNCATE` removes all rows.

**Syntax:**
```sql
TRUNCATE TABLE Student;
```

- The table still exists.
- For InnoDB, MySQL implements `TRUNCATE TABLE` by effectively dropping and recreating the table internally, which makes it very fast for removing all rows.
- The table definition remains, but all data is removed.

> [!TIP]
> Unlike `DELETE`, `TRUNCATE` does not log each deleted row individually. It logs the metadata operation instead. This is one reason it's much faster for clearing an entire table.

> [!IMPORTANT]
> `TRUNCATE` **resets** the `AUTO_INCREMENT` counter.
> If IDs are 1, 2, 3 and you run `TRUNCATE TABLE Student;`, the next insert becomes 1.

**Rollback & Foreign Keys:**
```sql
START TRANSACTION;
TRUNCATE TABLE Student;
ROLLBACK;
```
The rollback **does not** restore the data. Unlike `DELETE`, `TRUNCATE` cannot be rolled back in the usual transactional sense.

Also, if another table references `Student` with a foreign key, `TRUNCATE` may fail. MySQL prevents truncating a table that is referenced by a foreign key.

---

## 💥 DROP TABLE

`DROP` removes:
- Data
- Table structure
- Indexes
- Constraints
- Triggers associated with the table
*Everything.*

### Internal Working
MySQL:
- Removes metadata.
- Deletes the table definition.
- Releases associated storage.
- Removes indexes and constraints tied to the table.

> [!WARNING]
> In normal MySQL usage, it cannot be rolled back. No table exists anymore.

---

## 📊 Comparison Table

| Feature | DELETE | TRUNCATE |
| :--- | :--- | :--- |
| **Rows** | Removes selected or all rows | Removes all rows only |
| **WHERE Clause**| Supports `WHERE` | No `WHERE` |
| **Operation Type**| Row-by-row operation | Table-wide operation |
| **Logging** | Fully logged row deletions | Metadata-level operation |
| **Rollback?** | Can usually be rolled back | Implicit commit; not normally rollbackable |
| **Auto-Increment**| Does **not** reset `AUTO_INCREMENT`| **Resets** `AUTO_INCREMENT` |
| **Speed** | Generally slower for large tables | Generally much faster |

### Which is fastest?
Generally: **DROP > TRUNCATE > DELETE**

### Which can be rolled back?
- **DELETE** → Yes, before `COMMIT` in a transaction.
- **TRUNCATE** → No *(implicit commit in MySQL)*.
- **DROP** → No *(implicit commit in MySQL)*.
