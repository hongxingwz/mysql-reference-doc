# 11.2.5 Numeric Type Attributes

MySQL支持可选的拓展在关键字后面接括号数字的形式指定显示的宽度。例如，INT\(4\)指定了INT类型显示的宽度为4。这个可选的宽度在应用在显示数值时少于指定的宽度以左则填充空格。\(That is, this width is present in the metadata returned with result sets. Whether it is used or not is up to the application\)。

展示的宽度不会限制可以在该列中存储值的范围，当存储的值大于显示的宽度时也会正确的显示。例如，指定为SMALLINT\(3\)的列的仓储范围是SMALLINT的-32768 到 32767。当值超过三个数字时会显示全部的数字而不是3个。

当与可选的\(非标准\)的属性ZEROFILL使用时，默认填充的空格会用0替换。例如，一个声明为INT\(4\) ZEROFILL的列，5提取时会是0005

> 注
>
> 当一个列在表达式中调用或使用UNION查询时ZEROFILL属性会被忽略。
>
> 如果你往一个ZEROFILL的列中存储的值的长度大于指定的显示宽度时，你可能会遇到一些问题当MySQL从一些复杂的连接生成临时表时，MySQL假定数据值会以显示的宽度填充。

所有的整形类型可以拥有可选的\(非标准\)属性UNSIGNED.无符号类型可以用来允许非负数值在一列中存储或当你需要一个更大的数值范围在列中存储。例如，如果一个INT列是UNSIGNED，列的存储范围是固定的，但其端点由-2147483648~2147483647移动到0~4294967295。

浮点和定点类型也可以是UNSIGEND，像整型类型一样，此属性阻止了负值向该列存储。不像整型，列的存储范围保持不变。

如果你为数值列指定了ZEROFILL，MySQL自动为该列添加UNSIGNED属性。

整型或浮点数值类型可以拥有额外的AUTO\_INCREMENT属性，当你往索引列并且拥有AUTO\_INCREMENT属性插入一个NULL值时，NULL值会被该列的下一个值替换。通常这个值是value + 1，value是当前表中该列的最大值\(AUTO\_INCREMENT列以1开始）

往AUTO\_INCREMENT插入0值与NULL具有同样的效果，除非NO\_AUTO\_VALUE\_ON\_ZERO SQL 模式启动

插入NULL值来生成AUTO\_INCREMENT需要此列被声明为NOT NULL，如果此被被声明为NULL，插入NULL就会存储为NULL。当你往AUTO\_INCREMENT列插入其它值时，该列会被设置为该值并且sequence会被重置，重置为当前列的最大值

