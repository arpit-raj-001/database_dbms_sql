<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🛡️ ACID Properties (Introduction)</h1>
  <h2>Chapter 12</h2>
</div>

---

> [!NOTE]
> ↳ *We will cover this in detail in Phase 22 (transactions) where we will dive deep into ACID, concurrency, isolation levels, MVCC, locks, and deadlocks.*

### 🎭 The Problem

**Example Accounts:**
| Account | Balance |
| :--- | :--- |
| Arpit | `250000` |
| Aadz | `125000` |

Now let Arpit give his everything to Aadz.
Then:
1. `(s1)` Deduct 250k from Arpit
2. `(s2)` Append 250k to Aadz

> [!WARNING]
> But if the transaction fails after `(s1)`, the database becomes inconsistent as money disappears! **ACID prevents situations like this.**

---

## 🛡️ What is ACID?

**ACID** is a set of 4 properties that guarantees reliable, correct, and safe execution of database transactions even in the presence of failure or concurrent access.

> **Transaction:** A sequence of one or more database operations treated as a single, indivisible operation.

### 🅰️ Atomicity
→ Guarantees that either **every** operation in a transaction completes successfully or **none** of them do. There is no partial completion.

### ©️ Consistency
→ Guarantees every committed transaction moves the database from one valid state to another valid state while preserving all constraints and business rules.
*Example: A bank balance cannot be negative.*

### 🇮 Isolation
→ Guarantees that concurrent transactions execute as if they were running one after the other, preventing unwanted performance interference.
*Example: 2 people cannot buy the last available product.*

### 🇩 Durability
→ Guarantees that once a transaction is committed, its changes will survive power failures, crashes, or system restarts.
