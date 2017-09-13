# Information Functions

**ROW\_COUNT()**
**ROW_COUNT()** returns a value as follows:
* DDL statements: 0. This applies to statements such as **CREATE TABLE** or **DROP TABLE**.
* DML statements other than **SELECT**:The number of affected rows. This applies to statements such as **UPDATE**, **INSERT**, Or **DELETE**(as before), but now also to statements such as **ALTER TABLE** and **LOAD DATA INFILE.**
* **SELECT**: -1 if the statement return a result set, or the number of rows "affected" if it does not. For example, for `SELECT * FROM t1, ROW_COUNT()` returns -1. For `SELECT * FROM t1 INTO OUTFILE 'file_name'`, **ROW\_COUNT()** returns the number of rows written to the file.
* **SIGNAL** statements: 0

For **UPDATE** statements, the affected-rows value by default is the number of rows actually changed.If you specify the **CLIENT\_FOUND\_ROWS** flag to **mysql\_real\_connect()** when connection to **mysqld**, the affected-rows value is the number of rows "found"; that is, matched by the **WHERE** clause.

For **REPLACE** statements, the affected-rows value is 2 if the new row replaced an old row, because in this case, one row was inserted after the duplicate was delete.

For`
