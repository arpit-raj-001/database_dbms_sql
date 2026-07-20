<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🗄️ Types of Databases</h1>
  <h2>Chapter 10</h2>
</div>

---

> [!NOTE]
> *Mongo DB, Redis, Cassandra won't be heavily discussed here. They come later in Phase 26 (NoSQL).*

Different applications have radically different requirements:
- Structured data
- Fast caching
- Complex relations
- Time-based measures
- AI embeddings

⟶ **Hence, different database models evolved.**

---

### 1️⃣ Relational Database
*Examples: MySQL, PostgreSQL, Oracle Database*

Data is stored in tables. Rows are called tuples, columns are called attributes. Relations are maintained using foreign keys.

- **FEATURES:** Structured schema, SQL support, ACID transactions, Joins, Normalization, Constraints.
- **CHARACTERISTICS:** First choice when data integrity is critical. Used in Banking, E-commerce, ERP, inventory, hospital systems, etc.
- ❌ **CONS:** Less flexible schema, horizontal scaling is challenging.

---

### 2️⃣ Document Database
*Examples: MongoDB, CouchDB*

Stores data as documents (typically JSON or BSON). Each document can have a different structure.

```json
{
  "studentId" : 006,
  "name" : "Aadz",
  "Skills" : [ "VLSI", "RISC-V", "C" ]
}
```

- **CHARACTERISTICS:** Flexible schema, nested objects, JSON-based, easy to evolve.
- **BEST FOR:** User profiles, blogging platforms, rapidly changing applications.
- ❌ **CONS:** Data duplication is common, complex joins are weaker than relational DBs.

---

### 3️⃣ Key-Value Database
*Examples: Redis, Amazon DynamoDB*

A giant hash map where we ask for a key and we get its value. Memory-based, extremely fast lookups, simple model.

- **BEST FOR:** Caching, sessions, rate-limiting, shopping carts, leaderboards.
- ✅ **PROS:** Simple operations, extremely low latency, high throughput.
- ❌ **CONS:** No joins, limited querying capability.

---

### 4️⃣ Wide Column Database
*Examples: Apache Cassandra, Apache HBase*

Optimized for large-scale distributed workloads. Unlike relational DBs, rows can have entirely different columns. Massive scalability, distributed by design, handles huge datasets.

| Roll | Name | email | phone |
| :--- | :--- | :--- | :--- |
| `006` | `Aadz` | `NULL` | `79870...` |
| `012` | `Arpit` | `hehe@gmail.com` | `NULL` |

- **BEST FOR:** Logging, social media activity feeds, telecom, IoT.
- ✅ **PROS:** Excellent horizontal scaling, high throughput, fault tolerant.
- ❌ **CONS:** Complex, very few relational features.

---

### 5️⃣ Graph Database
*Examples: Neo4j, Amazon Neptune*

Stores nodes and relationships (edges) instead of tables. Relationships are stored directly as first-class citizens.
**Example:** `A` ━━━━ **Friends** ━━━━ `B`

- **BEST FOR:** Knowledge graphs, recommendation systems, fraud detection, network analysis.
- ✅ **PROS:** Efficient relationship traversal, natural representation of connected data.
- ❌ **CONS:** Not ideal for traditional transactional business applications.

---

### 6️⃣ Time-Series Database
*Examples: InfluxDB, TimescaleDB*

Stores data ordered strictly by time (e.g., Timestamp vs Temperature).

- **BEST FOR:** Sensors, IoT, server monitoring, metrics collection.
- ✅ **PROS:** Efficient time-based queries, heavy data compression.
- ❌ **CONS:** Specialized for temporal workloads rather than general purpose.

---

### 7️⃣ Vector Database
*Examples: Pinecone, Weaviate*

Instead of storing exact rows, it stores embeddings (high dimensional arrays) and enables similarity search.

- **BEST FOR:** AI chatbots, RAG (Retrieval-Augmented Generation), recommendation systems, semantic search.
- ✅ **PROS:** Semantic retrieval, extremely fast nearest-neighbor search.
- ❌ **CONS:** Not a replacement for a main DB; used alongside SQL.

---

## 📝 Practice Questions

<details>
<summary><b>Q1. Why do different types of databases exist instead of one universal database?</b></summary>
<br>
<b>A1:</b> Different database types exist because workloads differ significantly. Transaction-heavy systems require strong consistency and relational modeling, while caching, graph traversal, time-based analytics, or AI similarity search each benefit from specialized storage models and query mechanisms.
</details>

<details>
<summary><b>Q2. Compare relational and document databases. Give one use case where each is the better choice.</b></summary>
<br>
<b>A2:</b> A relational database stores structured data in tables with predefined schemas and is ideal for applications such as banking or order processing where ACID transactions and joins are essential. A document database stores flexible JSON-like documents, making it better suited for evolving user profiles, content management systems, or product catalogs.
</details>

<details>
<summary><b>Q3. A company needs to store millions of user login sessions with very fast retrieval by session ID. Which database type would you recommend and why?</b></summary>
<br>
<b>A3:</b> A key-value database is the best choice. Login sessions are naturally modeled as <code>session_id</code> → <code>session_data</code> mappings, requiring extremely fast reads and writes without complex relational queries.
</details>

<details>
<summary><b>Q4. Why are graph databases well suited for social networking applications?</b></summary>
<br>
<b>A4:</b> Graph databases store entities as nodes and relationships as edges, enabling efficient traversal of connected data such as friends, followers, or recommendations without relying on multiple expensive joins.
</details>

<details>
<summary><b>Q5. An IoT company records temperature readings from one million sensors every second. Which database type is most appropriate? Explain.</b></summary>
<br>
<b>A5:</b> A time-series database is most appropriate because the data is timestamped, append-heavy, and commonly queried over time ranges for aggregation, monitoring, and trend analysis.
</details>

<details>
<summary><b>Q6. What is a vector database, and why has it become important in modern AI applications?</b></summary>
<br>
<b>A6:</b> A vector database stores high-dimensional embeddings produced by machine learning models and supports efficient similarity search. It is a core component of modern AI systems such as semantic search, recommendation engines, and Retrieval-Augmented Generation (RAG), where finding the most semantically similar data is more important than exact keyword matching.
</details>
