



###STRCMP(expr1, expr2)
STRCMP() returns 0 if the strings are the same, -1 if the first argument is smaller than the second according to the current sort order, and 1 otherwise.



```
mysql> SELECT STRCMP('text', 'text2');
 -> -1
mysql> SELECT STRCMP('text2', 'text');
 -> 1
mysql> SELECT STRCMP('text', 'text');
 -> 0
```

STRCMP() performs the comparison using the collation of the arguments.



```
mysql> SET @s1 = _latin1 'x' COLLATE latin1_general_ci;
mysql> SET @s2 = _latin1 'X' COLLATE latin1_general_ci;
mysql> SET @s3 = _latin1 'x' COLLATE latin1_general_cs;
mysql> SET @s4 = _latin1 'X' COLLATE latin1_general_cs;
mysql> SELECT STRCMP(@s1, @s2), STRCMP(@s3, @s4);
+------------------+------------------+
| STRCMP(@s1, @s2) | STRCMP(@s3, @s4) |
+------------------+------------------+
|                0 |                1 |
+------------------+------------------+
```

If the collations are incompatible, one of the arguments must be converted to be compatible with the other. See Section 10.1.8.4, "Collation Coercibility in Expressions."



```
mysql> select strcmp(@s1, @s3);
ERROR 1267 (HY000): Illegal mix of collations (latin1_general_ci,IMPLICIT) and (latin1_general_cs,IMPLICIT) for operation 'strcmp'

mysql> select strcmp(@s1, @s3 collate latin1_general_ci);
+--------------------------------------------+
| strcmp(@s1, @s3 collate latin1_general_ci) |
+--------------------------------------------+
|                                          0 |
+--------------------------------------------+
```




