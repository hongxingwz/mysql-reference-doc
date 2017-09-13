# COLLATION(str)
Returns the collation of the string argument.


```
mysql> SELECT COLLATION('abc');
+------------------+
| COLLATION('abc') |
+------------------+
| utf8_general_ci  |
+------------------+
1 row in set (0.00 sec)

mysql> SELECT COLLATION(_utf8'abc');
+-----------------------+
| COLLATION(_utf8'abc') |
+-----------------------+
| utf8_general_ci       |
+-----------------------+
1 row in set (0.00 sec)


```

