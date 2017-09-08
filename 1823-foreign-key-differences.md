# 1.8.2.3 Foreign Key Differences
The MySQL implementation of foreign keys differs from the SQL standard in the following key respects:
* If there are several rows in the parent table that have the same referenced key value, InnoDB acts in foreign key checks as if the other parent rows with the same key value do not exist.For example, if you have defined a **RESTRICT** type constraint, and there is a child row with several parent rows,  InooDB does not permit the deletion of any of those parent rows.

  InnoDB performs cascading operations through a depth-first algorithm, based on records in the indexes corresponding to the foreign key constraints.

* A **FOREIGN KEY** constraint that references a non-**UNIQUE** key is not standard SQL but rather an **InnoDB** extension.

