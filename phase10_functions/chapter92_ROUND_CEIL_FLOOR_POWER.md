<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>🔢 SQL Mathematics & Number Notes</h1>
  <h2>Chapter 92</h2>
</div>

---

## 🔢 Integer vs Floating Point

| Integer | Floating Point |
| :--- | :--- |
| `25` | `25.6789` |
| Exact value. | Approximate representation. |

### Exact Types
- `DECIMAL`
- `NUMERIC`

> [!TIP]
> **Ideal for:** Money, Taxes, Banking
> `DECIMAL(8,2)`: 8 is total digits (precision) and 2 is after decimal digit (scale).

---

## 🔄 Type Conversion

### Implicit Conversion
SQL converts automatically.
```sql
SELECT 10 + 5.5;
```
**Result:** `15.5`

### Explicit Conversion
Using `CAST`.
```sql
SELECT CAST(12.9 AS INT);
```

---

## 🤷‍♂️ NULL Behavior

`NULL` propagates.
```sql
SELECT 5 + NULL;
```
**Result:** `NULL`

```sql
ROUND(NULL)
```
**Result:** `NULL`

---

## 🔵 ROUND()

`ROUND()` rounds a number to the nearest value.
`ROUND(number)` or `ROUND(number, decimal_places)`

**Example:**
```sql
SELECT ROUND(12.3456, 2);
```
**Result:** `12.35`

**Round to Integer:**
```sql
SELECT ROUND(12.7);
```
**Result:** `13`

```sql
SELECT ROUND(12.2);
```
**Result:** `12`

---

### Negative Precision
*Very common interview topic.*

**Round to nearest ten:**
```sql
SELECT ROUND(456, -1);
```
**Result:** `460`

**Nearest hundred:**
```sql
SELECT ROUND(456, -2);
```
**Result:** `500`

**Nearest thousand:**
```sql
SELECT ROUND(6789, -3);
```
**Result:** `7000`

---

### ROUND vs TRUNCATE

Suppose `12.987`

**ROUND:**
`12.99` (performs mathematical rounding).

**TRUNCATE:**
`12.98` (simply removes digits).

---

## ⬆️ CEIL() / CEILING()

### 3.1 Introduction
`CEIL()` always rounds up toward positive infinity. Even if the decimal part is very small.

**Example:**
`12.01` → `13`

### 3.2 Syntax
```sql
CEIL(number)
```

**Positive Numbers:**
```sql
SELECT CEIL(12.1);
```
**Result:** `13`

**Already integer:**
```sql
SELECT CEIL(20);
```
**Result:** `20`

**Negative Numbers:**
*Very important.*
```sql
SELECT CEIL(-4.7);
```
**Result:** `-4` (Because -4 is greater than -4.7).

---

## ⬇️ FLOOR()

### 4.1 Introduction
`FLOOR()` always rounds down toward negative infinity.

**Example:**
`12.99` → `12`

### 4.2 Syntax
```sql
FLOOR(number)
```

**Negative Numbers:**
*Interview favorite.*
```sql
SELECT FLOOR(-4.2);
```
**Result:** `-5`

---

## ⚡ Syntax (Power)

```sql
POWER(base, exponent)
-- or
POW(base, exponent)
```
*(MySQL also supports `POW()`.)*

### Fractional Powers
Fractional exponents represent roots.
**Square root:**
$x^{1/2} = \sqrt{x}$

**Example:**
```sql
SELECT POWER(25, 0.5);
```
**Result:** `5`

**Cube root:**
```sql
SELECT POWER(27, 1.0/3);
```
**Result:** `3`

### Population Growth
| Population | Growth rate | Years |
| :--- | :--- | :--- |
| `100000` | `5%` | `10` |

```sql
SELECT 100000 * POWER(1.05, 10);
```
