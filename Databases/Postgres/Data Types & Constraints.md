
| Category              | Data Type                 | Description                                                                                                                                                                                                                                                                                                                                            |
| --------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Numeric Types**     | `smallint` / `int2`       | 2-byte integer (Range: -32,768 to 32,767)                                                                                                                                                                                                                                                                                                              |
|                       | `integer` / `int`, `int4` | 4-byte integer (Range: -2,147,483,648 to 2,147,483,647)                                                                                                                                                                                                                                                                                                |
|                       | `bigint` / `int8`         | 8-byte integer (Large-range integer)<br>-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807                                                                                                                                                                                                                                                        |
|                       | `decimal(p,s)`            | Exact numeric with user-defined precision (`p`) and scale (`s`)<br>DECIMAL specifies the data type exact **numeric**, with the decimal scale specified by the scale and the implementation-defined decimal precision equal to or greater than the value of the specified precision                                                                     |
|                       | `numeric(p,s)`            | Same as `decimal`, for high-precision calculations<br>NUMERIC specifies the data type exact **numeric**, with the decimal precision and scale specified by the precision and scale<br><br>**There isn't any difference between numeric and decimal, in Postgres. There are two type names because the SQL standard requires us to accept both names.** |
|                       | `real`                    | 4-byte floating-point number                                                                                                                                                                                                                                                                                                                           |
|                       | `double precision`        | 8-byte floating-point number                                                                                                                                                                                                                                                                                                                           |
|                       | `serial`                  | Auto-incrementing 4-byte integer                                                                                                                                                                                                                                                                                                                       |
|                       | `bigserial`               | Auto-incrementing 8-byte integer                                                                                                                                                                                                                                                                                                                       |
| **Character Types**   | `char(n)`                 | Fixed-length character string                                                                                                                                                                                                                                                                                                                          |
|                       | `varchar(n)`              | Variable-length character string with a limit                                                                                                                                                                                                                                                                                                          |
|                       | `text`                    | Variable-length character string without a limit                                                                                                                                                                                                                                                                                                       |
| **Boolean Type**      | `boolean`                 | `TRUE`, `FALSE`, or `NULL`                                                                                                                                                                                                                                                                                                                             |
| **Date & Time Types** | `date`                    | Stores only date (YYYY-MM-DD)                                                                                                                                                                                                                                                                                                                          |
|                       | `time`                    | Stores only time (HH:MI:SS)                                                                                                                                                                                                                                                                                                                            |
|                       | `timestamp`               | Stores date and time without time zone                                                                                                                                                                                                                                                                                                                 |
|                       | `timestamptz`             | Stores date and time with time zone                                                                                                                                                                                                                                                                                                                    |
|                       | `interval`                | Stores a time interval (e.g., '2 days 3 hours')                                                                                                                                                                                                                                                                                                        |
| **UUID Type**         | `uuid`                    | Universally Unique Identifier                                                                                                                                                                                                                                                                                                                          |
| **Monetary Type**     | `money`                   | Currency-based value with fixed decimal precision                                                                                                                                                                                                                                                                                                      |
| **JSON Types**        | `json`                    | Stores JSON (text-based, no validation)                                                                                                                                                                                                                                                                                                                |
|                       | `jsonb`                   | Stores JSON in a binary format (faster search and indexing)                                                                                                                                                                                                                                                                                            |
| **Array Type**        | `type[]`                  | One-dimensional or multi-dimensional arrays (e.g., `integer[]`)                                                                                                                                                                                                                                                                                        |
| **HStore Type**       | `hstore`                  | Key-value pairs in a single column                                                                                                                                                                                                                                                                                                                     |
| **Geometric Types**   | `point`                   | Stores (x, y) coordinates                                                                                                                                                                                                                                                                                                                              |
|                       | `line`                    | Stores infinite line                                                                                                                                                                                                                                                                                                                                   |
|                       | `lseg`                    | Stores line segment                                                                                                                                                                                                                                                                                                                                    |
|                       | `box`                     | Stores rectangular box                                                                                                                                                                                                                                                                                                                                 |
|                       | `path`                    | Stores open/closed path                                                                                                                                                                                                                                                                                                                                |
|                       | `polygon`                 | Stores polygon shape                                                                                                                                                                                                                                                                                                                                   |
|                       | `circle`                  | Stores circle shape                                                                                                                                                                                                                                                                                                                                    |
| **Network Types**     | `cidr`                    | IPv4/IPv6 network                                                                                                                                                                                                                                                                                                                                      |
|                       | `inet`                    | IP address                                                                                                                                                                                                                                                                                                                                             |
|                       | `macaddr`                 | MAC address                                                                                                                                                                                                                                                                                                                                            |
| **Full-Text Search**  | `tsvector`                | Stores text search data                                                                                                                                                                                                                                                                                                                                |
|                       | `tsquery`                 | Stores text search queries                                                                                                                                                                                                                                                                                                                             |
| **Bit String Types**  | `bit(n)`                  | Fixed-length bit string                                                                                                                                                                                                                                                                                                                                |
|                       | `bit varying(n)`          | Variable-length bit string                                                                                                                                                                                                                                                                                                                             |
| **XML Type**          | `xml`                     | Stores XML data                                                                                                                                                                                                                                                                                                                                        |
| **Range Types**       | `int4range`               | Range of `integer` values                                                                                                                                                                                                                                                                                                                              |
|                       | `int8range`               | Range of `bigint` values                                                                                                                                                                                                                                                                                                                               |
|                       | `numrange`                | Range of `numeric` values                                                                                                                                                                                                                                                                                                                              |
|                       | `daterange`               | Range of `date` values                                                                                                                                                                                                                                                                                                                                 |
|                       | `tsrange`                 | Range of `timestamp` values                                                                                                                                                                                                                                                                                                                            |
|                       | `tstzrange`               | Range of `timestamptz` values                                                                                                                                                                                                                                                                                                                          |

