# 13.6.2 Statement Label Syntax
![](/assets/1504876553547.png)

Labels are permitted for **BEGIN ... END** blocks and for the **LOOP**, **REPEAT**, and the **WHILE** statements.Label use for those statements follows these rules:
* **_begin\_label_** must be followed by a colon.
* **_begin\_label_** can be given without **end\_label.** if **end\_label** is present, it must be the same as **begin\_label**.
* **end\_label** cannot be given without **begin\_label**.
* Labels at the same nesting level must be distinct.
* Labels can be up to 16 characters long

To refer to a label within the labeled construct, use and **ITERATE** or **LEAVE** statement. The following example uses those statements to continue iterating or terminate the loop:


```
mysql> CREATE PROCEDURE doiterate(p1 INT)
    -> BEGIN
    ->   label1: LOOP
    ->     SET p1 = p1 + 1;
    ->     IF p1 < 10 THEN ITERATE label1; END IF;
    ->     LEAVE label1;
    ->   END LOOP label1;
    -> END$$
```
The scope of a block label does not include the code for handlers declared within the block. For details, see Section 13.6.7.2 "DECLARE ... HANDLER Syntax".


