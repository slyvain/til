# Simple Hierarchical Queries

Let's consider the following table `person`:

| Id | Firstname | Lastname | Parent_Id |
| -- | --- | --- | --- |
| 1  | Patricia | Smith | |
| 2  | Adam | Smith | 1|
| 3  | Martha | Smith | 1  |
| 4  | Kevin | Smith | 2 |
| 5  | Tyler | Smith | 4 |

More examples on the [Oracle documentation](https://docs.oracle.com/cd/B28359_01/server.111/b28286/queries003.htm#SQLRF52315).

## Find all children

This query will *also* return the starting Id, so filter it out if necessary.

```sql
SELECT t1.*
FROM person t1
--WHERE t1.id <> :myId
START WITH t1.id = :myId
CONNECT BY nocycle PRIOR t1.id = t1.parent_id ;
```

## Find root parent

Get the root parent of any row.

```sql
SELECT t1.*
FROM person t1
WHERE t1.parent_id IS NULL
START WITH t1.id = :myId
CONNECT BY PRIOR t1.parent_id = t1.id
```

The query will return the first row `Id 1` for any of those entry.

## Find all parents

Simply remove the `where` statement, to list all the parents of the given Id.
This query will *also* return the starting Id, so filter it out if necessary.

```sql
SELECT t1.*
FROM person t1
--WHERE t1.id <> :myId
START WITH t1.id = :myId
CONNECT BY PRIOR t1.parent_id = t1.id
```
