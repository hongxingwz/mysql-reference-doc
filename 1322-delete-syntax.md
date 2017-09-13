# 13.2.2 DELETE Syntax

**DELETE **is a DML statement that removes rows from a table.

## Single-Table Syntax

![](/assets/1505284269759.png)The **DELETE **statement deletes rows from _tbl\_name_ and returns the number of deleted rows. To check the number of deleted rows, call the ROW\_COUNT\(\) function described in  [Section 12.14, "Information Functions".](/1214-information-functions.md)

## Main Clauses

The conditions in the optional **WHERE** clause identify which rows to delete. With no **WHERE** clause, all rows are deleted. _where\_condition_ is an expression that evaluates to true for each row to be deleted. It is specified as described in Section 13.2.9, "SELECT Syntax".

If the **ORDER BY** clause is specified, the rows are deleted in the order that is specified. The **LIMIT** clause places a limit on the number of rows that can be deleted. These clauses apply to single-table deletes, but not multi-table deletes.

## Multiple-Table Syntax

![](/assets/1505297486261.png)

## Privileges

You need the **DELETE** privilege on a table to delete rows from it. You need only the **SELECT** privilege for any columns that are only read, such as those named in the **WHERE** clause.

## Performance

When you do not need to know the number of deleted rows, the **TRUNCATE TABLE** statement is a faster way to empty a table than a **DELETE **with no **WHERE** clause.  Unlike **DELETE**，**TRUNCATE TABLE** cannot be used within a transaction or if you have a lock on the table. See Section 13.1.34, "TRUNCATE TABLE Syntax" and [Section 13.3.5, "LOCK TABLES and UNLOCK TABLES Syntax"](/1335-lock-tables-and-unlock-tables-syntax.md).

The speed of delete operations may also be affected by factors discussed in Section 8.2.4.3, "Optimizing DELETE Statements."

To ensure that a give DELETE statement does not take too mush time, the MySQL-specific** LIMIT **_**row\_count **_ clause for **DELETE** specifies the maximum number of rows to be deleted. If the number of rows to delete is larger than the limit, repeat the **DELETE** statement until the number of affected rows is less than the **LIMIT** value

## Auto-Increment Columns
If you delete the row containing the maximum value for an **AUTO\_INCREMENT** column, the value is not reused for a MyISAM or InnoDB table. If you delete all rows in the table with `DELETE FROM tbl_name`(without a **WHERE** clause) in **autocommit** mode, the sequence starts over for all storage engines except **InnoDB** and **MyISAM**. There are some exceptions to this behavior for **InnoDB** tables, as discussed in Section 14.8.1.5, "AUTO_INCREMENT Handling in InnoDB".

## 总结：

### DELETE与TRUNCATE

    mysql> select count(*) from `test0913`.`BIG_RESULT`;
    +----------+
    | count(*) |
    +----------+
    |  3656488 |
    +----------+
    1 row in set (0.41 sec)

    mysql> begin;
    Query OK, 0 rows affected (0.00 sec)

    mysql> delete from `test0913`.`BIG_RESULT`;
    Query OK, 3726683 rows affected (10.51 sec)

    mysql> rollback;
    Query OK, 0 rows affected (40.76 sec)

    mysql> commit;
    Query OK, 0 rows affected (0.00 sec)

    mysql> truncate `test0913`.`BIG_RESULT`;
    Query OK, 0 rows affected (3.94 sec)

可以看到delete用时(10.51 sec)，而truncate用时(3.94 sec)。



