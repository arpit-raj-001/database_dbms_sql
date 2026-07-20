<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔗 Normalization & Dependency Preservation</h1>
  <h2>Chapter 51</h2>
</div>

---

## 🧐 Dependency Preservation?

> [!NOTE]
> **Definition:** A decomposition is Dependency Preserving if all Functional Dependencies (FDs) from the original relation can still be enforced without joining the decomposed tables.

**Simple Definition:**
After splitting a table, you should still be able to enforce every original rule directly on the smaller tables. No joins should be required.

### Dependency Preservation allows:
- Easier constraint checking
- Faster updates
- Simpler application logic
- Better performance

---

## 🧪 Preservation Tests

### Method 1 (Interview-Friendly)
Check every original FD. Ask: *"Can this FD be checked inside one decomposed relation?"*
If yes, Dependency Preserved.

---

## ⚖️ BCNF Trade-offs
*One of the most important interview concepts.*

**3NF generally provides:**
- ✅ Lossless Join
- ✅ Dependency Preservation

**BCNF guarantees:**
- ✅ Lossless Join
- ❌ But may lose Dependency Preservation.

---

## 🛤️ Complete Normalization Algorithm

```text
Raw Relation (UNF)
       ▼
Find Functional Dependencies
       ▼
Find Candidate Keys
       ▼
Convert to 1NF
       ▼
Convert to 2NF
       ▼
Convert to 3NF
       ▼
Convert to BCNF (if needed)
       ▼
Convert to 4NF (if needed)
       ▼
Convert to 5NF (if needed)
       ▼
Verify Lossless Join
       ▼
Verify Dependency Preservation
       ▼
Final Database Schema
```
