# 13.6.5.8 WHILE Syntax

![](/assets/1504957948864.png)The statement list within a **WHILE **statement is repeated as long as the **_search\_condition_** expression is true. **_statement\_list_** consists of one or more SQL statements, each terminated by a semicolon(**;**) statement delimiter.

A **WHILE** statement can be labeled. For the rules regarding label use, see Section 13.6.2, "Statement Label Syntax".

Example:


```
CREATE PROCEDURE dowhile()
BEGIN
	DECLARE v1 int default 5;

	while v1 > 0 do
	# ...
	set v1  = v1 - 1;
	end while;
END$$
```





