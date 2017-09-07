# 9.4 User-Defined Variables

You can store a value in a user-defined variable in one statement and refer to it later in another statement. This enables you to pass values from one statement to another.

User variables are written as **@var\_name**, where the variable name **var\_name** consists of alphanumeric character,**.**, **\_**和**$**. A user variable name can contain other characters if you quote it as a string or identifier\(for example, @'my-var', @"my-var", 或@```my-var``\).

User-defined variables are session specific. A user variable defined by one client cannot be seen or used by other clients.\(Exception: A user with access to be Performance Schema user\_variables\_by\_thread table can see all variables for all sessions.\)All variables for a give client session are automatically freed when that client exists.

User variable names are not case sensitive. Names have a maximum length of 64 characters.

One way to set a user-defined variable is by issuing a SET statement:

```
SET @var_name = expr[, @var_name = expr] ...
```

For **SET, **either **= **or **:=** can be used as the assignment operator.

You can also assign a value to user variable in statements other than SET. In this case, the assignment operator must be **:= **and not **= **because the latter is treated as the comparison operator **= in **non**-SET **statements:

```
mysql> SET @t1=1, @t2=2, @t3 := 4;
Query OK, 0 rows affected (0.00 sec)

mysql> select @t1, @t2, @t3, @t4 := @t1 + @t2 + @t3;
+------+------+------+------------------------+
| @t1  | @t2  | @t3  | @t4 := @t1 + @t2 + @t3 |
+------+------+------+------------------------+
|    1 |    2 |    4 |                      7 |
+------+------+------+------------------------+
```

User variables can be assigned a value from a limited set of data types: integer, decimal, floating-point, binary or nonbinary string, or **NULL **value. Assignment of decimal and real values does not preserve the precision or scale of the value. A value of a type other than one of the permissible types is converted to a permissible type. For example, a value having a temporal or spatial data type is converted to a binary string. A value having the JSON data type is converted to a string with a character set of utf8mb4 and a collation of utf8mb4\_bin.

If a user variable is assigned a nonbinary\(character\) string  value, it has the same character set and collation as the string.The coercibility of user variables is implicit.\(This is the same coercibility as for table column value\).

Hexadcimal or bit values assigned to user variables are treated as binary strings. To assign a hexadecimal or bit value as a number to a user variable use it in numeric context. For example, add 0 or user CAST\(... AS UNSIGNED\)

```
SET @v1 = X'41';
SET @v2 = X'41' + 0;
SET @v3 = CAST(X'41' as unsigned);
select @v1, @v2, @v3;
+------+------+------+
| @v1  | @v2  | @v3  |
+------+------+------+
| A    |   65 |   65 |
+------+------+------+

SET @v1 = b'1000001';
SET @v2 = b'1000001' + 0;
SET @v3 = CAST(b'1000001' as unsigned);
SELECT @v1, @v2, @v3;
+------+------+------+
| @v1  | @v2  | @v3  |
+------+------+------+
| A    |   65 |   65 |
+------+------+------+
```

If the value of a user variable is selected in a result set, it is returned to the client as **string**.

If you refer to a variable that has not been initialized, it has a value of **NULL** and a type of string.

User variables may be used in most contexts where expressions are permitted. This does not currently include contexts that explicitly require a literal value, such as in the LIMIT clause of a SELECT statement, or the IGNORE N LINES clause of a LOAD DATA statement.

As a general rule, other than in SET statements, you should never assign a value to a user variable and read the value within the same statement. For example, to increment a variable, this is okay:

```
SET  @a = @a + 1;
```

For other statements, such as SELECT, you might get the results you expect, but this is not guaranteed. In the following statement, you might think that MySQL will evaluate @a first and the do an assignment second:

```
SELECT  @a, @a: = @a + 1;
```

However, the order of evaluation for expressions involving user variables in undefined.

Another issue with assigning a value to a variable and reading the value within the same non-SET statement is that the default result type of a variable is based on its type at the start of the statement. The following example illustrates this:

```
mysql> SET @a = 'test';
mysql> SELECT  @a, (@a := 20) ;
```

For this **SELECT** statement, MySQL reports to the client that column one is a string and converts all accesses of **@a** to Strings, even though **@a **is regarded as a number for the next statement.

To avoid problems with this behavior, either do not assign a value to and read the value of the same variable within a single statement, or else set the variable to 0, 0.0, or ' ' to define its type before you use it.

In a SELECT statement, each select expression is evaluated only when sent to the client. This means that in a HAVING, GROUP BY, or ORDER BY clause, referring to a variable that is assigned a value in the select expression list does not work as expected:

```
mysql> SELECT (@aa := id) AS a, (@aa + 3) AS b FROM tbl_name HAVING b=5;
```

The reference to **b **in the** HAVING** clause refers to an alias for an expression in the select list that uses** @aa.**This does not work as expected: @aa contains the value of **id **from the previous selected now, not from the current row.

User variables are intended to provide data values. They cannot be used directly in an SQL statement as an identifier or as part of an identifier, such as in contexts where  a table or database name is expected, or as a reserved word such as SELECT.This is true even if the variable is quoted, as shown in the following example:

    mysql> SELECT c1 FROM t;
    +------+
    | c1   |
    +------+
    |    0 |
    |    1 |
    +------+

    mysql> SET @col = 'cl';
    Query OK, 0 rows affected (0.00 sec)

    mysql> SELECT @col FROM  t;
    +------+
    | @col |
    +------+
    | cl   |
    | cl   |
    +------+

    mysql> select `@col` FROM t;
    ERROR 1054 (42S22): Unknown column '@col' in 'field list'

    mysql> SELECT  @col FROM t;
    +------+
    | @col |
    +------+
    | `c1` |
    | `c1` |
    +------+
    2 rows in set (0.01 sec)

An exception to this principle that user variables cannot be used to provide identifiers, is when you are constructing a string for use as a prepared statement to execute later. In this case, user variables can be used to provide any part of the statement. The following example illustrates how this can be done:

```
mysql> SET @c = "c1";
Query OK, 0 rows affected (0.00 sec)

mysql> SET @s = CONCAT("SELECT ", @c, " FROM t");
Query OK, 0 rows affected (0.00 sec)

mysql> select @s;
+------------------+
| @s               |
+------------------+
| SELECT c1 FROM t |
+------------------+
1 row in set (0.00 sec)

mysql> PREPARE stmt from @s;
Query OK, 0 rows affected (0.01 sec)
Statement prepared

mysql> execute stmt;
+------+
| c1   |
+------+
|    0 |
|    1 |
+------+
2 rows in set (0.00 sec)
```

See Section 13.5 "Prepared SQL Statement Syntax", for more information.

