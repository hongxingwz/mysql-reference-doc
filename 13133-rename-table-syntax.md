# 13.1.33 RENAME TABLE Syntax

```
RENAME TABLE
    tbl_name TO new_table_name
    [, tbl_name2 TO new_tbl_name2] ...
```

**RENAME TABLE** renames one or more tables. You must have **ALTER** and **DROP** privileges for the original table, and **CREATE** and **INSERT** privileges for the new table.

For example, to rename a table named **old\_table** to **new\_table**, use this statement:


```
RENAME TABLE old_table TO new_table;
```
That statement is equivalent to the following **ALTER TABLE** statement:


```
ALTER TABLE old_table RENAME new_table;
```
**RENAME TABLE**, unlike **ALTER TABLE**, can rename multiple tables within a single statement:


```
RENAME TABLE old_table1 TO new_table1,
             old_table2 TO new_table2,
             old_table3 TO new_table3,

```

Renaming operations are performed left to right. Thus, to swap two table names, do this(assuming that a table with the intermediary name tmp_table does not already exist):


```
RENAME TABLE old_table TO tmp_table,
            new_table TO old_table,
            tmp_table TO new_table;
```
When you execute RENAME TABLE, you cannot have any locked tables or active transactions. With that condition satisfied, the rename operation is done atomically; no other session can access any of the tables while the rename is in progress.

If any error occur during a **RENAME TABLE**, the statement fails and no changes are made.

You can use **RENAME TABLE** to move a table from one database to another:


```
RENAME TABLE current_db.tbl_name TO other_db.tbl_name;
```
Using this method to move all tables from one database to different one in effect renames the database(an operation for which MySQL has no single statement), except that the original database continues to exist, albeit with no tables.

Like **RENAME TABLE**, **ALTER TABLE ...** **RENAME** can also be used to move a table to a different database. Regardless of the statement used, if the rename operation would move the table to a database located on a different file system, the success of the outcome is platform specific and depends on the underlying operating system calls used to move the table files.

If a table has triggers, attempts to rename the table into a different database fail with a **Trigger in wrong schema** error.

**RENAME TABLE** does not work for **TEMPORARY** tables.  However, you can use **ALTER TABLE** to rename **TEMPORARY** tables.

**RENAME TABLE** works for views, except that views cannot be renamed into a different database.

Any privileges granted specifically for a renamed table or view are not migrated to the new name. They must be changed manually.









