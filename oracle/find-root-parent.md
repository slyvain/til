# Find Root Parent

In a hierarchical table, get the root parent of any row.

Example, with the table `person`:

| Id | Firstname | Lastname | Parent_Id |
| -- | --- | --- | --- |
| 1  | Jaden | Smith | |
| 2  | Adam | Sandler | 1|
| 3  | Taylor | Lautner | 1  |
| 4  | Kevin | James | 2 |
| 5  | Tyler | Perry | 4 |

```sql
SELECT t1.*
FROM person t1
WHERE t1.parent_id IS NULL
START WITH t1.id = :myId
CONNECT BY PRIOR t1.parent_id = t1.id
```

The query will return the first row `Id 1` for any of those entry.