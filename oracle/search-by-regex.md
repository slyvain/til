# Match By RegEx

Match results based on a regular expression. For example:

```sql
SELECT * FROM table
WHERE
  REGEXP_LIKE(column, '^[0-9]+?$') -- Only positive numbers
  OR REGEXP_LIKE(column, '^-?[0-9]+?$') -- Positive or negative numbers
  OR REGEXP_LIKE(column, '^-?[0-9]+(\.[0-9]+)?$') -- Decimal numbers (neg or pos)
```