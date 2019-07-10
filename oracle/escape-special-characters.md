# Escape Special Characters

## Escape reserved words

[List of Oracle reserved words](https://docs.oracle.com/cd/A97630_01/appdev.920/a42525/apb.htm).

To escape a reserve word, wrap it in double quotes `"`

Example with a table called `table` with a column `order`:
```sql
SELECT * FROM "table" ORDER BY "order"
```

## Escape ampersand sign

The ampersand corresponds to the variable substitution prefix.

To escape this character. use string concatenation and the `chr(38)` replacing `&`:

Example, a row with description `Some text with an & sign`:
```sql
SELECT * FROM my_table WHERE description = 'Some text with an ' || chr(38) ||  ' sign';
```

An alternative without `chr(38)`:
```sql
SELECT * FROM my_table WHERE description = 'Some text with an &' || ' sign';
```

Finally:
```sql
SET DEFINE OFF
SELECT * FROM my_table WHERE description = 'Some text with an & sign';
SET DEFINE ON
```