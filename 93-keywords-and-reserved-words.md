# Keywords and Reserved Words

Keywords are words that have significance in SQL. Certain keywords, such as SELECT, DELETE, or BIGINT, are reserved and require special treatment for use as identifiers such as table and column names.This may also be true for the names of built-in functions.

Nonreserved keywords are permitted as identifiers without quoting. Reserved words are permitted as identifiers if you quote them as described in Section 9.2, "Schema Object Names":

```
mysql> CREATE TABLE interval (begin INT, end INT);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'interval (begin INT, end INT)' at line 1
```

BEGIN and END are keywords but not reserved, so their use as   identifiers does not require quoting. INTERVAL is a reserved keyword and must be quoted to be used as an identifier:

    mysql> CREATE TABLE IF NOT EXISTS `interval`(begin INT, end INT);
    Query OK, 0 rows affected (0.01 sec)

Exception: A word that follows a period in a qualified name must be an identifier, so it need not be quoted even if it is reserved:

    mysql> CREATE TABLE IF NOT EXISTS mydb.`interval`(begin INT, end INT);
    Query OK, 0 rows affected (0.01 sec)

Names of built-in functions are permitted as identifiers but may require care to be used as such. For example, COUNT is a acceptable as a column name. However, by default, no whitespace is permitted in function invocations between the function name and the following\(character. This requirement enables the parser to distinguish whether the name is used in a function call or in nonfunction context.For further details on recognition of function names, see Section 9.2.4, "Function Name Parsing and Resolution"\).

The following table shows the keywords and reserved words in MySQL5.7, along with changes to individual words from version to version. Reserved keywords are marked with\(R\). In addition, \_FILENAME is reserved.

At some point, you might upgrade to a higher version, so it is a good idea to have a look at future reserved words, too. you can find these in the manuals that cover higher versions of MySQL. Most of the reserved words in the table are forbidden by standard SQL as column or table names\(for example GROUP\). A few are reserved because MySQL needs them and uses a **yacc** parse

