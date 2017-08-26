# 11.3.1 The DATE, DATETIME, and TIMESTAMP Types

DATE, DATETIME 和 TIMESTAMP类型是相关联的。此章介绍了他们的特性，他们的相似性，他们的不同点。MySQL以几种不同的形式识别DATE, DATETIME 和 TIMESTAMP值，在9.1.3节，“Date and Time Literals”中描述。对于DATE和DATETIME范围的描述，“支持“意味着尽管以前的值可能工作，但不会给予保证。

DATE类型用来有日期部分但没有时间部分的值。MySQL以”YYYY-MM-DD“提取和显示DATE值。支持的范围是‘1000-01-01’到‘9999-12-31’。

DATETIME类型用来存储继包含date和time部分的值MySQL以‘YYYY-MM-DD HH:MM:SS’的形式提取和显示DATETIME类型的值。其支持的时间范围是'1000-01-01 00:00:00' 到‘9999-12-31 23:59:59’

TIMESTAMP数据类型用来存储既包含日期和时间部分的值。TIMESTAMP的范围是‘1970-01-01 00:00:01’UTC 到 ‘2038-01-19 03:14:07’UTC

DATETIME 或 TIMESTAMP值可以包含 trailing fractional seconds部分来达到微秒\(6位数字\)的精度。特别指出的是，任何fractional部分插入进DATETIME或TIMESTAMP列被会存储而不是忽略。如果包含fractional部分，这些值的形式是"YYYY-MM-DD HH:MM:SS\[.fraction\]",DATETIME值的范围是'1000-01-01 00:00:00.000000' 到‘9999-12-31 23:59:59.999999’。TIMESTAMP值的范围是‘1970-01-01 00:00:01.00000’ 到’2038-01-19 03:14:07.999999‘。The fractional部分应该永远与日期的其它部分用decimal point分开；其他的fractional seconds delimiter不会再被识别了。在MySQL中关于更多的fractional seconds 支持，参阅11.3.6节 ”Fractional Seconds in Time Values“。

TIMESTAMP和DATETIME类型提供自动初始化\(automatic initialization\) 和更新到当前的日期和日间\(updating to the current date and time\).获取更多的信息，参阅Section 11.3.5 "Automatic Initialization and Updating for TIMESTAMP and DATETIME"

MySQL将TIMESTAMP从当前的区时到UTF来存储，并在提取时从UTC转换到当前的区时\(这在其他时间类型不会发生\)默认的，每个连接的当前区时是服务器的时间。基于每个连接也可以设置区时。只要时区保持一致，你获取的时间与存储的时间是一样的。如果你存储了一个TIMESTAMP值，然后改变了时区然后提取值，提取的值与你存储的值不一样。这样是因为同一时区不用于双向转换。当前时区的值同time\_zone系统变化的值一样。获取更多的信息，参阅10.6章，“MySQL Server Time Zone Support”。

无效的DATE, DATETIME 或 TIMESTAMP值被转换成“0”值的相对应型式\('0000-00-00' 或 ’0000-00-00 00:00:00‘\)

注意在MySQL日期值解释的某些性质

* MySQL允许指定的字符串作为日期的宽松型式，任何标点符号字符可以用来作为日期或时间部分的分割。在一些情况下，这种语法具有欺骗性。例如，像“10:11:12”看起来像时间值因为“:”但是确被像年“2010-11-12”解释如果在日期上下文中使用。“10:45:15”被转换为“0000-00-00”因为’45‘不是有效的月份。
 在日期和时间和fractional seconds部分的唯一分割符(mysql识别的)是decimal point

* 服务器要求月份和天数是有效的，不仅是1-12和1-31的范围。在严格模式没有启用下，无效的日期例如'2004-04-31'被转换为'0000-00-00'并会生成一个警告。在严格模式下，无效的日期生成一个错误，为了允许这样的日期，启用ALLOW\_INVALID\_DATES。参阅5.1.8节 "Server SQL Modes"获取更多的信息

* MySQL不接受TIMESTAMP值在天或月份中包含0值。唯一的例外是"0"值'0000-00-00 00:00:00'

* 包含两位数年份的日期是有区分的因为世纪是未知的。MySQL解释两位数的年份以以下两种形式：
 * Year值在00-69之间被转换成2000-2069
 * Year值在70-99之间被转换为1970-1999
 查看 11.3.8节 "Two-Digit Years in Dates"

>注：
>
> MySQL服务可以以MAXDB SQL模式运行。在这种情况下，TIMESTAMP与DATETIME一样。如果在这种模式起用下创建一个表，TIMESTAMP列作为DATETIME列创建，作为结果，这样的列以DATETIME格式显示，与DATETIME具有同样的范围，there is no automatic initialization or updating to the current date and time.参阅5.1.8节 "Server SQL Modes"


