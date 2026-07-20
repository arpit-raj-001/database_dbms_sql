AUTHORED BY: ARPIT RAJ, LNMIIT JAIPUR

# SQL
## Ch-1 (Why Database)

### File System vs DBMS
A file system (managed by OS) is responsible for creating/deleting files or reading/writing bytes or managing storage on disk.

Before database, most software stored info directly in OS files.

But the OS manages files not the data semantics. Concepts like tables, relationships, keys, constraints and transactions are implemented by DBMS, not file system.

Flat files like .csv .txt can store data but they cant efficiently manage data at scale. 

### What is a DBMS?
A database management system (DBMS) is a software that provides an abstraction layer between application and physical storage. Emphasizes management not just storage.
Provides :-

**I) Persistent storage**
data survives after program exit

**II) Efficient retrieval**
instead of scanning every record, DBMS uses indexes and access paths to retrieve data quickly

**III) Data integrity**
constraints ensure invalid data cant be stored

**IV) Concurrency control**
thousands of users can read and modify data at same time without corrupting it

**V) Transaction management**
a group of related operation either all succeed or fail together

**VI) Crash recovery**
if system crashes after transaction committed, DBMS can recover committed data and roll back incomplete work

**VII) Security**
different users have different info

**VIII) Data independence**
interact with logical view of data without worry on how it's stored on disk

### Scaling & File-Based Systems
For simple sequential storage flat files can be faster as less overhead, but as data volume, complexity, concurrency increases DBMS performs better due to indexing, buffering, caching, query optimization.

**File Based System become difficult to maintain** as app grows ↑, multiple programs have their own copies of data and implement their own logic leading to :-
• Data redundancy
• Data inconsistency
• lack of concurrency control
• poor scalability
• complex recovery mechanism

**DBMS gives ↓**
centralized organization of data, efficient querying, indexing, query optimization, data integrity via constraints, transaction management, concurrency control, crash recovery, security, data independence, recovery mechanisms
