# 13.2.10.5 Row Subqueries
Scalar or column subqueries return a single value or a column of values. A row subquery is a  subquery variant that returns a single row and can thus return more than one column value. Legal operators for row subquery comparisons are:


```
= > < >= <= <> ! <=>
```

Here are two examples:



```
SELECT * FROM t1
    WHERE (col1, col2) = (SELECT col3, col4 FROM t2 WHERE id = 10);
    
SELECT * FROM t1
    WHERE ROW(col1, col2) = (SELECT col3, col4 FROM t2 WHERE id = 10);
```
For both queries, if the table **t2** contains a single row with **id = 10**, the subquery returns a single row.If this row has **col3** and **col4** values equal to the **col1** and **col2** values of any rows in **t1**, the **WHERE** expression is **TRUE** and  each query returns those **t1** rows. If the **t2** row **col3** and **col4** values are not equal the **col1** and **col2** values of any **t1** row, the expression is **FALSE** and the query returns an empty result set. The expression is unknown(that is, NULL) if the subquery produces no rows. An error occurs if the subquery produces multiple rows because a row subquery can return at most one row.

