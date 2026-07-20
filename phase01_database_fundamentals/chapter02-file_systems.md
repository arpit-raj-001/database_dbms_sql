<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📂 File Systems in Detail</h1>
  <h2>Chapter 2</h2>
</div>

---

> [!IMPORTANT]
> - A **DBMS** is built on top of a file system.
> - A **file** is a collection of bytes stored on secondary storage and managed by the OS.

## 🛠️ The File System
A file system is a component of the OS responsible for organizing and managing files. Its capabilities include:
- `Create` / `Delete` / `Rename` files
- `Read` / `Write` bytes
- Manage directories / permissions at the file level

### 🌊 Flow of Data

```mermaid
graph LR
    A[Application] -->|read| B(OS)
    B --> C{File System}
    C --> D[(Disk)]
```

> [!NOTE]
> The **application itself** must parse CSVs, search, update records, and validate data. The OS does **none** of this.

---

## 📝 Text Files

| ✅ PROS | ❌ CONS |
| :--- | :--- |
| • Easy debugging<br>• Portable<br>• Platform independent | • No schema<br>• No indexing<br>• Slow process |

## 📊 CSV Files *(Comma Separated Values)*

| ✅ PROS | ❌ CONS |
| :--- | :--- |
| • Easy import/export<br>• Excel supported | • No indexing<br>• No constraint<br>• No relation<br>• No transaction |

## 📜 JSON Files *(Semi-Structured Data)*

| ✅ PROS | ❌ CONS |
| :--- | :--- |
| • Flexible schema<br>• Easy to exchange over APIs<br>• Supports nested objects | • No transaction<br>• No concurrency<br>• Searching needs scanning<br>• Large dataset is inefficient |

---

## 🥊 File System vs DBMS

| Feature | 📂 File System | 🗄️ DBMS |
| :--- | :---: | :---: |
| **Stores bytes** | ✔️ | ✔️ |
| **Query language** | ❌ | ✔️ |
| **Indexing** | ❌ | ✔️ |
| **Transaction** | ❌ | ✔️ |
| **Concurrency control**| ❌ | ✔️ |
| **Recovery** | ❌ | ✔️ |
| **Integrity** | ❌ | ✔️ |
| **Query optimization** | ❌ | ✔️ |
| **Security** | ❌ | ✔️ |

---

## ⏱️ Sequential Access vs Random Access

| 🛤️ Sequential Access | 🎯 Random Access |
| :--- | :--- |
| • **O(n)** time complexity<br>• Looks for data sequentially (checks all records)<br>• Good for logs, large sequential reads, streaming | • **O(1)** time complexity<br>• Jumps directly to required location using an index/offset<br>• Uses **B+ trees** and **hash indices** |
