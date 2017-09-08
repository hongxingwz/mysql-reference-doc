# CREATE PROCEDURE and CREATE FUNCTION Syntax

The following example shows a simple stored procedure that uses an **OUT** parameter:

```
mysql> delimiter //

mysql> CREATE PROCEDURE simpleproc (OUT param1 INT)
    -> BEGIN
    ->   SELECT count(*) into param1 from test0907.events_list;
    -> end //
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;

mysql> call simpleproc(@a);
Query OK, 1 row affected (0.00 sec)

mysql> select @a;
+-------+
| @a    |
+-------+
| 10661 |
+-------+
```

The example uses the **mysql** client **delimiter** command to change the statement delimiter from** ;** to **//** while the procedure is being defined. This enables the ; delimiter used in the procedure body to be passed through the server rather than being interpreted by **mysql **itself. See Section 23.1, "Defining Stored Programs".

