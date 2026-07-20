<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📜 What is JSON?</h1>
  <h2>Chapter 62</h2>
</div>

---

**JSON** stands for **JavaScript Object Notation**.
Despite the name, it is language-independent. Today almost every application exchanges data using JSON.

**Example:**
```json
{
 "name": "aadz",
 "age": 20,
 "cgpa": 7.97,
 "ethereal": true
}
```

Suppose you're storing user preferences. Different users may have different settings.

**User A:**
```json
{
 "theme": "dark",
 "language": "English"
}
```

**User B:**
```json
{
 "theme": "light",
 "fontSize": 18,
 "notifications": false,
 "language": "Hindi"
}
```

> [!TIP]
> Creating separate columns for every possible preference would be impractical. JSON allows flexible storage.

---

## 🛠️ JSON Data Type

MySQL provides a native `JSON` datatype.

**Example:**
```sql
CREATE TABLE Users(
 UserID INT PRIMARY KEY,
 Preferences JSON
);
```

**Insert:**
```sql
INSERT INTO Users VALUES
(1, '{
 "theme":"dark",
 "language":"English"
}'
);
```

### Internal Working
MySQL stores JSON in an optimized binary format internally.
**Benefits:**
- Faster retrieval.
- Faster parsing.
- Automatic validation.
- More efficient storage than plain text.

---

## 📐 JSON Rules
- **Keys:** are strings, must be enclosed in double quotes.
- **Values may be:** string, number, Boolean, null, object, array.
- Arrays may also contain objects.

**Example:**
```json
{
 "projects":[
 {
 "name":"Hospital System",
 "year":2025
 },
 {
 "name":"MediSync",
 "year":2026
 }
 ]
}
```

### Nested JSON
Objects can contain other objects.
**Example:**
```json
{
 "name":"aadz",
 "address":{
 "city":"Gwalior",
 "state":"Madhya pradesh",
 "country":"India"
 }
}
```
> [!NOTE]
> MySQL stores JSON efficiently in binary form. You don't need to manually serialize or deserialize it when storing or retrieving.

---

## 🔍 Querying JSON

One powerful feature of MySQL is querying values inside JSON documents.

Suppose `Preferences` stores:
```json
{
 "theme":"dark",
 "language":"English"
}
```

**Retrieve theme:**
```sql
SELECT JSON_EXTRACT(Preferences, '$.theme') FROM Users;
```
**Output:** `"dark"`

### `$.theme` means:
- `$` → root of the JSON document.
- `.theme` → value of the theme key.

**Another example:**
```sql
SELECT JSON_EXTRACT(Preferences, '$.address.city') FROM Users;
```

> [!TIP]
> You can also use the shorthand operators in MySQL:
> ```sql
> SELECT Preferences->>'$.theme' FROM Users;
> ```
> *Returns the unquoted SQL string (`dark` instead of `"dark"`).*

---

## ⚡ Indexing Basics

Suppose your application frequently searches by `theme` inside JSON.
Without indexing, MySQL scans every row.

Instead, you can create a generated column that extracts the JSON value and then index that generated column.

**Example:**
```sql
ALTER TABLE Users ADD COLUMN Theme VARCHAR(20)
GENERATED ALWAYS AS (Preferences->>'$.theme') STORED;

CREATE INDEX idx_theme ON Users(Theme);
```

---

## 🌐 API Responses

Many REST APIs return JSON.

**Example:**
```json
{
 "status":"success",
 "data":{
 "id":1,
 "name":"aadz"
 }
}
```
Storing the raw response as JSON can be useful for debugging or auditing.

---

## ⚖️ Advantages & Disadvantages

| ✅ Advantages | ❌ Disadvantages |
| :--- | :--- |
| • Flexible schema | • Complex JSON queries can be slower than simple column lookups |
| • Built-in validation | • Excessive use can make data harder to analyze |
| • Efficient binary storage | |
| • Easy integration with web APIs | |
| • Supports nested structures | |
| • Powerful querying functions | |

---

## ⚔️ JSON vs Normalized Tables

### Normalized Design
**Student:** `1 | aadz`
**Preferences:** `1 | Dark | English`

### JSON Design
**Student:** `1 | aadz | {"theme":"dark","language":"English"}`

**Which is better? Depends.**

> [!TIP]
> **Use JSON when:**
> - attributes vary between rows,
> - relationships aren't important.
> 
> **Use normalized tables when:**
> - relationships matter,
> - joins are common,
> - data must be consistent.
> 
> **Avoid JSON when:**
> - the data has clear relational structure,
> - you frequently join, group, or aggregate on the fields.
