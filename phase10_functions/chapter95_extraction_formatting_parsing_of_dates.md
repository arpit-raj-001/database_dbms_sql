<div align="center">
  <small><i>Authored by: Arpit Raj, LNMIIT Jaipur</i></small>
  <h1>📅 SQL Date Functions Notes (EXTRACT, TRUNC, FORMAT)</h1>
  <h2>Chapter 95</h2>
</div>

---

## 🔍 EXTRACT()

`EXTRACT()` retrieves a specific component from a date, time, or timestamp.

Instead of returning the whole date (`2026-07-19 14:35:28`), it can return:
- **Year:** `2026`
- **Month:** `7`
- **Day:** `19`
- **Hour:** `14`
- **Minute:** `35`
- **Second:** `28`

*Think of it as breaking a date into its individual parts.*

```sql
SELECT * FROM Orders WHERE EXTRACT(YEAR FROM order_date) = 2026;
```

```sql
SELECT EXTRACT(YEAR FROM DATE '2026-07-19');
```
**Result:** `2026`

---

### QUARTER

```sql
SELECT EXTRACT(QUARTER FROM DATE '2026-07-19');
```
**Result:** `3`

**Quarter mapping:**
| Months | Quarter |
| :--- | :--- |
| Jan–Mar | Q1 |
| Apr–Jun | Q2 |
| Jul–Sep | Q3 |
| Oct–Dec | Q4 |

---

### DOW (Day of Week)
*(PostgreSQL)*
```sql
SELECT EXTRACT(DOW FROM DATE '2026-07-19');
```
Returns an integer representing the day of the week (exact numbering is vendor-specific).

### DOY (Day of Year)
```sql
SELECT EXTRACT(DOY FROM DATE '2026-07-19');
```
**Result:** `200`
*Useful for scientific calculations and analytics.*

---

## 🔪 DATE_TRUNC()

*PostgreSQL-specific.*
Used to "round down" timestamps.

**Syntax:**
```sql
DATE_TRUNC(unit, timestamp)
```

### Truncate to Year
```sql
SELECT DATE_TRUNC('year', TIMESTAMP '2026-07-19 14:35:28');
```
**Result:** `2026-01-01 00:00:00`

### 10.4 Month
```sql
SELECT DATE_TRUNC('month', NOW());
```
**Result:** `2026-07-01 00:00:00`

### 10.5 Week
```sql
SELECT DATE_TRUNC('week', NOW());
```
*Returns the beginning of the week according to the database's definition.*

### Day
```sql
SELECT DATE_TRUNC('day', NOW());
```
**Result:** `2026-07-19 00:00:00`

### 10.7 Hour
```sql
SELECT DATE_TRUNC('hour', NOW());
```
**Result:** `2026-07-19 14:00:00`

---

## 🎨 Formatting Dates

Converting dates into human-readable strings.

### 11.1 DATE_FORMAT() (MySQL)

**Syntax:**
```sql
DATE_FORMAT(date, format)
```

**Example:**
```sql
SELECT DATE_FORMAT('2026-07-19', '%d-%m-%Y');
```
**Result:** `19-07-2026`

### Common MySQL Format Specifiers

| Specifier | Meaning |
| :--- | :--- |
| `%Y` | 4-digit year |
| `%y` | 2-digit year |
| `%m` | Month number |
| `%d` | Day |
| `%H` | 24-hour |
| `%i` | Minutes |
| `%s` | Seconds |
| `%M` | Month name |
| `%W` | Weekday name |

---

## 🧩 Parsing Dates

Converting strings into dates.
- **Formatting:** `Date → String`
- **Parsing:** `String → Date`

### 12.1 STR_TO_DATE() (MySQL)
```sql
SELECT STR_TO_DATE('19-07-2026', '%d-%m-%Y');
```
**Result:** `2026-07-19`

### CAST()
Convert compatible strings to dates.
```sql
SELECT CAST('2026-07-19' AS DATE);
```
