# 日期函数

DAYOFWEEK\(date\)    周几 \(1=星期天\)  
WEEKDAY\(date\)      周几 \(0=星期一\)  
DAYOFMONTH\(date\)    几号  
DAYOFYEAR\(date\)    一年的第几天

---

YEAR\(date\)        日期中的年  
MONTH\(date\)        日期中的月  
HOUR\(date\)        日期中的小时  
MINUTE\(date\)       日期中的分钟  
SECOND\(date\)        日期中的秒

---

## 当前的日间

NOW\(\)  
SYSDATE\(\)  
CURRENT\_TIMESTAMP\(\)  
current\_date

## 日期格式化

DATE\_FORMAT\(date\)

经常用的 DATE\_FORMAT\(now\(\), "%Y-%m-%d %H:%i:s"

timestampdiff()

TIMESTAMPADD()

DATE\_ADD()

DATEDIFF()

# 天坑：

如果对于datetime类形的列，如果你输入"2017-08-24"，mysql 会将其视为"2017-08-24 00:00:00"。所以当你在使用下面的sql查询时

```
select * from table where createDate <= "2017-08-24"
```

会查询不到日期为2017-08-24 17:20:46

转换思路，使用下面的SQL

```
select * from table where DATE(createDate) <= "2017-08-24"
select * from table where DATE(createDate) < "2017-08-25" #感觉这个最好 效率最高
```



