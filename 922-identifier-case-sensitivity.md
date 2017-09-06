# 9.2.2 Identifier Case Sensitivity

In Mysql, databases correspond to directories within the data directory.Each table within a database corresponds to at least on file within the database directory\(and possibly more, depending on the storage engine\). Triggers also correspond to files. Consequently, the case sensitivity of the underlying operating system plays a part in the case sensitivity of database,  table, and trigger names. This means such names are not case sensitive in Windows, but are case sensitive in most varieties of Unix. One notable exception is OS X, which is Unix-based but uses a default file system type\(FSF+\) that is not case sensitive. However, OS X also supports UFS volumes, which are case sensitive just as on any Unix. See Section 1.8.1, "MySQL Extensions to Standard SQL". The lower\_case\_table\_names system variable also affects how the server handles identifier case sensitivity, as described later in this section.

> Note
>
> Although database, table, and trigger names are not case sensitive on some platforms, you should not refer to one of these using different cases within the same statement. The following statement would not work because it refers to a table both as my\_table and as MY\_TABLE:  
> `mysql> SELECT * FROM my_table WHERE MY_TABLE.col=1`

Column, index, stored routine, and event names are not case sensitive on any platform, nor are column aliases.

However, names of logfile groups are case sensitive. This differs from standard SQL.

By default, table aliases are case sensitive on Unix, but not so on Windows or OS X. The following statement would not work on Unix, because it refers to the alias both as **a** and as **A**:

```
mysql> SELECT col_name FROM tb1_name AS a WHERE a.col_name = 1 or A.col_name = 2;
```

However, this same statement is permitted on Windows. To avoid problems caused by such differences, it is best to adopt a consistent convention, such as always creating and referring to databases and tables using lowercase names. This convention is recommended for maximum portability and ease of use.

How table and database names are stored on disk and used in MySQL is affected by the lower\_case\_table\_names system variable, which you can set when starting **mysqld**. lower\_case\_table\_names can take the values shown in the following table. This variable does not affect case sensitivity of trigger identifiers. On Unix the default value of lower\_case\_table\_names is 0. On Windows, the default value is 1. On OS X, the default value is 2.

| Value | Meaning |
| :--- | :--- |
| 0 | Table and database names are stored on disk using the letter specified in the CREATE TABLE or CREATE DATABASE statement. Name comparisons are case sensitive. You should not set this variable to 0 if you are running MySQL on a system that has case-insensitive file names\(such as Windows or OS X\).If you force this variable to 0 with --lower-case-table-names=0 on a case-insensitive file system and access MyISAM tablenames using different lettercases, index corruption may result. |



