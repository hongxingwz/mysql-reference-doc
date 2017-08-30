# 11.7 Data Type Default Values

DEFAULT value语句在数据类型声明了一列的默认值。唯一的例外是，默认值必须是一个常量；它不能是函数或一个表达式。这代表着，例如，不能把一个日期列的默认值设置为一个像NOW\(\)或CURRENT\_DATE的函数。例外是你可指定CURRENT\_TIMESTAMP作为TIMESTAP和DATETIME列的默认值。参阅"11.3.5章" "Automatic Initialization and Updating for TIMESTAMP and DATETIME"。

BLOB, TEXT, GEOMETRY, 和JSON列不能被赋予默认值。

如果一列定义时没有显示的默认值，MySQL以下面的方式决定默认值：

如果列可以用NULL作为其值，列显示的定义为DEFAULT NULL语句

如果列不可以用NULL作为其值，MySQL定义列没有显示的DEFAULT语句。

例外是：如果一列定义为PRIMARY KEY的一部分但不是显示的NOT NULL，MySQL作为NOT NULL列创建时\(因为PRIMARY KEY列一定是NOT NULL的\)。 在MySQL5.7.3之前，该列也会隐式的赋予一个默认值。来阻止这，在定义PRIMARY KEY列时包含显示的NOT NULL定义





对于指定的表，你可以使用SHOW CREATE TABLE语句来查看哪列具有显示的DEFAULT语句。

隐式的默认值下面描述的这样定义：

* 对于数值类型默认值是0，例外的是对于整数或浮点数列声明为AUTO\_INCREMENT属性，默认值是序列中的下一个值
* 对于date和time types类型除了TIMESTAMP，默认相关的值是"zero"对于TIMESTAMP类型也是适用的如果"explicit\_defaults\_for\_timestamp"系统变量的值被启用（参阅5.1.5章 "Server System Variables"）。否则，对于一个表中的第一列timestamp值，默认是当前的日期。参阅11.3章"Date and Time Types"
* 对于字符串除了ENUM类型，默认值是空的字符串。对于ENUM类型，默认是枚举的第一个值。 
如果在整数列定义SERIAL DEFAULT VALUE 是NOT NULL AUTO_INCREMENT UNIQUE的别名


