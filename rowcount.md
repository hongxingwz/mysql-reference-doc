# ROW\_COUNT()
**ROW_COUNT()** returns a value as follows:
* DDL statements: 0. This applies to statements such as **CREATE TABLE** or **DROP TABLE**.
* DML statements other than **SELECT**:The number of affected rows. This applies to statements such as **UPDATE**, **INSERT**, Or **DELETE**(as before), but now also to statements such as **ALTER TABLE** and **LOAD DATA INFILE.**
* **SELECT**: -1 if the statement return a result set, or the number of rows "affected" if it does not. For example, for `SELECT * FROM t1, ROW_COUNT()` returns -1. For `SELECT * FROM t1 INTO OUTFILE 'file_name'`, **ROW\_COUNT()** returns the number of rows written to the file.
* **SIGNAL** statements: 0

For **UPDATE** statements, the affected-rows value by default is the number of rows actually changed.If you specify the **CLIENT\_FOUND\_ROWS** flag to **mysql\_real\_connect()** when connection to **mysqld**, the affected-rows value is the number of rows "found"; that is, matched by the **WHERE** clause.

For **REPLACE** statements, the affected-rows value is 2 if the new row replaced an old row, because in this case, one row was inserted after the duplicate was delete.

For `INSERT ... ON DUPLICATE KEY UPDATE` statements, the affected-rows value per row is 1 if the row is inserted as a new row, 2 if an existing row is updated, and 0 if the existing row is set to its current values. If you specify the **CLIENT\_FOUND\_ROWS** flag, the affected-rows value is 1 (not 0) if an existing row is set to its current values.

The **ROW\_COUNT()** value is similar to the value from the **mysql\_affected\rows()** C API function and the row count that the **mysql** client displays following statement execution.


```
mysql> insert into Person values ("4", "bob@example.com"), (5, "hongxingwz@gmail.com");
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select row_count();
+-------------+
| row_count() |
+-------------+
|           2 |
+-------------+

mysql> delete from Person  where  id in  (4, 5);
Query OK, 2 rows affected (0.00 sec)

mysql> select row_count();
+-------------+
| row_count() |
+-------------+
|           2 |
+-------------+

```

> Important
>
> ROW_COUNT() is not replicated reliably using statement-based replication. This function is automatically replicated using row-based replication.


