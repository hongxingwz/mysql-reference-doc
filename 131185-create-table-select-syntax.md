# 13.1.18.5 CREATE TABLE ... SELECT Syntax
You can create one table from another by adding a **SELECT** statement at the end of the **CREATE TABLE** statement:


```
CREATE TABLE new\_tb1 [AS] SELECT * FROM orig_tb1;
```

MySQL creates new columns for all elements in the SELECT. For example:


```
mysql CREATE TABLE test(a INT NOT NULL AUTO_INCREMENT,
    PRIMARY KEY(a), KEY(b))
    ENGINE=MyISAM SELECT b, c FROM test2;

```
This creates a MyISAM table with three columns, a, b, and c. The **ENGINE** option is part of the **CREATE TABLE** statement, and should not be used following the SELECT; this would result in a syntax error. The same is true for other **CREATE TABLE** options such as **CHARSET**.

Notice that the columns from the **SELECT** statement are appended to the right side of the table, not overlapped onto it. Take the following example:



```
mysql> select * from foo;
+------+
| n    |
+------+
| 1    |
+------+
1 row in set (0.00 sec)
mysql> create table bar (m int) select n from foo;
Query OK, 1 row affected (0.02 sec)
Records: 1  Duplicates: 0  Warnings: 0

mysql> select * from bar;
+------+------+
| m    | n    |
+------+------+
| NULL | 1    |
+------+------+
1 row in set (0.00 sec)

mysql> 

```
For each row in table **foo**, a row is inserted in **bar**  with the values from **foo** and default values for the new columns.

In a table resulting from **CREATE TABLE ... SELECT**, columns named only in the **CREATE TABLE** part come first. Columns named in both parts or only in the **SELECT** part come after that. The data type of **SELECT** columns can be overridden by also specifying the column in the **CREATE TABLE** part.

If any errors occur while copying the data to the table, it is automatically dropped and not created .

You can precede the **SELECT** by **IGNORE** or **REPLACE** to indicate how to handle rows that duplicate unique key values. With **IGNORE**, rows that duplicate an existing row on a unique key value are discarded. With **REPLACE**, new rows replace rows that have the same unique key value. If neither IGNORE nor REPLACE is specified, duplicate unique key values result in an error. For more information, see **Comparison of the IGNORE Keyword and  Strict SQL Mode**

Because the ordering of the rows in the underlying **SELECT** statements cannot always be determined, **CREATE TABLE ... IGNORE SELECT** and **CREATE TABLE ... REPLACE SELECT** statements are flagged as unsafe for statement-based replication. Such statements produce a warning in the error log when using statement-based mode and are written to the binary log using the row-based format when using **MIXED** mode. See also 16.2.1.1, "Advantages and Disadvantages of Statement-Based and Row-Based Replication".

**CREATE TABLE ... SELECT** does not automatically create any indexes for you. This is done intentionally to make the statement as flexible as possible. If you want to have indexes in the created table, you should specify these before the SELECT statement:


```
mysql> CREATE TABLE bar (UNIQUE (n)) SELECT n FROM foo;
```
For **CREATE TABLE ... SELECT**, the destination table does not preserve information about whether columns in the selected-from table are generated columns. The **SELECT** part of the statement cannot assign values to generated columns in the destination table.




