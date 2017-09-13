# 13.2.1 CALL Syntax

```
CALL sp_name([parameter[, ...]])
CALL sp_name[()]
```

The **CALL** statement invokes a stored procedure that was defined previously with **CREATE PROCEDURE**.

Stored procedures that take no arguments can be invoked without parentheses. That is, `CALL p()` and `CALL p` are equivalent.

`CALL` can pass back values to its caller using parameters that are declared as `OUT` or `INOUT` parameters. When the procedure returns, a client program can also obtain the number of rows affected for the final statement executed within the routine:At the SQL level, call the `ROW_COUNT()` function; from the C API, call the `mysql\_affected\_rows()` function.