### **Why is the max value `2,147,483,647` for INT4?**
A **4-byte integer** (also called `int4` in PostgreSQL) consists of **32 bits**:
- **1 bit for the sign** (0 for positive, 1 for negative)
- **31 bits for the actual number**

In binary:
- The largest **unsigned** 32-bit number would be:  
  $$
  2^{32} - 1 = 4,294,967,295
  $$

- But since `INTEGER` is **signed**, half of this range is used for negative numbers:
  - **Negative range**:  
    $$
    -2^{31} \text{ to } -1
    $$
  - **Positive range**:  
    $$
    0 \text{ to } 2^{31} - 1
    $$
  - The total count:  
    $$
    2^{31} = 2,147,483,648
    $$
    numbers in each direction.

#### **Can it be more than `2,147,483,647`?**
- **Not for `INTEGER` (int4)**: If you exceed this, PostgreSQL will throw an **integer out of range** error.
- **Use `BIGINT` (int8)** if you need larger values. It’s an **8-byte (64-bit) integer** with a much bigger range:
  $$
  -9,223,372,036,854,775,808 \text{ to } 9,223,372,036,854,775,807
  $$


## **Money Representation: `MONEY` vs `NUMERIC` vs `INTEGER` vs `REAL`**  

When dealing with monetary values in PostgreSQL, you have multiple data type choices, each with different trade-offs in precision, performance, and storage.

---

### **1. `MONEY` Data Type (PostgreSQL-Specific)**
PostgreSQL provides a `MONEY` type specifically for currency values.

### **Key Features:**
- Stores **currency values** with **fixed precision**.
- Includes **locale-based formatting** (e.g., `$1,234.56` or `€1.234,56` depending on locale settings).
- Uses **8 bytes of storage** (like `BIGINT`).
- Supports **automatic currency formatting** based on PostgreSQL locale settings.

### **Example Usage:**
```sql
CREATE TABLE transactions (
    id SERIAL PRIMARY KEY,
    amount MONEY
);

INSERT INTO transactions (amount) VALUES ('$1234.56'); -- Locale-dependent
SELECT * FROM transactions;
```

**Pros:**
✅ Fixed precision, avoiding floating-point errors.  
✅ Includes built-in currency formatting.  

**Cons:**
❌ Limited arithmetic operations (e.g., division results must be converted).  
❌ Locale settings can affect behavior.  
❌ Doesn't support multiple currencies directly.  

