# 13.2.10 Subquery Syntax

A subquery is a **SELECT** statement within another statement

All subquery forms and operations that the SQL standard requires are supported, as well as a few features that are MySQL-specific.

Here is an example of subquery:

```
SELECT * FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

In this example, `SELECT *  FROM t1 ...` is the outer query\(or outer statement\), and `(SELECT column1 FROM t2)` is the subquery. We say that the subquery is nested within the outer query, and in fact it is possible to nested subqueries within other subqueries, to a considerable depth. A subquery must always appear within parentheses.

The main advantages of subqueries are:

* They allow queries that are structured so that it is possible to isolate each part of a statement.
* They provide alternative ways to perform operations that would otherwise require complex joins and unions.
* Many people find subqueries more readable than complex joins or unions. Indeed, it was the innovation of subqueries that gave people the original idea of calling the early SQL "Structured Query Language."

Here is an example statement that shows the major points about subquery syntax as specified by the SQL standard and supported in MySQL:





A subquery can return a scalar\(a single value\), a single row, a single column, or a table\(one or more rows of one or more columns\). These are cladded scalar, column, row, and table subqueries. Subqueries that return a particular kind of result often can be used only in certain contexts, ad described in the following sections.

There are few restrictions on the type of statements in which subqueries can be used. A subquery can contain many of the keywords or clauses that an ordinary **SELECT** can contain: **DISTINCT**, **GROUP BY**,** ORDER BY**, **LIMIT**, joins, index hints, **UNION** constructs, comments, functions, and so on.

A subquery's outer statement can be any one of: **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **SET** or **DO.**

In MySQL, you cannot modify a table and select from the same table in a subquery. This applies to statements such as **DELETE**, **INSERT**, **REPLACE**, **UPDATE**, and \(because subqueries can be used in the **SET** clause\) **LOAD DATA INFILE.**

For information about how the optimizer handles subqueries, see Section 8.2.2, "Optimizing Subqueries, Derived Tables, and View References". For a discussion of restrictions on subquery use, including performance issues for certain forms of subquery syntax, see Section C.4, "Restrictions on Subqueries".



