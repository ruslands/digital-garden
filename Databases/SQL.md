### **SQL EXECUTION ORDER:**

1. FROM / JOIN
   - Identifies the tables and combines data

2. WHERE
   - Filters rows before grouping
   - Best to maximize filtering here to reduce dataset size early
   - Executes before GROUP BY, more efficient than HAVING

3. GROUP BY
   - Groups the filtered data

4. HAVING
   - Filters grouped data

5. SELECT
   - Specifies columns to return

6. DISTINCT
   - Removes duplicate rows

7. ORDER BY
   - Sorts the final result set

8. LIMIT / OFFSET
   - Restricts the number of rows returned




