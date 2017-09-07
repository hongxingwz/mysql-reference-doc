# 9.6 Comment Syntax

MySQL Server supports three comment styles:

* From a \# character to the end of the line.
* From a -- sequence to the end of the line. In MySQL, the --\(double-dash\) comment style requires the second dash to be followed at least one whitespace or control character\(such as a space, tab, newline, and so on\). This syntax differs slightly from standard SQL comment syntax, as discussed in Section 1.8.2.4, "'--' as the Start of a Comment".
* From a /\* sequence to the following \*/ sequence, as in the C programming language. This syntax enables a comment extend over multiple lines because the beginning and closing sequences need not be on the same line.

The following example demonstrates all three comment styles:

```
mysql> SELECT 1 + 1; # This comment continues to the end of line
mysql> SELECT 1 + 1; -- This comment continues to the end of line
mysql> SELECT 1 /* this is an in-line comment */ + 1;
mysql> SELECT 1 +
/*
this is a 
mltiple-line comment
*/
1;
```

Nested comments are not supportec.\(Under some conditons, nestd comments might be permitted, but usually are not, and users should avoid them.\)

MySQL Server supports some variants of C-style comments. These enable you to write code that includes MySQL extensions, but is still portable, by using comments of the following form:

```
/*! MySQL-specific code */
```

in this case, MySQLServer parses and executes the code within the comment as it would any other SQL statement, but other SQL servers will ignore the extensions. For example, MySQL recoginizes the STRAIGHT\_JSOIN keyword in the following statement, but other servers will not:

```
SELECT /*! STARIGHT_JOSIN */ col1 FROM  table1, table2 WHERE ...
```

if you add a version number after the ! character, the syntax within the comment is executed only if the MySQL version is greate than or equal to the specified version number. The KEY\_BLOCK\_SIZE keyword in the following comments is executed only by servers from MySQL 5.1.10 or higher:

```
CREATE TABLE t1(a INT, key (a)) /*!50110 KEY_BLOCK_SIZE=1024 */;
```

The comment syntax just described applies to how the **mysqld **server parses SQL statements**. **The **mysql **client program also performs some parsing of statements before sending them to the server.\(it does this to determine statement boundaries within a multiple-statement input line.\)

Comments in this format, /\*!12345 ... *\/, are not stoed on the server. If this format is used to comment stored routines, the comments will not be retained on the server.

Another variant of C-style comment syntax is used to specify optimizer hints. Hint comments include a + character following the /\* comment opening sequence. Example:


```
SELECT /*+ BKA(t1) */ FROM ...;
```
The use of short-form **mysql **commands such as \C within multiple-line /\* ... \*/ comments is not supported.



