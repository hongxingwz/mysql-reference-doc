# 13.2.9.3 UNION Syntax

![](/assets/1505194432685.png)[UNION](#13293-union-syntax) is used to combine the result from multiple SELECT statement into a single result set.

The column names from the first **SELECT** statement are used as the column names for the results returned. Selected columns listed in corresponding positions of each SELECT statement should have the same data type.(For example, the first column selected by the first statement should have the same type as the first column selected by the other statements.)

If the data types of corresponding **SELECT** columns do not match, the types and lengths of the columns in the **UNION ** result take into account the values retrieved by all of the **SELECT**  statements. For example, consider the following:
mysql> select repeat('a', 1) as n union select repeat('b', 10)
    -> ;
+------------+
| n          |
+------------+
| a          |
| bbbbbbbbbb |
+------------+

The **SELECT** statements are normal select statements, but with the following  restrictions:
* Only the last **SELECT** statement can use **INTO OUTFILE**(However, the entire **UNION** result is written to the file.)
* **HIGH\_PRIORITY** cannot be used with **SELECT** statements that are part of a **UNION**. If you specify it for the first SELECT, it has no effect. If you specify it for any subsequent **SELECT** statements, a syntax error results.


The default behavior for UNION is that duplicate rows are removed from the result. The optional **DISTINCT** keyword has no effect other than the default because it also specifies duplicate-row removal. With the optional **ALL** keyword, duplicate-row removal does not occur and the result includes all matching rows from all the **SELECT** statements. 

You can mix **UNION ALL** and **UNION DISTINCT** in the same query. Mixed **UNION** types are treated such that a **DISTINCT** union overrides any **ALL** union to its left. A **DISINCT** union can be produced explicitly by using **UNION DISTINCT** or implicitly by using **UNION** with on following **DISTINCT** or **ALL** keyword.

To apply **ORDER BY** or **LIMIT** to an individual **SELECT**, place the clause inside the parentheses that enclose the **SELECT**:


```
(SELECT a FROM t1 WHERE a = 10 AND B = 1 ORDER BY  a LIMIT 10) UNION
 10)
```

> 注
>
> Previous versions of MySQL may permit such statements without parentheses. In MySQL 5.7, the requirement for parentheses is enforced.

Use of **ORDER BY** for individual **SELECT** statements implies nothing about the order in which the rows appear in the final result because **UNION** by default produces can unordered set of rows. Therefore, the use of **ORDER BY** in this context is typically in conjunction with **LIMIT**, so that it is used to determine the subset of the selected rows to retrieve for the **SELECT**, even though it does not necessarily  affect the order of those rows in the final **UNION** result. If **ORDER BY**  appears without **LIMIT** in a **SELECT**, it is optimized away because it will have no effect anyway.

To use an **ORDER BY** or **LIMIT** clause to sort or limit the entire **UNION** result,  parenthesize the individual **SELECT** statements and place the **ORDER BY** or **LIMIT** after the last one. The following example uses both clauses:


```
(SELECT a FROM t1 WHERE  a = 10 AND B = 1)
UNION
(SELECT a FROM t2 WHERE a = 11 AND B = 2)
ORDER BY a LIMIT  10;
```

A statement without parentheses is equivalent to one parenthesized as just shown.

This kind of **ORDER BY** cannot use column references that include a table name(that is , names in `tbl_name.col_name` format). Instead, provide a column alias in the first **SELECT** statement and refer to the alias in the **ORDER BY**.(Alternatively, refer to the column in the **ORDER BY** using its column position. However, use of column positions is deprecated.)

Also, if a column to be sorted is aliased, the **ORDER BY**  clause must refer the alias, not the column name. The first of the flowing  statements will work, but the second will fail with an Unknown column 'a' in 'order clause' error:


```
(SELECT a AS b FROM t) UNION (SELECT ...) ORDER BY  b;
(SELECT a AS b FROM t ) UNION (SELECT ...) ORDER BY  a;
```
To cause rows in a **UNION** result to consist of the sets of rows retrIeved by each **SELECT** one after the other, select an additional column in each **SELECT** to use as a sort column and add an **ORDER BY** following the last SELECT:




Use of an additional column also enables you to determine which **SELECT** each row comes from. Extra columns can provide other identifying information as well, such as a string that indicates a table name.

**UNION** queries with an aggregate function in an ORDER BY  clause are rejected with an **ER\_AGGREGATE\_ORDER\_FOR\_UNION** error. Example:


```
SELECT 1 AS  foo UNION SELECT 2 ORDER BY MAX(1);
```



##总结：
* 第一条SELECT的列名作为返回结果集的列名
* 相对应的数据应该具有相同的数据类型，如果不对应则该列的类型和长度由该列所有数据比较后决定。
---
* 仅最后一条SELECT语句可以具有INTO OUTFILE(然而，整个UNION的结果集会被写入进文件)
* HIGH_PRIORITY不能用在UNION的SELECT语句中，如果在第一个SELECT语句中指定了，则没有效果，如果在后续的SELECT语句中指定了，将会发生错误结果。
---
* UNION返回的结果集会重（DISTINCT 具有同样的效果）。 (UNION ALL)返回所有匹配的记录。







