<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>💾 SQL Binary Data and Datatypes</h1>
  <h2>Chapter 64</h2>
</div>

---

## 🧩 What is Binary Data?

Everything inside a computer is ultimately stored as bits (0s and 1s).

**Examples:**
- Images (`.jpg`, `.png`)
- PDFs
- Audio (`.mp3`)
- Videos (`.mp4`)
- ZIP files
- Executable files (`.exe`)

These are **binary files**, not text.
For example, the first few bytes of a PNG image are:
`89 50 4E 47 0D 0A 1A 0A`
This is binary data, not readable text.

---

## 🔡 Character vs Binary Data

Suppose we store: `aadz`
Internally (ASCII): `97 97 100 122`
That's still text.

Now an image: `FF D8 FF E0 00 10 ...`
Those bytes don't represent readable characters. Hence SQL provides binary datatypes.

---

## 🗂️ Binary Datatypes

| Datatype | Purpose |
| :--- | :--- |
| **BINARY** | Fixed-length binary |
| **VARBINARY** | Variable-length binary |
| **TINYBLOB** | Small binary objects |
| **BLOB** | Medium binary objects |
| **MEDIUMBLOB** | Large binary objects |
| **LONGBLOB** | Very large binary objects |

---

### HashValue BINARY(16)

Suppose `ABC`.
**Storage:** `ABC + remaining bytes padded with 0x00`.
> [!NOTE]
> Exactly like `CHAR` pads spaces, `BINARY` pads with zero bytes.

### VARBINARY

Like `VARCHAR`. Stores variable-length binary data.
**Syntax:** `Hash VARBINARY(64)`

Now:
- 5 bytes stores 5 bytes.
- 40 bytes stores 40 bytes.
*Only the actual bytes plus a small length prefix are stored.*

---

## 📦 BLOB Family

### BLOB
BLOB stands for **Binary Large Object**. Used for storing large binary data.
**Syntax:** `Resume BLOB`
**Maximum Size:** `65,535 bytes ≈ 64 KB`
**Uses:** PDFs, Small images, Binary documents.

### TINYBLOB
Smallest BLOB.
**Maximum:** `255 bytes`
**Uses:** Tiny binary values. Rarely used.

### MEDIUMBLOB
**Maximum:** `16,777,215 bytes ≈ 16 MB`
**Uses:** Images, Medium videos, Audio, Documents.

### LONGBLOB
Largest binary datatype.
**Maximum:** `4,294,967,295 bytes ≈ 4 GB`
**Uses:** Movies, Large videos, Medical imaging, Archives.

---

## 🧠 Internal Storage

> [!TIP]
> Large BLOB values are usually stored **outside** the main data row (especially in InnoDB), with the row containing a pointer to the external data.
> This keeps rows reasonably compact while allowing storage of very large binary objects.

---

## ⚖️ Advantages & Disadvantages

| ✅ Advantages | ❌ Disadvantages |
| :--- | :--- |
| • Stores any binary file | • Large storage |
| • Supports very large objects | • Slower backups |
| • No manual encoding required | • Higher replication cost |
| | • Larger indexes (if indexing prefixes) |
| | • More memory and disk I/O |

---

## 🏫 Real Examples

**Student Database:**
```sql
CREATE TABLE Student(
 StudentID INT AUTO_INCREMENT PRIMARY KEY,
 Name VARCHAR(100),
 Email VARCHAR(255),
 DOB DATE,
 CGPA DECIMAL(3,2),
 Gender ENUM('Male', 'Female', 'Other'),
 Hosteller BOOLEAN
);
```

**Why these column types?**
- `StudentID` → `INT` is sufficient.
- `Name`, `Email` → Variable length (`VARCHAR`).
- `DOB` → Date only (`DATE`).
- `CGPA` → Exact decimal values (`DECIMAL`).
- `Gender` → Limited choices (`ENUM`).
- `Hosteller` → Yes/No (`BOOLEAN`).

---

## 🌳 Master Decision Tree

```text
Need Numbers?
│
├── Integer?
│   ├── Small → TINYINT / SMALLINT
│   ├── Medium → INT
│   └── Huge → BIGINT
│
├── Decimal?
│   ├── Money → DECIMAL
│   └── Scientific → DOUBLE
│
Need Text?
│
├── Fixed Length → CHAR
├── Variable Length → VARCHAR
└── Large Text → TEXT/LONGTEXT
│
Need Date?
│
├── Birthday → DATE
├── Time Only → TIME
├── Event → DATETIME
└── Logs → TIMESTAMP
│
Need Binary?
│
├── Fixed → BINARY
├── Variable → VARBINARY
└── Large Files → BLOB
│
Need Flexible Data?
│
└── JSON
│
Need Fixed Choices?
│
├── ENUM
└── SET (MySQL only)
```
