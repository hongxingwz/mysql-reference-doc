# 11.3.1 The DATE, DATETIME, and TIMESTAMP Types

DATE, DATETIME 和 TIMESTAMP类型是相关联的。此章介绍了他们的特性，他们的相似性，他们的不同点。MySQL以几种不同的形式识别DATE, DATETIME 和 TIMESTAMP值，在9.1.3节，“Date and Time Literals”中描述。对于DATE和DATETIME范围的描述，“支持“意味着尽管以前的值可能工作，但不会给予保证。

DATE类型用来有日期部分但没有时间部分的值。MySQL以”YYYY-MM-DD“提取和显示DATE值。支持的范围是‘1000-01-01’到‘9999-12-31’。

DATETIME类型用来存储继包含date和time部分的值MySQL以‘YYYY-MM-DD HH:MM:SS’的形式提取和显示DATETIME类型的值。其支持的时间范围是'1000-01-01 00:00:00' 到‘9999-12-31 23:59:59’

TIMESTAMP数据类型用来存储既包含日期和时间部分的值。TIMESTAMP的范围是‘1970-01-01 00:00:01’UTC 到 ‘2038-01-19 03:14:07’UTC

DATETIME 或 TIMESTAMP值可以包含 trailing fractional seconds部分来达到微秒\(6位数字\)的精度。特别指出的是，任何fractional部分插入进DATETIME或TIMESTAMP列被会存储而不是忽略。如果包含fractional部分，这些值的形式是"YYYY-MM-DD HH:MM:SS\[.fraction\]",DATETIME值的范围是'1000-01-01 00:00:00.000000' 到‘9999-12-31 23:59:59.999999’。TIMESTAMP值的范围是‘1970-01-01 00:00:01.00000’ 到’2038-01-19 03:14:07.999999‘。The fractional部分应该永远与日期的其它部分用decimal point分开；

