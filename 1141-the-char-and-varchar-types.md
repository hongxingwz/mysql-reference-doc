# The CHAR and VARCHAR Types

CHAR和VARCHAR类型是相似的，但他们存储和提取是不同的。他们在最大的长度和是否截取尾部的空格上也是不同的。

CHAR和VARCHAR类型被声明时具有一个长度表示最想存储的字符的最大数量。例如，CHAR\(30\)可以保存30个字符。

CHAR列在你创建表时其长度就固定了。长度可以是0-255的任何值。当CHAR值被存储时，他们会在右侧填充空格达到指的长度。当CHAR值被提取时，尾部的空格会被移除除非PAD\_CHAR\_TO\_FULL\_LENGTH SQL模式启用。

在VARCHAR列值是可变长度的字符串。长度可以是从0-65535之间的任何值。The effective maximum length of a VARCHAR is subject to the maximum row size(65535 bytes, which is shared among all columns) and the character set used. 参阅 Section C.10.4 "Limits on Table Column Count and Row Size"
与CHAR的差异是，VARCHAR的值通常被存储为1字节或2字节前缀加上数据。前缀的长度表明了值的byte的数量。如果value值需要不超过255字节列使用1字节来存储长度，如果值超过255字节则需要2字节来存储长度。
如果strict SQL模式没有被激活，你把一个值赋给一个CHAR或VARCHAR列超出了列的最大长度，value值会被截取来适用于这个列的最大长度并生成一个警告。如果不想截取多余的字符，你可以制造一个错误(而不是一个警告)并且压制插入值通过使用严格模式。参阅5.1.8章，"Server SQL Modes"



