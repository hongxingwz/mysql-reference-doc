# MySQL Extensions to Standard SQL

MySQL Server supports some extensions that you probably will not find in other SQL DBMSs. Be warned that if you use them, your code will not be portable to other SQL servers. In some cases, you can write code that includes MySQL extensions, but is still portable, by using comments of the following form:

```
/*! MySQL-specific code */
```

In this case, MySQL Server parses and executes the code within the comment as it would any other SQL statement, but other SQL servers will ignore the extensions. For example, MySQL Server recognizes the  **STRAIGHT\_JSON** keyword in the following statement, but other servers will not:

```
SELECT /*! STRAIGHT_JSON */ col1 FROM table1, table2 WHERE ...
```

if you add a version number after the **!** character, the syntax within the comment is executed only if the MySQL version is greater than or equal to the specified version number. The **KEY\_BLOCK\_SIZE** clause in the following comment is executed only by servers from MySQL 5.1.10 or higher:

```
CREATE TABLE t1(a INT, KEY(a)) /*!50110 KEY_BLOCK_SIZE=1024 */
```

* Organization of data on disk
  MySQL Server maps each database to a directory under the MySQL data directory, and maps tables within a database to file names in the database directory.This has a few implications:
  * Database and table name are case sensitive in MySQL Server on operating systems that have case-sensitive file names\(such as most Unix systems\). See Section 9.2.2,  "Identifier Case Sensitivity".
  * You can use standard system commands to back up, rename, move, delete, and copy tables that are managed by the **MyiSAM **storage engine. For example, it is possible to rename a **MyISAM **table by renaming the **.MYD**, **.MYI**, and** .frm **files to which the table corresponds.\(Nevertheless, it is preferable to use **RENAME TABLE** or **ALTER TABLE ... RENAME** and let the server rename the files. \)
* General language syntax
  * By default, strings can be enclosed by **" **as well as **'**. if the **ANSI\_QUOTES **SQL mode is enabled, strings can be enclosed only by **'** and server interprets strings enclosed by **" ** as identifiers.
  * ** **is the escape character in strings.
  * In SQL statements, you can access tables from different databases with the db\_name.tb1\_name syntax. Some SQL servers provide the same functionality but call this **User space**. MySQL Server doesn't support tablespaces such as used in statements like this:**CREATE TABLE ralph.my\_table ... IN my\_tablespace**.
* SQL statement syntax
  * The **ANALYZE TABLE, CHECK TABLE, OPTIMIZE TABLE, **AND **REPAIR TABLE **statements**.**
  * The **CREATE DATABASE**, **DROP DATABASE**, and **ALTER DATABASE **statements. See section 13.1.11, "CREATE DATABASE Syntax", Section 13.1.22, "DROP DATABASE Syntax", and Section 13.1.1, "ALTER DATABASE Syntax".
  * The **DO** statement.
  * **EXPLAIN SELECT** to obtain a description of how tables are processed by the query optimizer.
  * The **FLUSH**  and **RESET** statements.
  * The **SET **statement. See Section 13.7.5, "SHOW Syntax". The information produced by many of the MySQL-specific **SHOW **statements can be obtained in more standard fashion by using **SELECT **to query **INFORMATION\_SCHEMA. **SEE CHAPTER 24, INFORMATION\_SCHEMA Tables.
  * Use of **LOAD DATA INFILE**. In many cases, this syntax is compatible with Oracle's **LOAD DATA INFILE. **See Section 13.2.6, "**LOAD DATA INFILE Syntax**".
  * Use of **RENAME TABLE**. See Section 13.1.33, "**RENAME TABLE Syntax**".
  * Use of **REPLACE **instead of **DELETE **plus **INSERT. **See Section 13.2.8,"**REPLACE Syntax**".
  * Use of **CHANGE col\_name**, **DROP col\_name**, or **DROP INDEX**, **IGNORE** or **RENAME** in **ALTER TABLE** statements. Use of multiple **ADD**, **ALTER**, **DROP**, or **CHANGE** clauses in an **ALTER TABLE** statement. See Section 13.1.8, "**ALTER TABLE Syntax**".
  * Use of index names, indexes on a prefix of a column, and use of **INDEX** or **KEY** in **CREATE TABLE** statements. See Section 13.1.18, "CREATE TABLE Syntax".
  * Use of **TEMPORARY** or **IF NOT EXISTS** with **CREATE TABLE**.
  * Use of **IF EXISTS** with **DROP TABLE** and **DROP DATABASE**.
  * The capability of dropping multiple tables with a single **DROP TABLE** statement.
  * The **ORDER BY** and **LIMIT** clauses of the **UPDATE** and **DELETE** statements.
  * The **DELAYED** clause of the **INSERT** and **REPLACE** statements.
  * The **LOW\_PRIORITY** clause of the **INSERT**, **REPLACE**, **DELETE**, and **UPDATE** statements
  * Use of **INTO OUTFILE** or **INTO DUMPFILE** in **SELECT** statements. See Section 13.2.9, "SELECT Syntax".
  * Options such as **STARIGHT\_JOIN** or **SQL\_SMALL\_RESULT** in **SELECT** statements.
  * You don't need to name all selected columns in the **GROUP BY** clause. This gives better performance for some very specific, but quite normal queries. See Section 12.19, "**Aggregate (GROUP BY) Functions**"
  * You can specify **ASC** and **DESC** with **GROUP BY**, not just with **ORDER BY**.
  * The ability to set variables in statement with the **:=** assignment operator. See Section 9.4, "User-Defined Variables"
* Data types
  * The **MEDIUMINT**, **SET**, and **ENUM** data types, and the various **BLOB** and **TEXT** data types.
  * The **AUTO\_INCREMENT**, **BINARY**, **NULL**, **UNSIGNED**, and **ZEROFILL** data type attributes.
  
* Functions and operators
  * To make it easier for users who migrate from other SQL environments, MySQL Server supports aliases for many functions. For example, all string functions support both standard SQL syntax and ODBC syntax.
  * MySQL Server understands the **||** and **&&** operators to mean logical **OR** and **AND**, as in the C programming language. In MySQL Server, **||** and **OR** are synonyms, as are **&&** and **AND**. Because of this nice syntax, MySQL Server doesn't support the standard SQL **||** operator for string concatenation; use **CONCAT()** instead. Because **CONCAT()** takes any number of arguments, it is easy to convert use of the **||** operator to MySQL Server.
  * Use of **COUNT(DISTINCT value\_list)** where **value\_list** has more than one element.
  * String comparisons are case insensitive by default, with sort ordering determined by the collation of the current character set, which is **latin1**(cp1252 West European) by default. To perform case-sensitive comparisons instead, you should declare your columns with the **BINARY** attribute or use the **BINARY** cast, which causes comparisons to be done using the underlying character code values rather than a lexical ordering.
  * The **%** operator is a synonym for MOD(). That is, **N % M** is equivalent to **MOD(N, M)**. **%** is supported for C programmers and for compatibility with **PostgreSQL**.
  * 


