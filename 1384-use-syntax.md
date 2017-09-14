# 13.8.4 USE Syntax

![](/assets/1505376160726.png)
The **USE _db\_name_** statement tell MySQL to use the _**db\_name**_ database as the default(current) database for subsequent statements. The database remains the default until the end of the session or another **USE** statement is issued:


```
USE db1;
SELECT COUNT(*) FROM mytable; # selects from db1.mytable
USE db2;
SELECT COUNT(*) FROM mytable; # selects from db2.mytable
```
Making a particular database the default by means of the **USE** statement does not preclude you from accessing tables in other databases. The following example accesses the **author** table from the **db1** database and the **editor** table from the **db2** database:


```
USE db1;
SELECT author\_name, editor\_name FROM author, db2.editor 
    WHERE author.editor\_id = db2.editor.editor\_id;
```




