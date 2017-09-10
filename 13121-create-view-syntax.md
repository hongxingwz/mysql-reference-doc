# 13.1.21 CREATE VIEW Syntax

![](/assets/1505003547188.png)The **CREATE VIEW** statement creates a new view, or replaces an existing view if the **OR REPLACE** clause is given. If the view dose not exist, **CREATE OR REPLACE VIEW** is the same as **CREATE VIEW**. If the view does exist, **CREATE OR REPLACE VIEW** is the same as **ALTER VIEW.**

For information about restrictions on view use, see Section C.5, "Restrictions on Views".

The **select\_statement** is a **SELECT** statement that provides the definition of the view.(Selecting from the view selects, in effect, using the **SELECT** statement.) The **select\_statement** can select from base tables or other views.

The view definition is "frozen" at creation time and is not affected by subsequent changes to the definitions of the underlying tables. For example, if a view is defined as **SELECT * ** on a table, new columns added to the table later do not become part of the view, add columns dropped from the table will result in an error when selecting from the view.

The **ALGORITHM** clause affects how MySQL processes the view. The **DEFINER** and **SQL SECURITY** clauses specify the security context to be used when checking access privileges at view invocation time. The **WITH CHECK OPTION** clause can be given to constrain inserts or updates to rows in tables referenced by the view. These clauses are described later in this section.

The **CREATE VIEW** statement requires the **CREATE VIEW** privilege for the view, and some privilege for each column selected by the **SELECT** statement. For columns used elsewhere is the SELECT statement, you must have the **SELECT** privilege. If the **OR REPLACE** clause is present, you must also have the **DROP** privilege for the view. **CREATE VIEW** might also require the **SUPER** privilege, depending on the **DEFINER** value, as described later in this section.

When a view is referenced, privilege checking occurs as described later in this section.

A view belongs to a database. By default, a new view is created in the default database. To create the view explicitly in a given database, use **db\_name.view\_name** syntax to qualify the view name with the database name:


```
CREATE VIEW test.v AS SELECT * FROM t;
```
Unqualified table or view names in the **SELECT** statement are also interpreted with respect to the default database. A view can refer to tables or views in other databases by qualifying the table or view name with the appropriate database name.

Within a database, base tables and views share the same namespace, so a base table and a view cannot have the same name.

Columns retrieved by the **SELECT** statement can be simple references to table columns, or expressions that use functions, constant values, operators, and so forth.

A view must have unique column names with no duplicates, just like a base table. By default, the names of the columns retrieved by the **SELECT** statement are used for the view column names. To define explicit names for the view columns, specify the optional **column\_list** clause as a list of comma-separated identifiers. The number of names in **column\_list** must be the same as the number of columns retrieved by the **SELECT** statement.

A view can be created from many kinds of SELECT statements. It can refer to base tables or other views.It can use joins, **UNION**, and subqueries. The **SELECT** need not even refer to any tables:


```
CREATE VIEW v_today(today) as select current_date;
```
The following example defines a view that selects two columns from another table as well as an expression calculated from those columns:


```
mysql> CREATE TABLE t (qty INT, price INT);
mysql> INSERT INTO t VALUES (3, 50);
mysql> CREATE VIEW v AS SELECT qty, price, qty*price as value FROM t;
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM v;
+------+-------+-------+
| qty  | price | value |
+------+-------+-------+
|    3 |    50 |   150 |
+------+-------+-------+
1 row in set (0.00 sec)
```

A view definition is subject to the following restrictions:
* Before MySQL 5.7.7, the **SELECT** statement cannot contain a subquery in the **FROM** clause.
* The **SELECT** statement cannot refer to system variables or user-defined variables.
* Within a stored program, the **SELECT** statement cannot refer to program parameters or local variables.
* The **SELECT** statement cannot refer to prepared statement parameter.
* Any table or view referred to in the definition must exist. If, after the view has been created, table or view that the definition refers to is dropped, use of the view results in an error. To check a view definition for problems of this kind, use the **CHECK TABLE** statement.
* The definition cannot refer to a **TEMPORARY** table, and you cannot create a **TEMPORARY** view.
* You cannot associate a trigger with a view.
* Aliases for column names in the **SELECT** statement are checked against the maximum column length of 64 characters(not the maximum alias length of 256 characters).

ORDER BY is permitted in a view definition, but it is ignored if you select from a view using a statement that has its own ORDER BY.

For other options or clauses in the definition, they are added to the options or clauses of the statement that references the view, but the effect is undefined. For example, if a view definition includes a **LIMIT** clause, and you select from the view using a statement that has its own **LIMIT** clause, it is undefined which limit applies. This same principle applies to options such as **ALL**, **DISTINCT**, or **SQL\_SMALL\_RESULT** that follow the **SELECT** keyword, and to clauses such as **INTO**, **FOR UPDATE**, **LOCK IN SHARE MODE**, and **PROCEDURE**.

The results obtained from a view may be affected if you change the query processing environment by changing system variables:



```
mysql> CREATE VIEW v(mycol) AS SELECT 'abc';
ERROR 1050 (42S01): Table 'v' already exists
mysql> SET sql_mode = '';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT "mycol" FROM v;
+-------+
| mycol |
+-------+
| mycol |
+-------+
1 row in set (0.00 sec)

mysql> set sql_mode = 'ANSI_QUOTES';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT "mycol" FROM v;
+-------+
| mycol |
+-------+
| abc   |
+-------+
1 row in set (0.00 sec)
```

The **DEFINER** and **SQL SECURITY** clauses determine which MySQL account to use when checking access privileges for the view when a statement is executed that references the view. The  valid **SQL SECURITY** characteristic values are **DEFINER** (the default) and **INVOKER**. These indicate that the required privileges must be held by the user who defined or invoked the view, respectively.

If a **_user_** value is given for the **DEFINER** clause, it should be a MySQL account specified as **'user\_name'@'host\_name'**, **CURRENT\_USER**, or **CURRENT\_USER()**. The default **DEFINER** value is the user who executes the **CREATE VIEW** statement. This is the same as specifying **DEFINER = CURRENT\_USER** explicitly.

If the **DEFINER** clause is present, these rules determine the valid **DEFINER** user values:
* If you do not have the **SUPER** privilege, the only valid **_user_** value is your own account, either specified literally or by using **CURRENT\_USER**. You cannot set the definer to some other account.

* If you have the **SUPER** privilege, you can specify any syntactically valid account name. If the account does not exist, a warning is generated.

* Although it is possible to create a view with a nonexistent **DEFINER** account, an error occurs when the view is referenced if the **SQL SECURITY** value is **DEFINER** but the definer account does not exist.

For more information about view security, see Section 23.6, "Access Control for Stored Programs and Views"






