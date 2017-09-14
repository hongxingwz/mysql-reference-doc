# 13.5 Prepared SQL Statement Syntax

MySQL 5.7 provides support for server-side prepared statements.This support takes advantage of the efficient client/server binary protocol. Using prepared statements with placeholders for parameter values has the following benefits:
* Less overhead for parsing the statement each time it is executed. Typically, database applications process large volumes of almost-identical statements(大体积相同的语句), with only changes to literal or variable values in clauses such **WHERE** for queries and deletes, **SET** for updates, and **VALUES** for inserts.

* Protection against SQL injection attacks. The parameter values can contain unescaped SQL quote and delimiter characters.

## Prepared Statements in Application Programs
You can use server-side prepared statements through client programming interfaces, including the **MySQL C API client library** or **MySQL Connector/C** for C programs, **MySQL Connector/J** for java programs, and **MySQL Connector/Net** for programs using **.NET** technologies. For example, the C API provides a set of function calls that make up it prepared statement API. See Section 27.8.8, "C API Prepared Statements". Other language interfaces can provide support for prepared statements that use the binary protocol by linking in the C client library, one example being the **mysqli** extension, available in PHP5.0 and later.

## Prepared Statements in SQL Scripts
An alternative SQL interface to prepared statements is available. This interface is not as efficient as using the binary protocol through a prepared statement API, but requires no programming because it is available directly at the SQL level:
* You can use it when no programming interface is available to you.
* You can use it from any program that can send SQL statements to the server to be executed, such as the **mysql** client program.
* You can use it even if the client is using an old version of the client library, as long as you connect to a server running MySQL 4.1 or higher.

SQL syntax for prepared statements is intended to be used for situations such as these:
* To test how prepared statements work in your application before coding it.
* To use prepared statements when you do not have access to programming API that supports them.
* To interactively troubleshoot application issues with prepared statements.
* To create a test case that reproduces a problem with prepared statements, so that you can file a bug report.

## PREPARE, EXECUTE, and DEALLOCATE PREPARE Statements
SQL syntax for prepared statements is based on three SQL statements:
* PREPARE prepares a statement for execution (see Section 13.5.1, "PREPARE Syntax").
* DEALLOCATE PREPARE releases a prepared statement(see Section 13.5.3, "DEALLOCATE PREPARE Syntax").

The following examples show two equivalent ways of preparing a statement that computes the hypotenuse of a triangle given the lengths of the two sides.

The first example shows how to create a prepared statement by using a string literal to supply the text of statement:


```
PREPARE stmt1 FROM 'SELECT SQRT(POW(?, 2) + POW(?, 2)) AS pypotenuse';

SET @a = 3;
SET @B = 4;

EXECUTE stmt1 USING @a, @b;
+------------+
| pypotenuse |
+------------+
|          5 |
+------------+

mysql> DEALLOCATE PREPARE stmt1;
```

The second example is similar, but supplies the text of the statement as a user variable:


```
SET @s = 'SELECT SQRT(POW(?, 2) + POW(?, 2)) AS hypotenuse';
PREPARE stmt2 FROM @s;

SET @a = 6;
SET @b = 8;
EXECUTE stmt2 USING @a, @b;
+------------+
| hypotenuse |
+------------+
|         10 |
+------------+
mysql> DEALLOCATE PREPARE stmt2;
```
Here is an additional example that demonstrates how to choose the table on which to perform a query at runtime, by storing the name as a user variable:



```
CREATE TABLE t1 (a INT NOT NULL);
INSERT INTO t1 VALUES (4),(8), (11), (32), (80);

SET @table = 't1';
SET @s = CONCAT('SELECT * FROM  ', @table);
PREPARE stmt3 FROM @S;
EXECUTE stmt3;
+----+
| a  |
+----+
|  4 |
|  8 |
| 11 |
| 32 |
| 80 |
+----+
mysql> DEALLOCATE PREPARE stmt3;
Query OK, 0 rows affected (0.00 sec)
```

A prepared statement is specific to the session in which it was created. If you terminate a session without deallocating a previously prepared statement, the server deallocates it automatically.

A prepared statement is also global to the session. If you create a prepared statement within a stored routine, it is not deallocated when the stored routine ends.

To guard against too many prepared statements being created  simultaneously, set the **max\_prepared\_stmt\_count** system variable. To prevent the use of prepared statements, set the value to 0.




