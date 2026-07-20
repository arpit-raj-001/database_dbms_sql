<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🧮 Functional Dependencies & Normalization Algorithms</h1>
  <h2>Chapter 54</h2>
</div>

---

## 📚 Canonical Cover (Minimal Cover)

> [!NOTE]
> **Definition:** A Canonical Cover (also called Minimal Cover) is the smallest equivalent set of Functional Dependencies that represents the same constraints as the original FD set.

**It has:**
- No redundant FDs.
- No redundant attributes.
- Exactly one attribute on the right-hand side of each FD.

### Properties of a Minimal Cover
For an FD set $F$, its Minimal Cover $F_c$:
1. Every FD has one attribute on the RHS.
2. No attribute on the LHS is irrelevant.
3. No FD is redundant.
4. $F_c$ is equivalent to $F$.

---

## ⚙️ Algorithm to find Minimal Cover

Given FD set $F$:

**Step 1:** Split RHS attributes.
**Step 2:** Remove extraneous attributes from the LHS.
**Step 3:** Remove redundant FDs.

### Example
**Given:**
- `A → BC`
- `B → C`
- `A → B`
- `AB → C`

**Step 1: Split RHS**
- `A → B`
- `A → C`
- `B → C`
- `A → B`
- `AB → C`

**Step 2: Remove duplicates**
- `A → B`
- `A → C`
- `B → C`
- `AB → C`

**Step 3: Check redundancy**
Since `A → B` and `B → C` implies `A → C`.
Remove `A → C`.

Now check `AB → C`. Since `A → B`, `AB` is just `A`. `A → C` is already implied.
Remove `AB → C`.

**Final Minimal Cover:**
- `A → B`
- `B → C`

---

## 🗑️ Extraneous Attributes

> [!NOTE]
> **Definition:** An attribute is extraneous if removing it does not change the closure of the FD set.

### Left Extraneous Attribute
**Example:** `AB → D`
If `A⁺ = ABD`, then `B` is unnecessary.

### Right Extraneous Attribute
**Example:** `A → BC`
If `C` is already implied by other FDs, remove it.

---

## 🔁 Attribute Closure Algorithm
*One of the most important GATE topics.*

> [!NOTE]
> **Definition:** The closure of an attribute set $X$, denoted by $X⁺$, is the set of all attributes that can be functionally determined from $X$.

**Used for:**
- Finding Candidate Keys.
- Testing Functional Dependencies.

**Algorithm:**
1. Start with $X⁺ = X$
2. Repeatedly apply FDs. Whenever `LHS ⊆ X⁺`, add `RHS`.
3. Repeat until no more attributes can be added.

---

## 🔑 Candidate Key Algorithm

> [!NOTE]
> **Definition:** A Candidate Key is the smallest attribute set whose closure equals all attributes of the relation.

**Algorithm:**
1. Find attributes never appearing on RHS. They *must* be in every Candidate Key.
2. Compute closures.
3. Add attributes until $X⁺ =$ All attributes.
4. Remove unnecessary attributes to ensure minimality.

---

## ✂️ BCNF Decomposition Algorithm

BCNF removes every FD where: `Determinant ≠ Candidate Key`.

**Algorithm:**
1. Find BCNF violation `X → Y`.
2. Split into:
   - $R_1(XY)$
   - $R_2(R - Y) \cup X$
3. Repeat until every relation satisfies BCNF.

---

## 🧩 3NF Synthesis Algorithm

Unlike BCNF, **3NF guarantees Dependency Preservation.**

**Algorithm:**
1. Compute Minimal Cover.
2. For every FD `X → Y`, create one relation `XY`.
3. Remove duplicate relations.
4. If no relation contains a Candidate Key, add one relation containing the Candidate Key.

---

## 🏃‍♂️ Chase Algorithm (Lossless Join Test)

> [!NOTE]
> The Chase Algorithm is a formal procedure used to verify whether a decomposition is lossless. It is mainly used in theory.

For a decomposition of $R$ into $R_1$ and $R_2$, it is lossless if:
$(R_1 \cap R_2) \rightarrow R_1$ **OR** $(R_1 \cap R_2) \rightarrow R_2$ using the given FD set.

**In words:** The common attribute determines all attributes of $R_1$ or $R_2$.

---

## 🛡️ Dependency Preservation Test

> [!NOTE]
> **Definition:** A decomposition preserves dependencies if every original FD can still be enforced without joining the decomposed relations.

**Procedure:**
For each original FD:
1. Check whether its attributes exist within a single decomposed relation.
2. If not, compute closures using the projected FD sets.
3. If all original FDs can still be derived, the decomposition is dependency preserving.

---

## 🧮 Closure of Functional Dependencies (F⁺)

> [!NOTE]
> **Definition:** The closure of a Functional Dependency set, denoted $F⁺$, is the set of all Functional Dependencies that can be derived from the original FD set using Armstrong's Axioms.

### Difference Between X⁺ and F⁺
| $X⁺$ | $F⁺$ |
| :--- | :--- |
| Closure of an attribute set | Closure of an FD set |
| Produces attributes | Produces Functional Dependencies |

### Equivalence of FD Sets
**Definition:** Two FD sets $F_1$ and $F_2$ are equivalent if they imply exactly the same Functional Dependencies ($F_1⁺ = F_2⁺$).

**How to Test Equivalence:**
1. Check whether every FD in $F_1$ can be derived from $F_2$.
2. Check whether every FD in $F_2$ can be derived from $F_1$.
