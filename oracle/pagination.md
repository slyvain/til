# Pagination

This entry is to show how to paginate rows (here customers) when we join on another table (orders), resulting in "duplicated" results from the left table:
having multiple orders per customer means having the same customer returned in mutliple rows.

Play around: http://sqlfiddle.com/#!4/2aeff/13/0
For more on this: https://blogs.oracle.com/oraclemagazine/on-top-n-and-pagination-queries

Consider the tables below, a customer have one or multiple orders.

```sql
CREATE TABLE Customers (
    CustomerId NUMBER NOT NULL,
    Name VARCHAR2(70) NOT NULL,
    CONSTRAINT customers_pk  PRIMARY KEY(CustomerId)
);

CREATE TABLE Orders (
    OrderId NUMBER NOT NULL,
    CustomerId NUMBER NOT NULL,
    Total NUMBER NOT NULL,
    PRIMARY KEY(OrderId),
    FOREIGN KEY(CustomerId) references Customers(CustomerId)
);
```

## TOP-N Rows

Oracle provides 3 ways for counting rows:
**ROW_NUMBER** *gives unique continuous numbers from 1 to N, for each returned row*
**RANK** *assigns a rank, which are not continuous numbers*
**DENSE_RANK** *assigns a rank, but the numbers are continuous*

Let's see what we get when we execute the following:
```sql
SELECT 
    ROW_NUMBER() OVER (ORDER BY c.customerid ASC) rn,
    RANK() OVER (ORDER BY c.customerid ASC) as rnk,
    DENSE_RANK() OVER (ORDER BY c.customerid ASC) drnk,
    c.customerid, c.name, o.orderid, o.total
FROM customers c 
    INNER JOIN orders o ON (c.customerid = o.customerid)
ORDER BY c.customerid
```

| RN | Rank | DRank| CustomerId | Name | OrderId | Total
| --- | ---| --- | --- | --- | --- | --- |
| 1 | 1 | 1 | 1 | John | 18 | 588.69 |
| 2 | 2 | 2 | 2 | Patricia | 10 | 741.97 |
| 3 | 2 | 2 | 2 | Patricia | 13 | 304.89 |
| 4 | 2 | 2 | 2 | Patricia | 6 | 463.38 |
| 5 | 5 | 3 | 3 | James | 1 | 106.99 |
| 6 | 5 | 3 | 3 | James | 30 | 495.14 |
| 7 | 5 | 3 | 3 | James | 31 | 251.14 |
| 8 | 8 | 4 | 4 | Kirsten | 12 | 824.3 |
| 9 | 8 | 4 | 4 | Kirsten | 15 | 406.35 |
| 10 | 8 | 4 | 4 | Kirsten | 34 | 716.01 |
| 11 | 11 | 5 | 5 | Mark | 23 | 324.95 |
| 12 | 11 | 5 | 5 | Mark | 17 | 585.93 |
| 13 | 11 | 5 | 5 | Mark | 32 | 884.45 |
| 14 | 14 | 6 | 6 | Barri | 28 | 428.61 |
| 15 | 14 | 6 | 6 | Barri | 9 | 447.44 |
| 16 | 14 | 6 | 6 | Barri | 8 | 379.61 |
| 17 | 14 | 6 | 6 | Barri | 7 | 751.25 |
| 18 | 14 | 6 | 6 | Barri | 20 | 257.18 |
| 19 | 19 | 7 | 7 | Robert | 22 | 906.14 |
| 20 | 19 | 7 | 7 | Robert | 14 | 185.55 |
| 21 | 19 | 7 | 7 | Robert | 11 | 454.45 |
| 22 | 19 | 7 | 7 | Robert | 2 | 768.79 |
| 23 | 23 | 8 | 8 | Dora | 5 | 412.63 |
| 24 | 23 | 8 | 8 | Dora | 3 | 754.38 |
| 25 | 23 | 8 | 8 | Dora | 25 | 807.16 |
| 26 | 26 | 9 | 9 | Charlie | 26 | 206.51 |
| 27 | 26 | 9 | 9 | Charlie | 24 | 536.47 |
| 28 | 26 | 9 | 9 | Charlie | 29 | 979.98 |
| 29 | 26 | 9 | 9 | Charlie | 33 | 840.93 |
| 30 | 26 | 9 | 9 | Charlie | 4 | 578.6 |
| 31 | 31 | 10 | 10 | Marin | 27 | 501.6 |
| 32 | 31 | 10 | 10 | Marin | 16 | 379.11 |
| 33 | 31 | 10 | 10 | Marin | 19 | 484.41 |

> Here, because the CustomerIds begin at 1, they match perfectly the Dense_Rank values, but that's coincidental.

With this result set, it becomes clear that if we want to display the 5 first customers and their orders, only `Dense_Rank` would make sense.
Using `Row_Number` would return the 5 first rows: John, Patricia and James would be displayed, but James would appear to have only one order instead of 3.
`Rank` wouldn't cut it either as it returns a rank position with non-continuous numbers: 3 rows with Patricia rank `2`, there is no rank `3` or `4` because 3 rows tie to the same position. So we would have John, Patricia and James, and we expect 5 customers.
Filtering with `Dense_Rank` would return the customers John, Patricia, James, Kirsten and Mark.

## Pagination with Offset and limit

Let's say we want to paginate every 3 customers (to get a small example), ordered by Customers.CustomerId ascending.
We definately need to make use of the `Dense_rank` function. We also need to put in place an offset and a limit.
In Oracle, we can achieve it like this:

```sql
SELECT 
    pagination,
    customerid,
    name,
    orderid,
    total
FROM 
(
    SELECT 
        DENSE_RANK() OVER (ORDER BY c.customerid ASC) pagination,
        c.customerid,
        c.name,
        o.orderid,
        o.total
    FROM customers c 
        INNER JOIN orders o ON (c.customerid = o.customerid)
)
WHERE pagination BETWEEN (:offset + 1) AND (:limit + :offset)
```

With `offset` starting at `0` for the first page, and `limit` at `3`.

```sql
WHERE pagination BETWEEN (0 + 1) AND (3 + 0)
```

This gives the following result:

| Pag.| CustomerId | Name | OrderId | Total
| --- | --- | --- | --- | --- |
| 1 | 1 | John | 18 | 588.69 |
| 2 | 2 | Patricia | 10 | 741.97 |
| 2 | 2 | Patricia | 13 | 304.89 |
| 2 | 2 | Patricia | 6 | 463.38 |
| 3 | 3 | James | 1 | 106.99 |
| 3 | 3 | James | 30 | 495.14 |
| 3 | 3 | James | 31 | 251.14 |

Let's ask for the next 3 customers:
```sql
WHERE pagination BETWEEN (3 + 1) AND (3 + 3)
```
| Pag.| CustomerId | Name | OrderId | Total
| --- | --- | --- | --- | --- |
| 4 | 4 | Kirsten | 12 | 824.3 |
| 4 | 4 | Kirsten | 15 | 406.35 |
| 4 | 4 | Kirsten | 34 | 716.01 |
| 5 | 5 | Mark | 23 | 324.95 |
| 5 | 5 | Mark | 17 | 585.93 |
| 5 | 5 | Mark | 32 | 884.45 |
| 6 | 6 | Barri | 28 | 428.61 |
| 6 | 6 | Barri | 9 | 447.44 |
| 6 | 6 | Barri | 8 | 379.61 |
| 6 | 6 | Barri | 7 | 751.25 |
| 6 | 6 | Barri | 20 | 257.18 |

You get the idea!