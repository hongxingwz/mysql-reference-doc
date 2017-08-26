# 11.2.5 Numeric Type Attributes

MySQL支持可选的拓展在关键字后面接括号数字的形式指定显示的宽度。例如，INT\(4\)指定了INT类型显示的宽度为4。这个可选的宽度在应用在显示数值时少于指定的宽度以左则填充空格。\(That is, this width is present in the metadata returned with result sets. Whether it is used or not is up to the application\)。

展示的宽度不会限制可以在该列中存储值的范围，当存储的值大于显示的宽度时也会正确的显示。例如，指定为SMALLINT\(3\)的列的仓储范围是SMALLINT的-32768 到 32767。当值超过三个数字时会显示全部的数字而不是3个。

当与可选的\(非标准\)的属性ZEROFILL使用时，默认填充的空格会用0替换。例如，一个声明为INT\(4\) ZEROFILL的列，5提取时会是0005

> 注
>
> 当一个列在表达式中调用或使用UNION查询时ZEROFILL属性会被忽略