---

### **2. `NUMERIC(p, s)` (Best for High-Precision Money Calculations)**
The `NUMERIC` type (same as `DECIMAL`) is an **exact precision** type, making it **ideal for financial calculations**.

### **Key Features:**
- Supports **arbitrary precision** (determined by `p` and `s`).
- No rounding errors (unlike `REAL` or `DOUBLE PRECISION`).
- Uses **variable-length storage**, depending on precision.

### **Example Usage:**
```sql
CREATE TABLE invoices (
    id SERIAL PRIMARY KEY,
    total NUMERIC(10,2)  -- 10 digits, 2 decimal places
);

INSERT INTO invoices (total) VALUES (1234.56);
SELECT * FROM invoices;
```

**Pros:**
✅ Exact precision for financial applications.  
✅ Safer than `FLOAT` or `REAL` for calculations.  
✅ No locale formatting issues like `MONEY`.  

**Cons:**
❌ Slightly more storage overhead compared to `INTEGER`.  
❌ Slower performance for large datasets.  

---

### **3. `INTEGER` (`INT4`) – For Whole-Number Currencies**
If your currency values **only need whole numbers (e.g., cents)**, you can use `INTEGER`.

### **Key Features:**
- **Faster performance** compared to `NUMERIC` and `MONEY`.
- Uses **only 4 bytes** of storage.
- No decimal places (ideal for storing values in cents instead of dollars).

### **Example Usage (Storing Cents Instead of Dollars):**
```sql
CREATE TABLE payments (
    id SERIAL PRIMARY KEY,
    amount_cents INTEGER
);

INSERT INTO payments (amount_cents) VALUES (123456); -- $1234.56 in cents
SELECT amount_cents / 100.0 AS amount_dollars FROM payments;
```

**Pros:**
✅ Fastest performance.  
✅ No floating-point rounding issues.  
✅ Uses less storage than `NUMERIC`.  

**Cons:**
❌ Requires manual conversion to dollars.  
❌ Can't store fractional cents.  

---

### **4. `REAL` (`FLOAT4`) – Avoid for Money (Floating-Point Precision Issues)**
`REAL` (single-precision float) is a **floating-point number** that introduces **rounding errors**, making it **unsuitable for money calculations**.

### **Key Features:**
- Uses **4 bytes** of storage.
- Can store decimals, but with limited precision (~6-7 significant digits).
- May **introduce rounding errors** in calculations.

### **Example Problem (Floating-Point Precision Loss):**
```sql
SELECT 0.1 + 0.2 AS result;
-- Expected: 0.3
-- Actual: 0.30000000000000004 (floating-point error)
```

**Pros:**
✅ Uses less storage than `NUMERIC`.  
✅ Fast arithmetic operations.  

**Cons:**
❌ **Floating-point rounding errors** (not exact).  
❌ Not suitable for financial applications.  

---

### **Comparison Table**
| Data Type  | Storage | Precision | Rounding Errors? | Best Use Case |
|------------|---------|------------|-----------------|----------------|
| `MONEY`    | 8 bytes | Fixed 2 decimals | No | Quick money formatting, localized currency display |
| `NUMERIC`  | Variable | Exact | No | **Best for financial transactions** |
| `INTEGER`  | 4 bytes | Whole numbers | No | Storing money as **cents** for performance |
| `REAL`     | 4 bytes | Approximate (6-7 digits) | **Yes** | **Avoid for money calculations** |

---

### **Conclusion: Which One Should You Use?**
| **Use Case** | **Best Data Type** |
|-------------|----------------|
| **Banking, invoices, financial transactions** | `NUMERIC(p, s)` |
| **Localized currency formatting** | `MONEY` |
| **High-performance processing (storing in cents)** | `INTEGER` |
| **Avoiding** floating-point errors | Avoid `REAL` or `DOUBLE PRECISION` |

If you need **absolute precision**, go with `NUMERIC`. If you want **fast calculations and can store amounts in cents**, use `INTEGER`. Avoid `REAL` and `DOUBLE PRECISION` for monetary values.

Would you like additional real-world use case examples? 🚀

