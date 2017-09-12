# 13.7.4.1 SET Syntax for Variable Assignment

![](/assets/1505173205181.png)

**SET** syntax for variable assignment enables you to assign values to different types of variables that affect the operation of the server of clients:

* System variables. See Section 5.1.5, "Server System Variables". System variables also can be set at server startup, as described in Section 5.1.6, "Using System Variables".\(To _display_ system variable names and values, use the **SHOW VARIABLES** statement; Section 13.7.5.39, "SHOW VARIABLES Syntax"._ _\)
* User-defined variables. See Section 9.4, "User-Defined Variables".
* Stored procedure and function parameters, and stored program local variables. See Section 13.6.4, "Variables in Stored Programs"

A **SET** statement that assigns variable values is not written to the binary log, so in replication scenarios it affects only the host on which you execute it. To affect all replication hosts, execute the statement on each one.

The following examples illustrate **SET** syntax for setting variables. They use the **= **assignment operator, but the **:= **assignment operator is also permitted for this purpose.

A user variable is written as **@var\_name** and is assigned an expression value as follows:


```
SET @var_name = expr;
```

Examples:


```
SET @name = 43;
SET @total_tax = (SELECT SUM(tax) FROM taxable_transactions)
```

The **expr** can range from simple (a literal value) to more complex (the value returned by a scalar subquery).

**SET** applies to parameters and local variables in the context of the stored object within which they are defined. The following procedure uses the **counter** local variable as a loop counter:


```
CREATE PROCEDURE p()
BEGIN
    DECLARE counter INT DEFAULT 0;
    WHILE counter < 10 DO
    -- ... do work ...
    SET counter = counter + 1;
    END WHILE;
END;
```

Many system variables are dynamic and can be changed at runtime by using the **SET** statement. For a list, see Section 5.1.6.2, "Dynamic System Variables". To change a system variable with **SET**, refer to it by name, optionally preceded by a modifier:
* To indicate that a variable is a global variable, precede its name by the GLOBAL keyword or the **@@global.** qualifier:


```
SET GLOBAL max_connections = 1000;
SET @@global.max_connections = 1000;
```
The **SUPER** privilege is required to set global variables.

* To indicate that a variable is a session variable, precede its name by the **SESSION** keyword or either the **@@session.** or **@@** qualifier:


```
SET SESSION sql_mode = 'TRADITIONAL';
SET @@session.sql_mode = 'TRADITIONAL';
SET @@sql_mode = 'TRADITIONAL';
```
Setting a session variable normally requires on  special privilege, although there are exceptions that require the **SUPER** privilege(such as sql\_log\_bin). A client can change its own session variables, but not those of any other client.

* **LOCAL** and **@@local.** are synonyms for **SESSION** and **@@session.**

* If no modifier is present, **SET** changes the session variable.

* An error occurs under these circumstances:
* Use of **SET GLOBAL** or (**@@global.**) when setting a variable that has only a session value:


```
mysql> SET GLOBAL sql_log_bin = ON;
ERROR 1231 (42000): Variable 'sql_log_bin' can't be set to the value of 'ON'
```

* Omission of **GLOBAL**(or **@@global.**) when setting a variable that has only a global value:


```
mysql> set max_connections = 1000;
ERROR 1229 (HY000): Variable 'max_connections' is a GLOBAL variable and should be set with SET GLOBAL
```

* Use of **SET SESSION**(or @@SESSION.) when setting a variable that has only a global value:


```
mysql> SET SESSION max_connections = 1000;
ERROR 1229 (HY000): Variable 'max_connections' is a GLOBAL variable and should be set with SET GLOBAL
```

The preceding modifiers apply only to system variables. An error occurs for attempts to apply them to user-defined variables, stored procedure or function parameters, or stored program variables.

A **SET** statement can contain multiple variable assignments, separated by commas. This statement assigns values to a user-defined variable and a system variable:


```
SET @x = 1, SESSION sql_mode = '';
```
If you set multiple system variables, the most recent **GLOBAL** or **SESSION** modifier in the statement is used for following assignments that have no modifier specified.


```
SET GLOBAL sort_buffer_size = 1000000, SESSION sort_buffer_size = 1000000;
SET @@global.sort_buffer_size = 1000000, @@local.sort_buffer_size = 1000000;
SET GLOBAL max_connections = 1000, sort_buffer_size = 100000;
```
If any variable assignment in a** SET **statement fails, the entire statement fails and no variables are changed.

If you change a session system variable, the value remains in effect within your session until you change the variable to different value or the session ends. The change has no effect on other sessions.

If you change a global system variable, the value is remembered and used for new sessions until you change the variable to different value or the server exists. The change is visible to any client that accesses the global variable. However, the change affects the corresponding session variable only for clients that connect after the change. The global variable change dose not affect the session variable for any current client sessions(not even then session within which the SET GLOBAL statement occurred).

To make a global system variable setting permanent so that it applies across server restarts, you should also set it in an option file.

To set a **GLOBAL** value to the compiled-in MySQL default value or a **SESSION** variable to the current corresponding **GLOBAL** value, set the variable to the value **DEFAULT**. For example, the following two statements are identical in setting the session value of **max\_join\_size**


```
SET @@session.max_join_size=DEFAULT;
SET @@session.max_join_size=@@global.max_join_size;
```

Not all system variables can be set to DEFAULT. In such cases, assigning **DEFAULT** results in an error.

An error occurs for attempts to assign **DEFAULT** to user-defined variables, stored procedure of function parameters, or stored program local variables.

To refer to the value of a system variable in expressions, use one of the @@-modifiers. For example, you can retrieve values in a **SELECT** statement like this:


```
SELECT @@global.sql_mode, @@session.sql_mode, @@sql_mode;
```

For a reference to a system variable in an expression as **@@var_name**(rather than with **@@global.** or **@@session.**), MySQL return the session value if it exists and the global value otherwise. This differs from` SET @@value_name`, which always refers to the session value.
























