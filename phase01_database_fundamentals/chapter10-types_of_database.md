AUTHORED BY : ARPIT RAJ , LNMIIT JAIPUR

# Types of Databases

> *Mongo DB, redis, cassandra wont be discussed here, It comes later in Phase 26 (NOSQL)*

**different databases for applications have different requirements :-**
• Structured data
• fast caching
• complex relation
• time based measure
• AI embeddings
⟶ **hence different database models evolved**

**DATABASES**
`Relational` `document` `key value` `wide column` `graph` `time series` `vector`

---

### 1. Relational database
*My SQL, postgre SQL, oracle database*

data is stored in tables, rows are called tuples columns are called attributes , relations are maintained using foreign keys

**FEATURES**
• Structured schema
• SQL support
• ACID transactions
• Joins
• Normalization
• Constraints

**CHARACTERISTICS**
first choice when data integrity is critical
used in Banking, Ecomm, ERP, inventory, hospital systems etc

**Cons:**
less flexible schema, horizontal scaling challenging

---

### 2. Document Database
*Mongo DB, couch DB*

stores data as documents, JSON or BSON typically. each document can have a different structure

```json
{
  "studentId" : 006,
  "name" : "Aadz",
  "Skills" : [ "VLSI", "RISC-V", "C" ]
}
```

**CHARACTERISTIC :**
• flexible schema
• nested objects
• JSON based
• easy to evolve

**BEST FOR**
user profiles, blogging platforms, rapidly changing applications

**Cons:**
data duplication is common, complex joins are weaker than relational

---

### 3. Key - Value database
*Redis, Amazon dynamo DB*

a giant hash map, where we ask for key and we get its value.
fast lookups, memory based, simple model

**BEST FOR :**
caching, sessions, rate-limiting, shopping cart, leader board

**PROS** → simple ops, low latency, high throughput
**CONS** → no joins, limited querying

---

### 4. Wide column database
*Apache Cassandra, Apache HBase*

these are optimized for large scale distributed workload
unlike relational, rows can have different columns
**massive scalability, distributed by design, handles huge datasets**

| Roll | Name | email | phone |
| :--- | :--- | :--- | :--- |
| 006 | Aadz | NULL | 79870... |
| 012 | Arpit | hehe@gmail.com | NULL |

**BEST FOR**
logging, social media activity feed, telecom, IoT

**pros** → excellent horizontal scaling, high throughput, fault tolerant
**cons** → complex, few relational features

---

### 5. Graph database
*Neo4j, Amazon neptune*

Stores nodes and relationships (edges) instead of tables, relationships are stored directly
**Eg** `A` ----- **Friends** ----- `B`

**BEST FOR**
knowledge graphs, recommendation systems, fraud detection, network analysis

**PROS**
efficient relationship traversal, natural representation of connected data
**CONS**
not ideal for traditional transactional business application

---

### 6. Time - Series Database
*influx DB, timescale DB*

stores data ordered by time, Eg Timestamp vs temperature

**BEST FOR :**
sensors, IoT, server monitor, metrics collection

**PROS**
efficient time based queries, data compression
**CONS**
specialized for temporal workloads rather than general purpose

---

### 7. Vector database
*Pinecone, Weaviate*

instead of storing rows, it stores embedding and enable similarity search

**BEST FOR**
AI chatbots, RAG, recommendation system, semantic search

**PROS**
semantic retrieval, fast nearest neighbour search
**CONS**
not a replacement, used alongside SQL

---

## Practice Questions

**Q1. Why do different types of databases exist instead of one universal database?**
A1: Different database types exist because workloads differ significantly. Transaction-heavy systems require strong consistency and relational modeling, while caching, graph traversal, time-based analytics, or AI similarity search each benefit from specialized storage models and query mechanisms.

**Q2. Compare relational and document databases. Give one use case where each is the better choice.**
A2: A relational database stores structured data in tables with predefined schemas and is ideal for applications such as banking or order processing where ACID transactions and joins are essential. A document database stores flexible JSON-like documents, making it better suited for evolving user profiles, content management systems, or product catalogs.

**Q3. A company needs to store millions of user login sessions with very fast retrieval by session ID. Which database type would you recommend and why?**
A3: A key-value database is the best choice. Login sessions are naturally modeled as session_id → session_data mappings, requiring extremely fast reads and writes without complex relational queries.

**Q4. Why are graph databases well suited for social networking applications?**
A4: Graph databases store entities as nodes and relationships as edges, enabling efficient traversal of connected data such as friends, followers, or recommendations without relying on multiple expensive joins.

**Q5. An IoT company records temperature readings from one million sensors every second. Which database type is most appropriate? Explain.**
A5: A time-series database is most appropriate because the data is timestamped, append-heavy, and commonly queried over time ranges for aggregation, monitoring, and trend analysis.

**Q6. What is a vector database, and why has it become important in modern AI applications?**
A6: A vector database stores high-dimensional embeddings produced by machine learning models and supports efficient similarity search. It is a core component of modern AI systems such as semantic search, recommendation engines, and Retrieval-Augmented Generation (RAG), where finding the most semantically similar data is more important than exact keyword matching.
