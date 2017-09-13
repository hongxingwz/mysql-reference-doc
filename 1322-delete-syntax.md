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

For MyISAM tables, you can specify an **AUTO\_INCREMENT** secondary column in a multiple-column key. In this case, reuse of values deleted from the top of the sequence occurs even for MyISAM tables. See Section 3.6.9, "Using AUTO_INCREMENT"

## Modifiers
The **DELETE** statement supports the following modifers:
* If you specify **LOW_PRIORITY**, the server delays execution of the DELETE until no other clients are reading from the table. This affects only storage engines that use only table-level locking(such as MyISAM, MEMORY, and MERGE).
* For MyISAM tables, if you use the QUICK modifier, the storage engine does not merger index leaves during delete, which may speed up some kinds of delete operations.

* The **IGNORE** modifier causes MySQL to Ignore errors during the process of deleting rows.(Errors encountered during the parsing stage are processed in the usual manner.) Errors that are ignored due the use of **IGNORE** are returned as warnings. For more information, see Comparison of the IGNORE Keyword and Strict SQL Mode.

## Order of Deletion
If the **DELETE** statement includes an **ORDER BY** clause, rows are deleted in the order specified by the clause. This is useful primarily in conjunction with **LIMIT**. For example, the following statement finds rows matching the WHERE clause, sorts them by **timestamp\_column**, and deletes the first (oldest) one:


```
DELETE FROM somelog WHERE user = 'jcole'
ORDER BY timestamp\_column LIMIT 1;
```
**ORDER BY** also helps to delete rows in an order required to avoid referential integrity violations.

**InnoDB Tables**
If you are deleting many rows from a large table, you may exceed the lock table size for an **InnoDB** table. To avoid this problem, or simply to minimize the time that the table remains locked, the following strategy(which dose not use DELETE at all) might be helpful:

1. Select the rows not to be deleted into an empty table that has the same structure as the original table:


```
INSERT INTO t\_copy SELECT * FROM t WHERE ...;
```

2. Use **RENAME TABLE** to atomically move the original table out of the way and rename the copy to the original name:


```
RENAME TABLE t TO t\_old, t\_copy TO t;
```

3. Drop the original table:


```
DROP TABLE t\_old;
```

No other sessions can access the tables involved while **RENAME TABLE** executes, so the rename operation is not subject to concurrency problems. See Section 13.1.33 "RENAME TABLE Syntax".


## Multi-Table Deletes
You can specify multiple tables in a **DELETE** statement to delete rows from one or more tables depending one the condition if the **WHERE** clause. You cannot use **ORDER BY** or **LIMIT** in a multiple-table **DELETE**.The **table\_references** clause lists the tables involved in the join, as described in Section 13.2.9.2, "JOIN Syntax".

For the first multiple-table syntax, only  matching rows from the tables listed before the FROM clause are deleted. For the second multiple-table syntax, only matching rows from the tables listed in the **FROM** clause (before the **USING** clause) are deleted. The effect is that you can delete rows from many tables at the same time and have additional tables that are used only for searching:





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



