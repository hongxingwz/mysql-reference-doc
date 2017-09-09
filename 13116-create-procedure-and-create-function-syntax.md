# CREATE PROCEDURE and CREATE FUNCTION Syntax

![](/assets/1504923712788.png)![](/assets/1504923761798.png)
These statements create stored routines. By default, a routine is associated with the default database. To associate the routine explicitly with a given database, specify the name as db\_name.sp\_name when you create it.

The **CREATE FUNCTION** statement is also used in MySQL to support UDFs(user-defined functions). See Section 28.4, "Adding New Functions to MySQL". A UDF can be regarded as an external stored function. Stored functions share their namespace with UDFs. See Section 9.2.4, "Function Name Parsing and Resolution", for the rules describing how the server interprets references to different kinds of functions.

To invoke a stored procedure, use the **CALL**  statement(see Section 13.2.1, "CALL Syntax"). To invoke a stored function, refer to it in an expression. The function returns a value during expression evaluation.

**CREATE PROCEDURE** and **CREATE FUNCTION** require the **CREATE ROUTINE** privilege. They might also require the **SUPER** privilege, depending on the **DEFINER** value, as described later in this section. If binary logging is enabled, **CREATE FUNCTION** might require the **SUPER** privilege, as described in Section 23.7, "Binary Logging of Stored Programs".

By default, MySQL automatically grants the **ALTER ROUTINE** and **EXECUTE** privileges to the routine creator. This behavior can be changed by disabling the auto\_sp\_privileges system variable. See Section 23.2.2, "Stored Routines and MySQL Privileges".

The **DEFINER** and **SQL SECURITY** clasues specify the security context to be used when checking access privileges at routine execution time, as described later in this section.

If the routine name is the same as the name of a built-in SQL function, a syntax error occurs unless you use a space between the name and the following parenthesis when defining the routine or invoking it later. For this reason, avoid using the names of existing SQL functions for your own stored routines.

The **IGNORE\_SPACE** SQL mode applies to built-in functions, not to stored routines. It is always permissible to have spaces after a stored routine name, regardless of whether **IGNORE\_SPACE** is enabled.

The parameter list enclosed within parentheses must always be present. If there are no parameters, an empty parameter list of () should be used. Parameter names are not case sensitive.

Each parameter is an **IN** parameter by default. To specify otherwise for a parameter, use the keyword **OUT** or **INOUT** before the parameter name.

>Note
>
>Specifying a parameter as **IN**, **OUT**, or **INOUT** is valid only for a **PROCEDURE**. For a **FUNCTION**, parameters are always regarded as **IN** parameters.

An **IN** parameter passes a value into a procedure. The procedure might modify the value, but the modification is not visible to the caller when the procedure returns. An **OUT** parameter passes a value from the procedure back to the caller. Its initial value is **NULL**  within the procedure, and its value is visible to the caller when the procedure returns. An **INOUT** parameter is initialized by the caller, can be modified by the procedure, and any change made by the procedure is visible to the caller when the procedure returns.

For each **OUT** or **INOUT** parameter, pas a user-defined variable in the **CALL** statement that invokes the procedure so that you can obtain its value when the procedure returns. If you are calling the procedure from within another stored procedure of function, you can also pass a routine parameter or local routine variable as an **IN** or **INOUT** parameter.

Routine parameters cannot be referenced in statements prepared within the routine; See Section C.1, "Restrictions on Stored Programs". 

The following example shows a simple stored procedure that uses an **OUT** parameter:

```
mysql> delimiter //

mysql> CREATE PROCEDURE simpleproc (OUT param1 INT)
    -> BEGIN
    ->   SELECT count(*) into param1 from test0907.events_list;
    -> end //
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;

mysql> call simpleproc(@a);
Query OK, 1 row affected (0.00 sec)

mysql> select @a;
+-------+
| @a    |
+-------+
| 10661 |
+-------+
```

The example uses the **mysql** client **delimiter** command to change the statement delimiter from** ;** to **//** while the procedure is being defined. This enables the ; delimiter used in the procedure body to be passed through the server rather than being interpreted by **mysql **itself. See Section 23.1, "Defining Stored Programs".

The **RETURNS** clause may be specified only for a **FUNCTION**, for which it is mandatory. It indicates the return type of the function, and the function body must contain a **RETURN** **_value_** statement. If the **RETURN** statement returns a value of a different type, the value is coerced to the proper type. For example, if a function specifies an ENUM or SET value in the **RETURNS** clause, but the **RETURN** statement returns an integer, the value returned from the function is the string for the corresponding ENUM member of set of SET member.

The following example function takes a parameter, performs an operation using an SQL function, and return the result. In this case, it is unnecessary to use delimiter because the function definition contains no  internal **;** statement delimiters:


```
mysql> CREATE FUCNTION hello (s CHAR(20))
    -> RETURNS CHAR(50) DETERMINISTIC
    -> RETURN CONCAT ('Hello, ' , s, '!' );
    
mysql> SELECT hello('world');

+----------------+
| hello('world') |
+----------------+
| Hello, world!  |
+----------------+
1 row in set (0.01 sec)
```
Parameter types and function types can be declared use any valid data type. The **COLLATE** attribute can be used if preceded by the **CHARACTER SET** attribute. The **_routine\_body_** consists of a valid SQL routine statement. This can be a simple statement such as **SELECT** or **INSERT**, or a compound statement written using **BEGIN** and **END**. Compound statements can contain declarations, loops, and other control structure statements. The syntax for these statements is described in Section 13.6, "Compound-Statement Syntax".

MySQL permits routines to contain DDL statements, such as **CREATE** and **DROP**. MySQL also permits stored  procedures(but not stored functions) to contains SQL transaction statements such as **COMMIT**. Stored functions may not contain statements that perform explicit or implicit commit or rollback. Support for these statements is not required by the SQL standard, which states that each DBMS vendor may decide whether to permit them.

Statements that return a result set can be used within a stored procedure but not within a stored function. This prohibition includes **SELECT** statements that do not have an **INTO _var_list_** clause and other statements such as **SHOW, EXPLAIN**, and** CHECK TABLE**. For statements that can be determined at function definition time to return a result set, a **Not allowed to return a result set from a function** error occurs(ER\_SP\_NO\_RETSET). For statements that can be determined only at runtime to return a result set, a **PROCEDURE %s can't return a result set in the given context** error occurs(ER\_SP\_BADSELECT).

**USE** statements within stored routines are not permitted. When a routine is invoked, an implicit USE **db\_name** is performed(and undone when the routine terminates). The causes the routine to have the given default database while it executes. References to objects in databases other than the  routine default database should be qualified with the appropriate database name.

For additional information about statements that are not permitted in stored routines, see Section C.1, "Restrictions on Stored Programs".

For information about invoking stored procedures from within programs written in a language that has a MySQL interface, see Section 13.2.1, "CALL Syntax".




