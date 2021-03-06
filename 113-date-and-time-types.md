# 11.3 Date and Time Types

代表时间值的日期和时间类型是DATE, TIME, DATETIME, TIMESTAMP和YEAR。每个时间类型有一个有效的范围值，你可以使用0值当你指定MySQL不能表示的无效值。TIMESTAMPE类型具有特殊的自动更新行为，我们在稍后描述。对于时间类型的存储需要，参阅11.8节 “Data Type Storage Requirements”

当与date和time类型工作时把这些建议装在脑海里

* MySQL以标准的格式来提取指定的日期或时间，如果尝试解释你提供了各种输入的格式\(例如，当你指定一个值或与日间或日期相比较时\)。获取允许的日期和时间格式的描述，参阅9.1.3章，“Date and Time Literals”。期待你提供有效的值，如果你使用其他的格式将会发生不可预测的结果。
* 仅管MySQL以几种不同的格式来解释值，日期部分一定要以年-月-日的顺序指定\(例如 “98-09-04”\)，而不是月-日-年或日-月-年的顺序\(例如，“09-04-98”, "04-09-98"\)
* 包含两位数的年份是有区别的因为不知道世纪，MySQL用下面的规则来解释两位数的年份:
  * 70-99的年份值被转换为1970-1999
  * 00-69的年份值被转换为2000-2069\(参阅11.3.8节, "日期中两位数的年份"\)
* 将一个时间类型转换为另一个时间时将根据11.3.7节“Conversion Between Date and Time Types”的规则
* MySQL将自动的把日期或时间值转换为数值类型如果值以数值上下文和,反之亦然。
* 默认的，当MySQL遇到的时间或日期值超出范围或不是有效的值是，将会把值转换为"0"值。TIME值超出范围时会把其裁剪为适当的值
* 通过设置合适的SQL模式值，你可以指定更准确的日期种类MySQL支持的\(参阅 5.1.8章 “Servlet SQL Modes”\)你可以让MySQL支持必要的日期，如"2009-11-31",通过激活ALLOW\_INVALID\_DATES SQL模式。当相当有用当你想存储一个用户指定的可能错误的值\(例如，在web环境下\)到数据库供将来处理。在此模式下，MySQL只验证月份在1-12之间，日期在1-31之间
* MySQL 允许你存储“0”值“0000-00-00”作为“假日期”。这在某些方面比使用NULL值更方便。不允许“0000-00-00”，启动NO\_ZERO\_DATE模式
* “Zero”日期或时间经过Connector/ODBC会自动转化为NULL因为ODBC不能处理这样的值。

下面的表展示了每种类型“0”值的格式。“0”值是特殊的，但是你可以显示的存储或引用他们通过使用表格展示的值。你也可以这样作用'0'或0\(容易写\)。对于包含日期部分\(DATE, DATETIME, 和TIMESTAMP\)，使用这些类型的0值将会产生警告如果NO\_ZERO\_DATE 模式启动的话。

| Data Type | "Zero" Value |
| :--- | :--- |
| DATE | '0000-00-00' |
| TIME | '00:00:00' |
| DATETIME | '0000-00-00 00:00:00' |
| TIMESTAMP | '0000-00-00 00:00:00' |
| YEAR | 0000 |



