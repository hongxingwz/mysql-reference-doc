# 13.2.9.1 SELECT ... INTO Syntax
The SELECT ... INTO form of SELECT enables a query result to be stored in variables or written to a file:
* **SELECT ... INTO _var_list_** selects column values and stores them into variables.
* **SELECT ... INTO OUTFILE** writes the selected rows to a file. Column and line terminators can be specified to produce a specific output format.
* **SELECT ... INTO DUMPFILE ** writes a single row to a file without any formatting.

The **SELECT** syntax description (see Section 13.2.9, "SELECT Syntax") shows the INTO clause near the end of the statement.It is also possible to use INTO immediately following the _select_expr_ list.

An **INTO** clause should not be used in a nested **SELECT** because such a **SELECT**  must return its result to the outer context.

The INTO clause can name a list of one or more variables, which can be user-defined variables, stored procedure or function parameters, or stored program local variables.(Within a prepared SELECT ... INTO OUTFILE statement, only user-defined variables are permitted; see Section 13.6.4.2, "Local Variable Scope and Resolution".)

The selected values are assigned to the variables. The number of variables must match the number of columns. The query should return a single row.If the query returns no rows, a warning with error code 1329 occurs(No data), and the variable values remain unchanged. If the query returns multiple rows, error 1172 occurs(Result consited of more than one row). If it is possible that the statement may retrieve multiple rows, you can use **LIMIT 1** to limit the result set to a single row.

User variable names are not case sensitive. See Section 9.4, "User-Defined Variables".

The **SELECT ... INTO OUTFILE '_file_name_'** form of **SELECT** writes the selected rows to a file. The file is created on the server host, so you must have the **FILE** privilege to use this syntax.**file\_name** cannot be an existing file, which among other things prevents files such as **/etc/passwd** and database tables from being destroyed. The **character\_set\_filesystem** system variable controls the interpretation of the file name.

The **SELECT ... INTO OUTFILE** statement is intended primarily to let you very quickly dump a table to a text file on the server  machine. If you want to create the resulting file on some other host than the server host, you normally cannot use **SELECT ... INTO OUTFILE** since there is no way to write a path to the file relative to the server host's file system.

However, if the MySQL client software is installed on remote machine, you can instead use a client command such as **mysql -e "SELECT ..." > file\_name** to generate the file on the client host.

It is also possible to create the resulting file on a different host other than the server host, if the location of the file on the remote host can be accessed using a network-mapped path on the server's file system. In this case, the presence of **mysql** (or some other MySQL client program) is not required on the target host.

**SELECT ... INTO OUTFILE** is the complement of **LOAD DATA INFILE**. Column values are written converted to the character set specified in the **CHARACTER SET** clause. If no such clause is present, values are dumped using the **binary** character set. In effect, there is no character set conversion. If a result set contains columns in several character sets, the output data file will as well and you may not be able to reload the file correctly.

The syntax for the **exprot\_options** part of the statement consists of the same **FILEDS** and **LINES** clauses that are used with the **LOAD DATA INFILE** statement. See Section 13.2.6, "LOAD DATA INFILE Syntax", for information about the **FIELDS** and **LINES** clauses, including their default values and permissible values.

**FIELDS ESCAPED BY** controls how to write special characters. If the **FIELDS ESCAPED BY** character is not empty, it is used when necessary to avoid ambiguity as prefix that precedes following characters on output:
* The FILEDS ESCAPED BY character
* The FIELDS [OPTIONALLY] ENCLOSED BY character
* The first character of the FIELDS TERMINATED BY and LINES TERMINATED BY values
* ASCII NUL(the zero-valued byte; what is actually written following the escape character is ASCII 0, not a zero-valued byte)

The FIELDS TERMINATED BY, ENCLOSED BY, ESCAPED BY, or LINES TERMINATED BY characters must be escaped so that you can read the file back is reliably.ASCII NUL is escaped to make it easier to view with some pagers.

The resulting file dose not have to conform to SQL syntax, so nothing else need be escaped.

If the FIELDS ESCAPED BY character is empty, no characters are escaped and NULL is output as NULL, not \N. it is probably not a good idea to specify an empty escape character, particularly if field values in you data contain any of the characters in the list just given.

Here is an example that produces a file in the comma-separated values(CSV) format used by many program:

```
SELECT a,b, a+b INTO OUTFILE '/tmp/result.txt'
    FIELDS TERMINATED BY ','  OPTIONALLY ENCLOSED BY '"'
    LINES TERMINATED BY '\n'
    FROM test_table;
```

