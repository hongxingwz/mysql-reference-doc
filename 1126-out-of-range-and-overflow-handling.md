# 11.2.6 Out-of-Range and Overflow Handling

当MySQL往数值列中存储一个值时，超出了该数值类型允许的范围，结果取决于此时的SQL模式

* 如果启用了strict SQL model，MySQL以一个错误来拒绝超出范围的值，并具插入失败，与SQL标准相一致
* 如果启用了no restrictive mode，MySQL会把该值裁剪为数据范围合理的值并存储

当一个超出范围的值赋予一个整型列时，MySQL stores the value representing the corresponding endpoint of the column data type range.

When a floating-point or fixed-point column is assigned a value that exceeds the range implied by the specified\(or default\)precision and scale, MySQL stores the value representing the corresponding endpoint of that range

假定表t1是这样定义的

```
mysql> CREATE TABLE t1 (i1 tinyint, i2 tinyint unsigned);
```

当启用strict SQL模式时，发生一个超出范围的错误：

```
set sql_model = 'TRADITIONAL';
mysql> insert into t1(i1, i2) values (256, 256);
ERROR 1264 (22003): Out of range value for column 'i1' at row 1
mysql> select * from t1;
Empty set (0.00 sec)
```

当不启用严格的SQL模式时，裁剪并产生一个警告：

```
mysql> set sql_mode = '';
mysql> insert into t1(i1, i2) values(256, 256);
Query OK, 1 row affected, 2 warnings (0.00 sec)
mysql> show warnings;
mysql> select * from t1;
+------+------+
| i1   | i2   |
+------+------+
|  127 |  255 |
+------+------+
```

当strict SQL模式没有起用时，列赋值转换在裁剪时会以警告作报告在ALTER TABLE, LOAD DATA INFILE, UPDATE, 和多行INSERT语句。在strict mode，这些语句会失败，一些或所有的值会插入失败，取决于表是否开启了事物和其他一些因素。获取详细的信息，参阅5.1.8节 “Server SQL Modes” 

