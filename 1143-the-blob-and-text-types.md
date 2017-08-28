# 11.4.3 The BLOB and TEXT Types
一个BLOB是一个二进制大对象可以容纳可变数量的数据。四种BLOB类型是TINYBLOB, BLOB, MEDIUMBLOB, 和LONGBLOB。他们的唯一区另是他们可以容纳的最大值。四种TEXT类型是TINYTEXT, TEXT, MEDIUMTEXT和LONGTEXT。他们与相关的四种BLOB类型具有相同的最大长度和存储要求。参阅11.8，“Data Type Storage Requirements”

BLOB值被看作为binary Strings(byte strings)。他们具有binary字符集和校验集，比较和排序基于列值的二进值的数量。TEXT值被看作非二进制字符(character strings)。他们具有字符集而不是二进制的，值的排序和比较基于collation of the character set.

如果严格SQL模式没有启用你赋值给BLOB或TEXT列超出了列的最大长度，值会被截取并生成一个警告。如果不想截取多除的字符，你可以通过启用严格模式来制造一个错误(而不是生成一个警告)并抑制值的插入。参阅5.1.8节"Servet SQL Modes"

截取超出长度的尾部空格插入进TEXT列永远生成一个警告，忽略SQL模式。

对于TEXT和BLOB列，在插入是不会填充在选择时不会被截取。

对于一个TEXT列作为索引，索引entry比较时在末尾被填充格空。这表示，如果索引需要唯一的值，duplicate-key错误将会发生如果值仅在尾部的空格的数量不同。例如，如果一个表包含'a', 尝试存储一个"a "将会造成duplicate-key错误，这对于BLOB列来说不适用。

在大多数情况，你可以把BLOB列作为VARBINARY列一样但可以存储的范围是很大的，你可以视TEXT列为VARCHAR列。BLOB和TEXT在以下几点不同与VARBINARY和VARCHAR列
* 对于BLOB和TEXT列的索引，你一定要指定索引的前缀长度。对于CHAR和VARCHAR，前缀长度是可选的。参阅8.3.4章，"Column Indexes"
* BLOB和TEXT列不能具有DEFAULT值

如果你使用BINARY属性和一个TEXT数据类型时，该列会被赋予binary(_bin)检验集of the column character set。

LONG和LONG VARCHAR映射到MEDIUMTEXT数据类型。这是一个相兼容的特性。

未完待续......
