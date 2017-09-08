# 1.8.2 MySQL Differences from Standard SQL
We try to make MySQL Server follow the ANSI SQL standard and the ODBC SQL standard, but MySQL Serve performs operations differently in some cases:
* There are several differences between the MySQL and standard SQL privilege systems. For example, in MySQL, privileges for a table are not automatically revoked when you delete a table. You must explicitly issue a **REVOKE** statement to revoke privileges for a table. For more information, see Section 13.7.1.6, "REVOKE Syntax".

* The CAST() function does not support cast to REAL or BIGINT. See Section 12.10, "Cast Functions and Operators".