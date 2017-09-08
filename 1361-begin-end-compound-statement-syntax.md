# 13.6.1 BEGIN ... END Compound-Statement Syntax

![](/assets/1504875474237.png)

**BEGIN ... END **syntax is used for writing compound statements, which can appear within stored programs\(stored procedures and functions, trigger, and events\). A compound statement can contain multiple statements, enclosed by the **BEGIN** and **END **keywords.  **_statement\_list_** represents a list of one or more statements, each terminated by a semicolon(**;**) statement delimiter. The **_statement_list_** itself is optional, so the empty compound statement(**BEGIN END**) is legal.
**
BEGIN ... END blocks can be nested.**

Use of multiple statements requires that a client is able to send statement strings contains the **;** statement delimiter. In the **mysql** command-line client, this is handled with the **delimiter** command.
Changing the **;** end-of-statement delimiter(for example, to //) permit **;** to be used in a program body. For an example, see Section 23.1, "Defining Stored Programs".

A **BEGIN ... END** block can be labeled. See Section 13.6.2, "Statement Label Syntax".

The optional **[NOT] ATOMIC** clause is not supported. This means that no transactional savepoint is set at the start of the instruction block and the **BEGIN** clause used in this context has no effect on the current transaction.

>NOTE
>
> Within all stored programs, the parser treats **BEGIN [WORK]** as the beginning of a **BEGIN ... END** block. To begin a transaction in this context, use **START  TRANSACTION** instead.

