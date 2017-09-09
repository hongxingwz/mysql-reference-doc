# 13.6.6 Cursors
MySQL supports cursors inside stored programs. The syntax is as in embedded SQL. Cursors have these properties:
* Asensitive:The server may or may not make a copy of its result table
* Read only: Not updatable
* Nonscrollable: Can be trraversed only in one direction and cannot skip rows

Cursor declarations must appear before handler declarations and after variable and condition declarations.
Example:


```
CREATE PROCEDURE curdemo()
BEGIN
	DECLARE done INT DEFAULT FALSE;
	DECLARE a CHAR(16);
	DECLARE b, c INT;
	DECLARE cur1 CURSOR FOR SELECT event_name, event_started FROM test0907.events_list;
	DECLARE cur2 CURSOR FOR SELECT event_name FROM test0907.events_list;
	DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;

	OPEN cur1;
	OPEN cur2;

	read_loop: LOOP
		# FETCH cur1 INTO a, b;
		# FETCH cur2 INTO c;
		IF done THEN
			LEAVE read_loop;
		ELSE 
			INSERT INTO test0907.events_log (name) values ("a");
		END IF;
	END LOOP;

	CLOSE cur1;
	CLOSE cur2;
END$$
```


