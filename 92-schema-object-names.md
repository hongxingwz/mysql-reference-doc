# 9.2 Schema Object Names

Certain objects within MySQL, including database, table, index, column, alias, view, stored procedure, partition, tablespace, and other object names are known as identifiers.This section describes the permissible syntax for identifiers in MySQL. Section 9.2.2, "Identifier Case Sensitivity", describes which types of identifiers are case sensitive and under what conditions.

An identifier may be quoted or unquoted. If an identifier contains special characters or is a reserved word. you must quote it whenever you refer to it.\(Exception: A reserved word that follows a period in a qualified name must be an identifier, so it need not be quoted.\)Reserved words are listed at Section 9.3, "Keywords and Reserved Words".

Identifiers are converted to Unicode internally. The my contain these characters:

* ASCII:\[0-9, a-z, A-Z$\_\]\(基本的拉丁, 数字0-9，美元符号，下划线\)
* Identifiers may begin with a digit but unless quoted may not consist solely of digits
* Database, table, and column names cannot end with space characters  
  The identifier quote character is the backtick\(\`\)

  mysql&gt; SELECT \* FROM `select` WHERE `select`.id &gt; 100;

If the **ANSI\_QUOTES** SQL model is enabled, it is also permissible to quote identifiers within double quotation marks:

```
mysql> CREATE TABLE "test" (col INT);
ERROR 1064: You have an error in your SQL syntax...
mysql> SET sql_mode='ANSI_QUOTES';
mysql> CREATE TABLE "TEST"
```

The **ANSI\_QUOTES **mode causes the server to interpret double-quoted strings as identifiers. Consequently, when this mode is enabled, string literals must be enclosed with single quotation marks. They cannot be enclosed within double quotation marks. The server SQL mode is controlled as described in Section 5.1.8, "Server SQL Modes".

Identifier quote characters can be included within an identifier if you quote the identifier. If the character to be included within the identifier is the same as that used to quote the identifier itself, then you need to double the character. The following statement creates a table named a\`b that contains a column named c"d:

    mysql> CREATE TABLE `a``b` (`c"d` INT);

In the select list of a query, a quoted column alias can be specified using identifier or string quoting characters:

    mysql> select 1 as `one`, 2 as 'two';
    +-----+-----+
    | one | two |
    +-----+-----+
    |   1 |   2 |
    +-----+-----+

Elsewhere in the statement, quoted references to the alias must use identifier quoting or the reference is treated as a string literal.

It is recommended that you do not use names that begin with Me or MeN, where  M and N are integers. For example, avoid using 1e as an identifier, because an expression such as 1e + 3 is ambiguous.  Depending on context, it might be interpreted as the expression 1e + 3 or as the number 1e + 3.

使用MD5\(\)来创建表名时要小心，因为他们可以产生不合法或有歧义的形式\(像上面描述的那样\)

A user variable cannot be used directly in an SQL statement as an identifier or as part of an identifier. See Section 9.4, "User-Defined Variables", for more information and examples of workarounds。

Special characters in database and table names are encoded in the corresponding file system names as described in Section 9.2.3, "Mapping of Identifiers to File names". If you have databases or tables from an older version of MySQL that contain special characters and for which the underlying directory names or file names have not been updated to use the new encoding, the server displays their names with a prefix of \#mysql150\#. For information about referring to such names or converting them to the newer encoding, see that section.



The following table describes the maximum length for each type of identifier.

| Identifier | Maximum Length\(characters\) |
| :--- | :--- |
| Database | 64\(NDB storage engine: 63\) |
| Table | 64\(NDB storage engine: 63\) |
| Column | 64 |
| Index | 64 |
| Constraint | 64 |
| Stored Program | 64 |
| View | 64 |
| Tablespace | 64 |
| Server | 64 |
| Log File Group | 64 |
| Alias | 256\(see exception following table\) |
| Compound Statement Label | 16 |
| User-Defined Variable | 64 |

Aliases for column names in **CREATE VIEW** statements are checked against the maximum column length of 64 characters\(not the maximum alias length of 256 characters\).

Identifiers are stored using Unicode\(UTF-8\). This applies to identifiers in table definitions that are stored in **.frm** files and to identifiers stored in the grant tables in the **mysql **database. The sizes of the identifier string columns in the grant tables are measured in characters. You can use multibyte characters without reducing the number of characters permitted for values stored in these columns. As indicated earlier, the permissible Unicode characters are those in the Basic Multilingual Plane\(BMP\).Supplementary characters are not permitted.

NDB Cluster imposes a maximum length of 63 characters for names of databases and tables.See Section 21.1.6.5, "Limits Associated with Database Objects in NDB Cluster".

