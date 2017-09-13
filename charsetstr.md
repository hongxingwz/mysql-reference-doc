# CHARSET(str)
Returns the character set of the string argument.


```
mysql> SELECT CHARSET('abc');
+----------------+
| CHARSET('abc') |
+----------------+
| utf8           |
+----------------+

mysql> SELECT CHARSET(CONVERT('abc' USING latin1));
+--------------------------------------+
| CHARSET(CONVERT('abc' USING latin1)) |
+--------------------------------------+
| latin1                               |
+--------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT CHARSET(USER());
+-----------------+
| CHARSET(USER()) |
+-----------------+
| utf8            |
+-----------------+
1 row in set (0.00 sec)
```

