# 12.7 日期和时间函数\(Date and Time Functions\)

此节描述的函数可以用来获取时间值。参阅11.3 "Date and Time Types",来获取每个日期和时间类型的范围，和每个值可以指定的有效形式。

**日期和时间函数**

| 名字 | 描述 |
| :--- | :--- |
| ADDDATE\(\) |  |
| ADDTIME\(\) |  |
| CONVERT\_TZ\(\) |  |
| CURDATE\(\) |  |
| CURRENT\_DATE\(\), CURRENT\_DATE |  |
| CURRENT\_TIME\(\), CURRENT\_TIME |  |
| CURRENT\_TIMESTAMP, CURRENT\_TIMESTAMP |  |
| CURTIME\(\) |  |
| DATE\(\) |  |
| DATE\_ADD\(\) |  |
| DATE\_FORMAT\(\) |  |
| DATE\_SUB\(\) |  |
| DATEDIFF\(\) |  |
| DAY\(\) |  |

这有一个使用日期函数的示例。下面的查询查询`date\_col`值在上30天内的所有行。


```
 mysql> SELECT something FROM tbl_name
-> WHERE DATE_SUB(CURDATE(),INTERVAL 30 DAY) <= date_col;
```

查询也可以选取在将来的日期。

期待日期函数接收日期时间值且忽略时间部分。期待时间值函数通常接受日期时间且忽略日期部分。


