# 9.2 Schema Object Names

Certain objects within MySQL, including database, table, index, column, alias, view, stored procedure, partition, tablespace, and other object names are known as identifiers.This section describes the permissible syntax for identifiers in MySQL. Section 9.2.2, "Identifier Case Sensitivity", describes which types of identifiers are case sensitive and under what conditions.

An identifier may be quoted or unquoted. If an identifier contains special characters or is a reserved word. you must quote it whenever you refer to it.\(Exception: A reserved word that follows a period in a qualified name must be an identifier, so it need not be quoted.\)Reserved words are listed at Section 9.3, "Keywords and Reserved Words".

Identifiers are converted to Unicode internally. The my contain these characters:

* ASCII:\[0-9, a-z, A-Z$\_\]\(基本的拉丁, 数字0-9，美元符号，下划线\)
* Identifiers may begin with a digit but unless quoted may not consist solely of digits
* Database, table, and column names cannot end with space characters
  The identifier quote character is the backtick\(\`\)

    mysql> SELECT * FROM `select` WHERE `select`.id > 100;

If the ANSI\_QUOTES SQL model is enabled, it is also permissible to quote identifiers within double quotation marks:

```
mysql> CREATE TABLE "test" (col INT);
ERROR 1064: You have an error in your SQL syntax...
mysql> SET sql_mode='ANSI_QUOTES';
mysql> CREATE TABLE "TEST"
```



