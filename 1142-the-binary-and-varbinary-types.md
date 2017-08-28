# 11.4.2 BINARY and VARBINARY Types

BINARY和VARBINARY类型与CHAR和VARCHAR类型相似，除了他们包含二进制的字符串而不是非二进制的字符串。也就是他们包含的是二进制的字符串而不是字符型的字符串。这代表着他们有binary字符集和较难集，比较和排序是基于字符的二进制数值值。

BINARY和VARBINARY的最大长度与CHAR和VARCHAR一样除了BINARY和VARBINARY的长度是以字符而不是字符。

BINARY和VARBINARY数据类型与CHAR BINARY 和VARCHAR BINARY数据类型是有区别的。对于后面的类型，BINARY属性不会使列被看成二进制字符列。相反，他会造成binary\(\_bin\)collation用于column character set to be used,and the column itself contains nonbinary character strings rather than binary byte strings.例如,CHAR\(5\) BINARY被看成CHAR\(5\) CHARACTER SET latin1 COLLATE latin1\_bin,假设默认的字符集是latin1。与BINARY(5)的不同时，他存储5字节二进制字符并且具有binary字符集和较验集。获取binary strings 和 binary collations 对于非二进制的字符，参阅10.1.8.5 "The binary Collation Compared to _bin Collations".

如果严格的SQL模式没有被激活，并且你把一个值值赋给BINARY或VARBINARY列超出了列的最大长度，值会被截取来适合列的长度并且生成一个警告。对于截取的情况，你可以造成一个错误(而不是生成一个警告)并抑制值的插入通过使用严格的SQL模式，参阅5.1.8节"Servet SQL Modes"

当BINARY值没有被存储时，他们的右侧被填充值来达到指定的长度。填充的值是0x00(the zero byte)。值在插入时在右侧填充0x00，在选择时尾部的字节不会被移除。所有的字节在比较时都是有效的，包括ORDER BY 和 DISTINCT操作。0x00字节与空格的不同是在比较时0x00< space。
例如：对于BINARY(3)列，‘a  ’变成'a \0'当插入时。‘a\0’变成‘a\0\0’当被插入时。当被选择时所有插入的值保留不变。

对于VARBINARY，在插入时没有填充在选择时没有字节被截取。所有的字节在比较时都有效，包括ORDER BY和DISTINCT操作。在比较时0x00字节与空格是不同的，0x00 < space。

对于这些情况，当尾部的填充字节被截取或比较时被忽略，如果一列有一个索引需要不同的值，插入这列的值仅在尾部的字节的数量不同会千成duplicate-key错误。例如，如果一个表包含'a'，尝试插入'a\0'将会造成duplicate-key错误。

你应该考虑小心的处理前置的填充和截取字符如果你打算使用BINARY数据类型来存储二进制数据并且你需要值在提取时与存储时一模一样。下面的例子说明了BINARY值0x00填充怎样影响值的比较：


```java
mysql> CREATE TABLE t (c BINARY(3));
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO t set c='a';
Query OK, 1 row affected (0.01 sec)

mysql> select HEX(c), c='a', c='a\0\0' from t;
+--------+-------+-----------+
| HEX(c) | c='a' | c='a\0\0' |
+--------+-------+-----------+
| 610000 |     0 |         1 |
+--------+-------+-----------+
1 row in set (0.00 sec)
```
如果值提取时与值存储时要一样具不填充，使用VARBINARY或BLOB数据类型的一个作为代替比较合适





