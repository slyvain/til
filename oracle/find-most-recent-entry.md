# Find Most Recent Entry

Get the most recent entry from a table when it exists multiple identical data (except for the timestamp, obviously).

Example, with the table `person`:

| Id | Firstname | Lastname | Modified |
| -- | ---| --- | --- |
| 1  | Paul | Smith | 18/11/2017 07:00:00 |
| 2  | Paul | Smith | 19/11/2017 08:00:00 |
| 3  | Paul | Smith | **20/11/2017 09:00:00** |

```sql
SELECT p.firstname, p.lastname, p.modified
FROM person p
  INNER JOIN (SELECT tmp.firstname, tmp.lastname, MAX (tmp.modified) AS MostRecent
              FROM person tmp
              GROUP BY tmp.firstname, tmp.lastname ) p2
    ON (p.firstname = p2.firstname AND p.lastname = p2.lastname AND p.modified = p2.MostRecent)
```

The result gives us `Row 3`.