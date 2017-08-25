# Calculating Visits Per Day

下面的示例展示了怎么你可以使用一小组函数来计算每个月用户访问网站量的天数：

```
mysql> CREATE TABLE t1 (year YEAR(4), month INT(2) UNSIGNED ZEROFILL,
    ->              day INT(2) UNSIGNED ZEROFILL);
Query OK, 0 rows affected (0.02 sec)

mysql> INSERT INTO t1 VALUES(2000,1,1),(2000,1,20),(2000,1,30),(2000,2,2),
    ->             (2000,2,23),(2000,2,23);
```

```
mysql>  SELECT year,month,BIT_COUNT(BIT_OR(1<<day)) AS days FROM t1
    ->        GROUP BY year,month;
+------+-------+------+
| year | month | days |
+------+-------+------+
| 2000 |    01 |    3 |
| 2000 |    02 |    2 |
+------+-------+------+
```



