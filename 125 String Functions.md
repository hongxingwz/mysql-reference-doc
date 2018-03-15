# 字符串函数(String Functions)

### CONCAT(str1, str2, ...)
以字符串的形式返回参数连接起来的结果。可以有一个或多个参数。如果所有的参数都是非二进制的字符串，返回的结果就是非二进制的字符串。如果参数包含任何的二进制字符串，返回的结果是一个二进制字符串。数字参数会被转换成与其等价的非二进制字符串形式。

**CONCAT()返回NULL如果任何参数是NULL**


```
mysql> SELECT CONCAT('My', 'S', 'QL');
  -> 'MySQL'
mysql> SELECT CONCAT('My', NULL, 'QL'); 
  -> NULL
mysql> SELECT CONCAT(14.3); 
  -> '14.3'
```
对于用引号的字符串，连接的操作可以被把字符串放在一起的操作替换


```
mysql> SELECT 'My' 'S' 'QL';
    -> 'MySQL'
```

### CONCAT_WS(separator, str1, str2, ...)

CONCAT_WS()代表用分割符把字符串串联起来，是CONCAT()的的一种特殊的形式。第一个参数是剩下参数的分割符。分割符被放在要串联字符串的中间。分割符可以是一个字符串，剩下的参数也可以是字符串。如果分割符是NULL，返回的结果也是NULL

```
mysql> SELECT CONCAT_WS(',','First name','Second name','Last Name'); 
     -> 'First name,Second name,Last Name'
mysql> SELECT CONCAT_WS(',','First name',NULL,'Last Name');
     -> 'First name,Last Name'
```
**CONCAT_WS()**不会跳过任何空字符串。然而，其会跳过任何NULL值参数





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




