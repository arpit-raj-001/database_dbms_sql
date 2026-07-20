Authored by: Arpit Raj, Lnmiit jaipur 

# Ch 2 File Systems in detail

• DBMS is built on top of a file system
• a file is a collection of bytes stored on secondary storage and managed by OS

### File system
↳ component of OS responsible for organizing and managing files
• create / delete / rename files
• read / write bytes
• manage directories / permissions at file level

### Flow
Application → read() → OS → file system → disk 

application itself parses CSV, search and update records validate data, OS does none of this 

### Text files
| PROS | CONS |
| :--- | :--- |
| • easy debugging<br>• portable<br>• platform independent | • no schema<br>• no indexing<br>• slow process |

### CSV files
*(Comma separated values)*

| PROS | CONS |
| :--- | :--- |
| • easy import/export<br>• excel supported | • no indexing<br>• no constraint<br>• no relation<br>• no transaction |

### JSON files
*(excellent to represent semi structured data)*

| PROS | CONS |
| :--- | :--- |
| • flexible schema<br>• easy to exchange over API's<br>• supports nested objects | • no transaction<br>• no concurrency<br>• searching needs scanning<br>• large dataset is inefficient |

### File system vs DBMS

| Feature | File system | DBMS |
| :--- | :--- | :--- |
| stores bytes | ✔ | ✔ |
| query language | ✘ | ✔ |
| indexing | ✘ | ✔ |
| transaction | ✘ | ✔ |
| Concurrency control | ✘ | ✔ |
| recovery | ✘ | ✔ |
| integrity | ✘ | ✔ |
| query optimization | ✘ | ✔ |
| security | ✘ | ✔ |

### Sequential access vs random access

| Sequential access | random access |
| :--- | :--- |
| • O(n)<br>• looks for data in a sequential manner, i.e checks all the record<br>• good for logs, large sequential reads, streaming etc | • O(1)<br>• Jump directly to required location using index/offset<br>• takes use of B+ trees and hash indices |
