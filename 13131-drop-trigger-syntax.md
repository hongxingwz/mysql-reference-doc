# 13.1.31 DROP TRIGGER Syntax

![](/assets/1504968350176.png)This statement drops a trigger. The schema\(database\) name is optional. If the schema is omitted, the trigger is dropped from the default schema. **DROP TRIGGER **requires the **TRIGGER** privilege for the table associated with the trigger.

Use** IF EXISTS **to prevent an error from occurring for a trigger that does not exist. A **NOTE** is generated for a nonexistent trigger when using **IF EXISTS**. See Section 13.7.5.40, "SHOW WARNINGS Syntax"

Triggers for a table are also dropped if you drop the table.

