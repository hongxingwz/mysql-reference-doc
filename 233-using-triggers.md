# 23.3 Using Triggers

A trigger is a named database object that is associated with a table, and that activates when a particular event occurs for the table. Some uses for triggers are to perform checks of values to be inserted into a table or to perform calculations on values involved in an update.

A trigger is defined to activate when a statement inserts, updates, or deletes rows in the associated table. These row operations are trigger events. For example. rows can be inserted by **INSERT** or **LOAD DATA** statements, and an insert trigger activates for each inserted row.A trigger can be set to activate either before or after the trigger event.For example, you can have a trigger activate before each row that is inserted into a table or after each row that is updated.

> Important
>
>MySQL triggers activate only for changes made to tables by SQL statements. This includes changes to base tables that underlie updatable views. Triggers do not activate for changes to tables made by APIs that do not transmit SQL statements to the MySQL Server. This means that triggers are not activated by updates made using the **NDB** API.
Triggers are not activated by changes in **INFORMATION\_SCHEMA** or **performance\_schema** tables. Those tables are actually views and triggers are not permitted on views.

The following sections describe the syntax for creating and dropping triggers, show some examples of how to use them, and indicate how to obtain trigger metadata.

## Additional Resources
* You may find the **Triggers User Forum** of use when working with triggers.
* For answers to commonly asked questions regarding triggers in MySQL, see Section A.5, "MySQL 5.7 FAQ: Triggers".
* There are some restriction on the use of triggers; see Section C.1, "Restrictions on Stored Programs".
* Binary logging for triggers takes place as described in Section 23.7 , "Binary Logging of Stored Programs".




