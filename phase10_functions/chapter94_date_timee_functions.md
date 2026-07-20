<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📅 SQL Date & Time Notes</h1>
  <h2>Chapter 94</h2>
</div>

---

## 🗄️ Internal Storage

Users see readable dates. Internally, databases store dates in optimized binary formats for efficient comparison and arithmetic.

### Unix Timestamp
Counts the number of seconds since the Unix epoch.
- **Epoch:** `1970-01-01 00:00:00 UTC`
- **Example:** `1752887730` represents a specific moment in time.

**Epoch Time Advantages:**
- Easy arithmetic
- Compact storage
- Fast comparisons

### Fractional Seconds
Many databases support milliseconds or microseconds.
**Example:** `2026-07-19 14:35:28.123456`

---

## 🕒 Current Date & Time Functions

### NOW()
Returns the current date and time.
```sql
SELECT NOW();
```
**Example result:** `2026-07-19 14:35:28`

### CURRENT_TIME
Returns only the current time.
```sql
SELECT CURRENT_TIME;
```

### CURRENT_TIMESTAMP
ANSI SQL standard function.
```sql
SELECT CURRENT_TIMESTAMP;
```
**Result:** `2026-07-19 14:35:28`

### LOCALTIMESTAMP
Returns the current local timestamp. Common in PostgreSQL.
```sql
SELECT LOCALTIMESTAMP;
```

> [!NOTE]
> In MySQL, `NOW()` and `CURRENT_TIMESTAMP` return the same value.

---

## ➕ Date Arithmetic

### Adding Days (MySQL)
```sql
SELECT DATE_ADD('2026-07-19', INTERVAL 10 DAY);
```
**Result:** `2026-07-29`

### 3.3 Subtracting Days
```sql
SELECT DATE_SUB('2026-07-19', INTERVAL 7 DAY);
```
**Result:** `2026-07-12`

### 3.4 Adding Weeks
```sql
SELECT DATE_ADD('2026-07-19', INTERVAL 2 WEEK);
```
**Result:** `2026-08-02`

### 3.5 Adding Months
```sql
SELECT DATE_ADD('2026-07-19', INTERVAL 3 MONTH);
```
**Result:** `2026-10-19`

### 3.6 Adding Years
```sql
SELECT DATE_ADD('2026-07-19', INTERVAL 5 YEAR);
```
**Result:** `2031-07-19`

---

## 🗓️ DATEADD()

> [!IMPORTANT]
> `DATEADD()` is not available in MySQL. It is used in SQL Server. MySQL uses `DATE_ADD()`.

`DATEADD()` adds a specified interval to a date or time.

### 4.2 Syntax (SQL Server)
```sql
DATEADD(datepart, number, date)
```

**Example:**
```sql
SELECT DATEADD(DAY, 10, '2026-07-19');
```
**Result:** `2026-07-29`

### Vendor Alternatives
| Database | Equivalent |
| :--- | :--- |
| **MySQL** | `DATE_ADD(date, INTERVAL n unit)` |

---

## 🧩 Mixed Intervals & Complex Additions

**MySQL Syntax:**
```sql
DATE_ADD(date, INTERVAL value unit)
-- or
date + INTERVAL value unit
```

```sql
SELECT NOW() + INTERVAL 3 HOUR;
SELECT NOW() + INTERVAL 30 MINUTE;
```
*(Example: OTP valid for 30 minutes).*

### Mixed Intervals
Different vendors support combining intervals differently.

**MySQL:**
```sql
SELECT DATE_ADD(
 DATE_ADD(CURRENT_DATE, INTERVAL 1 YEAR),
 INTERVAL 2 MONTH
);
```

**PostgreSQL:**
```sql
SELECT CURRENT_DATE + INTERVAL '1 year 2 months';
```

---

## 🕰️ Introduction to AGE() & DATEDIFF()

### AGE() (PostgreSQL)
`AGE()` is a PostgreSQL function. It returns the elapsed time between two dates. Unlike `DATEDIFF()`, it preserves years, months and days.

**6.2 Syntax:**
```sql
AGE(date1, date2)
```

**6.3 Age from Birthdate:**
```sql
SELECT AGE('2026-07-19', '2003-05-12');
```
**Result:** `23 years 2 months 7 days`

### DATEDIFF()
`DATEDIFF()` calculates the difference between two dates. Most commonly measured in days.

**7.2 Syntax (MySQL):**
```sql
DATEDIFF(date1, date2)
```
**Result:** `date1 - date2`

---

## ⚖️ AGE vs DATEDIFF

| AGE | DATEDIFF |
| :--- | :--- |
| `23 years 2 months 5 days` | `8467 days` |

**Vendor Support:**
| Database | AGE() | DATEDIFF() |
| :--- | :--- | :--- |
| **PostgreSQL** | ✅ Yes | |
| **MySQL** | ❌ No | ✅ Yes |
| **SQL Server** | ❌ No | ✅ Yes |

---

## 🔢 DATEDIFF Calculations (MySQL)

### 7.3 Difference in Days
```sql
SELECT DATEDIFF('2026-07-20', '2026-07-15');
```
**Result:** `5`

### 7.4 Difference in Months
MySQL `DATEDIFF` does not return months directly. Instead:
```sql
TIMESTAMPDIFF(MONTH, start, end)
```

### 7.5 Difference in Years
Likewise:
```sql
TIMESTAMPDIFF(YEAR, start, end)
```

### 7.6 Negative Results
```sql
SELECT DATEDIFF('2026-07-10', '2026-07-20');
```
**Result:** `-10`

---

## ⏱️ TIMESTAMPDIFF()

`TIMESTAMPDIFF()` returns the difference between two timestamps in a chosen unit. Unlike `DATEDIFF()`, you choose the unit.

**8.2 Syntax:**
```sql
TIMESTAMPDIFF(unit, start, end)
```

**8.3 Seconds:**
```sql
SELECT TIMESTAMPDIFF(SECOND, login_time, logout_time);
```
