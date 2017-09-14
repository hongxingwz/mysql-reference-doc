# 13.5.1 PREPARE Syntax

![](/assets/1505373423268.png)The **PREPARE** statement prepares a SQL statement and assigns it a name, **stmt_name**, by which to refer to the statement later. The prepared statement is executed with **EXECUTE** and released with **DEALLOCATE PREPARE**. For examples, see [Section 13.5, "Prepared SQL Statement Syntax"](/135-prepared-sql-statement-syntax.md)

Statement names are not case sensitive. **preparable\_stmt** is either a string literal or a user variable that contains the text of the SQL statement. The next must represent a single statement, not multiple statements. Within the statement, ? characters can be used as parameter markers to indicate where data values are to be bound to the query later when you execute it. The ? characters should not be enclosed within quotation marks, even if you intend to bind them to string values. Parameter markers can be used only where data values should appear, not for SQL keywords, identifiers, and so forth.

If a prepared statement with the given name already exists, it is deallocated implicitly before the statement is prepared. This means that if the new statement contains an error and cannot be prepared, an error is returned and no statement with the given name exists.

The scope of a prepared statement is the session within which it is created, which as several implications:
* A prepared statement created in one session is not available to other sessions. 
* When a session ends, whether normally or abnormally, its prepared statements no longer exist.If auto reconnect is enabled, the client is not notified that connection was lost. For this reason, clients may wish to disable auto-reconnect. See Section 27.8.20, "C API Automatic Reconnection Control".
* A prepared statement created within a stored program  continues to exist after the program finishes executing and can be executed outside the program later.
* A statement prepared in stored program context cannot refer to stored procedure or function parameters or local variables because they go out of scope when the program ends and would be unavailable were the statement to be executed later outside the program. As a workaround, refer instead to user-defined variables, which also have session scope; see Section 9.4 "User-Defined Variables".

