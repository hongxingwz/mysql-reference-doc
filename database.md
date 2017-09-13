# DATABASE\(\)

Returns the default \(current\) database name as a string in the **utf8** character set. If there is no default database, [DATABASE\(\)](#database) returns **NULL**.Within a stored routine, the default database is the database that the routine is associated with, which is not necessarily the same as the database that is the default in the calling context.

```
mysql> SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| test0913   |
+------------+
```

Within a stored routine, the default database is the database that the routine is associated with.

    mysql> DELIMITER $$
    mysql> CREATE PROCEDURE `test0912`.`test_select`()
        -> BEGIN
        ->   SELECT DATABASE();
        -> END;
        -> 
        -> CALL `test0912`.`test_select`;
        -> $$
    Query OK, 0 rows affected (0.00 sec)

    +------------+
    | DATABASE() |
    +------------+
    | test0912   |
    +------------+
    1 row in set (0.00 sec)

    Query OK, 0 rows affected (0.00 sec)



if there is no database, [DATABASE](#database)\(\) returns **NULL**.

```
mysql> select schema();
+----------+
| schema() |
+----------+
| NULL     |
+----------+

```



