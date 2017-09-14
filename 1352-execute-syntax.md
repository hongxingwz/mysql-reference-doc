# 13.5.2 EXECUTE Syntax

![](/assets/1505374610873.png)After preparing a statement with **PREPARE**, you execute it with an **EXECUTE** statement that refers to the prepared statement name.If the prepared statement contains any parameter markers, you must supply a **USING** clause that lists user variables containing the values to be bound to the parameters. Parameter values can be supplied only by user variables, and the **USING** clause must name exactly as many variables as the number of parameter markers in the statement.

You can execute a given prepared statement multiple times,  passing different variables to it or setting the variables to different values before each execution.

For examples, see [Section 13.5, "Prepared SQL Statement Syntax"](/135-prepared-sql-statement-syntax.md).

