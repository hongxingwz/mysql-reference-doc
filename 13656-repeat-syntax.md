# 13.6.5.6 REPEAT Syntax

![](/assets/1504957118803.png)

The statement list within a **REPEAT **statement is repeated until the **search\_condition** expression is true. Thus, a REPEAT always enters the loop at least once. **statement\_list** consists of one or more statement, each terminated by a semicolon\(**;**\) statement delimiter.

A **REPEAT** statement can be labeled. For the rules regarding label use, see Section 13.6.2, "Statement Label Syntax".

Example:

```
CREATE PROCEDURE dorepeat2(p1 INT)
BEGIN
	SET @x = 0;
	REPEAT 
		SET @x = @x+ 1;
	UNTIL @x > p1 END REPEAT;
END
$$
Query OK, 0 rows affected (0.00 sec)

mysql> select @x;
    -> $$
+------+
| @x   |
+------+
|   11 |
+------+
1 row in set (0.00 sec)
```



