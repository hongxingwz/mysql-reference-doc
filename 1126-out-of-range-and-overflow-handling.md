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

在数值表达式执行期间溢出会返回一个错误。有符号BIGINT的最大值是9223372036854775807,因此下面的表达式将产生一个错误：

```
mysql> select 9223372036854775807 + 1;
ERROR 1690 (22003): BIGINT value is out of range in '(9223372036854775807 + 1)'
```

为了使此运算执行，将该值转换为无符号的:

```
mysql> select cast(9223372036854775807 as unsigned) + 1;
+-------------------------------------------+
| cast(9223372036854775807 as unsigned) + 1 |
+-------------------------------------------+
|                       9223372036854775808 |
+-------------------------------------------+
```

是否溢出取决于操作数的范围，因此别一种处理方法是表达式使用精确的算术值，因为DECIMAL比整数有更大的范围：

```
mysql> select 9223372036854775807.0 + 1;
+---------------------------+
| 9223372036854775807.0 + 1 |
+---------------------------+
|     9223372036854775808.0 |
+---------------------------+
```

在整数值相减，如果一个是UNSIGNED,默认产生一个unsigend值。如果结果产生一个负数，将会返回一个错误：

```
mysql> select cast(0 as unsigned) - 1;
ERROR 1690 (22003): BIGINT UNSIGNED value is out of range in '(cast(0 as unsigned) - 1)'
```

如果启用了NO\_UNSIGEND\_SUBTRACTION SQL模式，结果将会是负数：

```
mysql> SET sql_mode = 'NO_UNSIGNED_SUBTRACTION';
mysql> SELECT CAST(0 AS UNSIGNED) -1;
+------------------------+
| CAST(0 AS UNSIGNED) -1 |
+------------------------+
|                     -1 |
+------------------------+
```

如果此种运算的结果用于更新一个UNSIGNED整数列，结果将裁剪为该列的最小值，或裁剪为0如果NO\_UNSIGNED\_SUBTRACTION启用。如果strict SQL模式启用，发生一个错误，列保持不变。

