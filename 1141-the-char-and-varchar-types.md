# The CHAR and VARCHAR Types

CHAR和VARCHAR类型是相似的，但他们存储和提取是不同的。他们在最大的长度和是否截取尾部的空格上也是不同的。

CHAR和VARCHAR类型被声明时具有一个长度表示最想存储的字符的最大数量。例如，CHAR\(30\)可以保存30个字符。

CHAR列在你创建表时其长度就固定了。长度可以是0-255的任何值。当CHAR值被存储时，他们会在右侧填充空格达到指的长度。当CHAR值被提取时，尾部的空格会被移除除非PAD\_CHAR\_TO\_FULL\_LENGTH SQL模式启用。

在VARCHAR列值是可变长度的字符串。长度可以是从0-65535之间的任何值。The effective maximum length of a VARCHAR is subject to the maximum row size\(65535 bytes, which is shared among all columns\) and the character set used. 参阅 Section C.10.4 "Limits on Table Column Count and Row Size"  
与CHAR的差异是，VARCHAR的值通常被存储为1字节或2字节前缀加上数据。前缀的长度表明了值的byte的数量。如果value值需要不超过255字节列使用1字节来存储长度，如果值超过255字节则需要2字节来存储长度。  
如果strict SQL模式没有被激活，你把一个值赋给一个CHAR或VARCHAR列超出了列的最大长度，value值会被截取来适用于这个列的最大长度并生成一个警告。如果不想截取多余的字符，你可以制造一个错误\(而不是一个警告\)并且压制插入值通过使用严格模式。参阅5.1.8章，"Server SQL Modes"。  
对于VARCHAR列，超出列长度的尾随空格在插入之前被截断，并生成警告，在使用时会忽略SQL模式。对于CHAR列，对于char列，不管SQL模式如何，从插入值截断多余的尾随空格都是静默执行的。

VARCHAR值不会被填充当他们被存储时。尾部的空格会被保留当他们存储和提取时，以符全标准的SQL。

下面的表格举列说明了CHAR和VARCHAR之间的不同通过展示存储不同的变量到CHAR\(4\)和VARCHAR\(4\)列的结果\(假设列使用单一的字节如果设置成latin1\)。

| Value | CHAR\(4\) | Storage Required | VARCHAR\(4\) | Storage Required |
| :--- | :--- | :--- | :--- | :--- |
| '' | '    ' | 4 bytes | '' | 1 byte |
| 'ab' | 'ab  ' | 4 bytes | 'ab' | 3bytes |
| 'abcd' | 'abcd' | 4 bytes | 'abcd' | 5 bytes |
| 'abcdefgh' | 'abcd' | 4 bytes | 'abcd' | 5 bytes |

表格最后一行展示的存储值仅当没有使用strict模式时才适用；如果MySQL运行在严格模式下，超出了列长度的值不会被存储，并返回一个错误。

InnoDB encodes fiexed-length fields greater than or equal to 768 bytes in length as variable-length fields, which cat be stored off-page。例 如，一个CHAR\(255\)列可以超过769字节如果最小的字符的字节长度设置超过了3字节，像是utf8mb4字符集。

如果指定的值被存储进CHAR\(4\)或VARCHAR\(4\)列，从列中提取的值并不总是相同因为尾部的空格字符会从CHAR列移除在提取时，下面的举列说明了这一点不同：

```
create table vc (v varchar(4), c char(4));
insert into vc values ('ab  ', 'ab  ');
select concat('(', v, ')'), concat('(', c, ')') FROM vc;
+---------------------+---------------------+
| concat('(', v, ')') | concat('(', c, ')') |
+---------------------+---------------------+
| (ab  )              | (ab)                |
+---------------------+---------------------+
```

在CHAR和VARCHAR列的值被存储和比较根据分配给列的字符集排序规则。

MySQL所有的核对集是PAD SPACE类型的。这表示所有的CHAR, VARCHAR, 和 TEXT值在比较时不会考虑任何尾部的空格。“Comparison” 在这种情况下不会包括LIKE pattern-matching 操作符，尾部的空格是非常生要的。例如：

```
mysql> create table names (myname CHAR(10));
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO names values ('Monty');
Query OK, 1 row affected (0.00 sec)

mysql> select myname='Monty', myname="Monty   " from names;
+----------------+-------------------+
| myname='Monty' | myname="Monty   " |
+----------------+-------------------+
|              1 |                 1 |
+----------------+-------------------+

mysql> SELECT myname like 'Monty', myname like 'Monty   ' from names;
+---------------------+------------------------+
| myname like 'Monty' | myname like 'Monty   ' |
+---------------------+------------------------+
|                   1 |                      0 |
+---------------------+------------------------+
```

这对于MySQL所有的版本都是有效的，并且不受服务器SQL模式的影响。

> 注：
>
> 关于更多的MySQL字符集和较验集的信息，参阅10.1章“Character Set Support”。对于存储要求额外的信息，参阅11.8章，“Data Type Storage Requirements”

因为这种原因尾部填充的字符被截取或比较时没有被忽略，如果一列有一个索引并需要唯一的值，在该列插入值仅在尾部填充字符的数量不是会造成duplicate-key错误。例如，如果一个表包含“a”,尝试插入一个"a   "将会造成duplicate-key 错误



