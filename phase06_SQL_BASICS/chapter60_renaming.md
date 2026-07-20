<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🏷️ Renaming Tables and Indexes</h1>
  <h2>Chapter 60</h2>
</div>

---

## 🔄 RENAME TABLE

> [!NOTE]
> **Internal Working:**
> When MySQL renames a table, it does not copy all the data. Instead, it updates the metadata (data dictionary) so that the table has a new name. This is why renaming is generally much faster than recreating a table and copying data.

**Example:**
```sql
RENAME TABLE Student TO Students;
```

**Internal Working (MySQL):**
- Checks permissions.
- Verifies the new name doesn't already exist.
- Updates metadata.
- Releases locks.
*(No row data is modified).*

---

## 🔁 ALTER TABLE ... RENAME

Another way.

**Syntax:**
```sql
ALTER TABLE Student
RENAME TO Students;
```

---

## ✏️ Rename Columns

**Modern Syntax:**
```sql
ALTER TABLE Student
RENAME COLUMN Name TO FullName;
```

**Older syntax:**
```sql
ALTER TABLE Student
CHANGE COLUMN Name FullName VARCHAR(100);
```

---

## 📛 How to Rename a Database

MySQL **does not support** `RENAME DATABASE` because it could lead to corruption and other issues.

**The typical process is:**
1. Create new database
2. Move tables *(using tools like `mysqldump`)*
3. Verify data
4. Drop old database: `DROP DATABASE College;`

---

## 📇 Renaming an Index

Modern MySQL supports:

```sql
ALTER TABLE Student
RENAME INDEX idx_old TO idx_new;
```
