# 11.3.2 The TIME Type

MySQL以”HH:MM:SS“形式提取和展示TIME值\(或”HHH:MM:SS“形式对于large hours values\).TIME值可以从”-838:59:59“到”838:59:59“。小值部分可以很大因为TIME不仅可用来表示一个天的时间\(要小于24小时\)，但也可以是流逝的时间或两个事件的间隔时间\(可能大于24小时，或甚至是负值\)。

MySQL以几种形式识别TIME值，一些可以包括a trailing fractional seconds part in up to microseconds\(6 digits\)precision。参阅9.1.3章， "Date and Time Literals"。获取更多的关于fractional seconds MySQL对于其的支持，参阅11.3.6 ”Fractional Seconds in Time Values“。特殊是，任何fractional part在值中的都会被存储进TIME列而不是被忽略。包括fractional part, TIME的值的范围是"-838:59:59.000000" 到 ”838:59:59.000000“ 。

当把简写的值赋与TIME列时要小心。MySQL interprets abbreviated TIME values with colons as time of the day.也就是，”11:12“表示”11:12:00“,而不是”00:11:12“。MySQL interprets abbreviated  values without colons using the assumption that the two rightmost digits represent seconds\(that is, as elapsed time rather than as time of a day\).例如，你可能认为'1112'和1112表示’11:12:00‘（11点后的12分钟\),但MySQL解释他们成"00:11:12"\(11分钟，12秒\)。简单的'12'和12被解释为'00:00:12'

The only delimiter recognized between a time part and a fractional seconds part is the decimal point.

默认的，TIME如果超出范围但是有效的会被裁剪成最近范围的值。例如,'-850:00:00' 和 '850:00:00'会被转换成'-838:59:59'和’838:59:59‘。无效的TIME值被转换成'00:00:00'。因为'00:00:00'本身是合法的TIME值，因此没有方式告诉,在表里存储的’00:00:00‘是否是原始指定的值还是无效的值。

关于TIME值更多的限制，启用严格模式来使错误发生。参阅5.1.8章，”Server SQL 模式“

