# 12.2 Type Conversion in Expression Evalutation

当操作符用来操作两个不同类型的操作数时，会发生类型转换使操作数相兼容。一些转换是隐式发生的。例如，MySQL自动的将数值转化为字符如果必要，反之亦然

```
mysql> select concat(2, 'test');
+-------------------+
| concat(2, 'test') |
+-------------------+
| 2test             |
+-------------------+
1 row in set (0.00 sec)
```

也可以将数字转换为字符显示的使用CAST\(\)函数。转换隐匿的发生当使用CONCAT\(\)函数因为他期待string参数

```
mysql> SELECT 38.8, cast(38.8 as char);
+------+--------------------+
| 38.8 | cast(38.8 as char) |
+------+--------------------+
| 38.8 | 38.8               |
+------+--------------------+

mysql> select 38.8, concat(38.8);
+------+--------------+
| 38.8 | concat(38.8) |
+------+--------------+
| 38.8 | 38.8         |
+------+--------------+
1 row in set (0.00 sec)
```

参阅此节关于隐式将数值转换为字符的转换的字符集和modified rules that apply to CREATE TABLE ... SELECT 语句。

下面的规则描述了比较操作的转换发生的规则:

* 如果一个或两个参数都是NULL，比较返回的结果是NULL，除了NULL自身的&lt;=&gt;相等性比较操作。对于NULL&lt;=&gt;NULL,结果是true。不需要转换。

* 如果两个参数都是字符，则作为字符串比较。

* 如果两个参数都是整数，则他们作为整数比较。
* 十六进制的值将被看成二进制字符如果没有与数值比较
* 如果一个参数是TIMESTAMP或DATATIME列，其他的参数是常量，常量被转换为timestamp在比较执行之前。这比ODBC作的更友好。注意这在IN\(\)的参数中是不会这样做的！为了安全，永远使用完全的datetime, date, 或time字符串在做比较时。例如，在用BETWEEN的date或time参数时为了达到更好的效果，使用CAST\(\)显示的将值转换为渴望的值。
* 一个single-row从一个表或多个表返回的不被认为是常量。例如，如果一个子查询返回一个整数来合DATETIME值作比较，比较会把他们都作为整数比较。整数不会被转换为时间值。要将操作数作为DATETIME值比较，使用CAST\(\)来显示的将子查询的值转换为DATETIME
* 如果一个参数是decimal值，比较取决于另一个参数。如果另一个参数是一个decimal或integer。如果另一个参数是浮点类型，则会作为浮点类型比较
* 剩下的其他情况，参数都作为浮点类型比较

## 总结：

SQL Schema:

```sql
DROP TABLE IF EXISTS `test`;

CREATE TABLE IF NOT EXISTS `test`(
  id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(30),
  password VARCHAR(30)
)ENGINE InnoDB CHARSET UTF8;

INSERT INTO `test` (`name`, `password`) VALUES ("test1", "password1"), ("test2", "password2");
```

安全问题：假如**password**类型为字符串，查询条件为int 0 则会匹配上。

```sql
mysql> SELECT * FROM `test`;
+----+-------+-----------+
| id | name  | password  |
+----+-------+-----------+
|  1 | test1 | password1 |
|  2 | test2 | password2 |
+----+-------+-----------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM `test` WHERE `name` = 'test1' AND `password` = 0;
+----+-------+-----------+
| id | name  | password  |
+----+-------+-----------+
|  1 | test1 | password1 |
+----+-------+-----------+
1 row in set, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+-----------------------------------------------+
| Level   | Code | Message                                       |
+---------+------+-----------------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'password1' |
+---------+------+-----------------------------------------------+
1 row in set (0.00 sec)
```

相信上面的例子，一些机灵的同学可以发现其实上面的例子也可以做sql注入。

假设网站的登录那块做的比较挫，使用下面的方式：

```
SELECT * FROM users WHERE username= '$_POST["username"]' AND password = '$_POST["password"]'
```

如果username输入的是`a' OR 1 = '1`，那么password随便输入，这样就生成了下面的查询：


```
SELET * FROM users WHERE username = 'a' OR 1 = '1'AND password = 'anyvalue'
```
就有可能登录系统。其实如果攻击者看过了这篇文章，那么就可以利用隐a工转化来进行登录了。如下：


```
INSERT INTO `test` (`name`, `password`) VALUES ('aaa', 'aaaa'), ('55aaa', '55aaaa');

mysql> SELECT * FROM `test`;
+----+-------+-----------+
| id | name  | password  |
+----+-------+-----------+
|  1 | test1 | password1 |
|  2 | test2 | password2 |
|  3 | aaa   | aaaa      |
|  4 | 55aaa | 55aaaa    |
+----+-------+-----------+
4 rows in set (0.00 sec)

mysql> SELECT * FROM `test` WHERE name = 'a' + '55';
+----+-------+----------+
| id | name  | password |
+----+-------+----------+
|  4 | 55aaa | 55aaaa   |
+----+-------+----------+
1 row in set, 5 warnings (0.00 sec)
```

之所以出现上述的原因是因为：


```
mysql> SELECT '55aaaa' = 55;
+---------------+
| '55aaaa' = 55 |
+---------------+
|             1 |
+---------------+
1 row in set, 1 warning (0.00 sec)

mysql> SELECT 'a' + '55';
+------------+
| 'a' + '55' |
+------------+
|         55 |
+------------+
1 row in set, 1 warning (0.00 sec)
```

下面通过一些例子来复习一下上面的转换规则：


```
mysql> SELECT 1 + 1;
+-------+
| 1 + 1 |
+-------+
|     2 |
+-------+
1 row in set (0.00 sec)

mysql> SELECT 'aa' + 1;
+----------+
| 'aa' + 1 |
+----------+
|        1 |
+----------+
1 row in set, 1 warning (0.00 sec)

mysql> SHOW WARNINGS;
+---------+------+----------------------------------------+
| Level   | Code | Message                                |
+---------+------+----------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'aa' |
+---------+------+----------------------------------------+
1 row in set (0.00 sec)


```
把字符中 "aa"和1进行求和，得到1，因为"aa"和数字1的类型不同，MySQL官方文档告诉我们：

> When an operator is used with operands of different types, type conversion occurs to make the operands compatible.

查看warnings可以看到隐式转换把字符串转换为了double类型。但是因为字符串是非数字型的，所以就会被转换为0，因此最终计算的是`0 + 1 = 1` 

上面的例子是类型不同，所以出现了隐式转化，那么如果我们使用相同类型的值进行运算呢？


```
mysql> SELECT 'a' + 'b';
+-----------+
| 'a' + 'b' |
+-----------+
|         0 |
+-----------+
1 row in set, 2 warnings (0.00 sec)

mysql> SHOW WARNINGS;
+---------+------+---------------------------------------+
| Level   | Code | Message                               |
+---------+------+---------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'a' |
| Warning | 1292 | Truncated incorrect DOUBLE value: 'b' |
+---------+------+---------------------------------------+
2 rows in set (0.00 sec)
```
是不是有点郁闷呢？
之所以出现这种情况，是在为`+`为算术操作符(arithmetic operator)这样就可以解释为什么a和b都转换为double了。因为转换之后其实就是: `0 + 0 = 0`了

在看一个例子：



```
mysql> SELECT 'a' + 'b' = 'c';
+-----------------+
| 'a' + 'b' = 'c' |
+-----------------+
|               1 |
+-----------------+
1 row in set, 3 warnings (0.00 sec)

mysql> SHOW WARNINGS;
+---------+------+---------------------------------------+
| Level   | Code | Message                               |
+---------+------+---------------------------------------+
| Warning | 1292 | Truncated incorrect DOUBLE value: 'a' |
| Warning | 1292 | Truncated incorrect DOUBLE value: 'b' |
| Warning | 1292 | Truncated incorrect DOUBLE value: 'c' |
+---------+------+---------------------------------------+
3 rows in set (0.00 sec)
```
现在就看也很好的理解上面的例子了吧。`a+b=c`结果为1，1在MySQL中可以理解为TRUE, 因为`'a' + 'b'`的结果为0，`c`也会隐式转换为0，因此比较其实是：`0=0`也就是true，也就是1.

**第二个需要注意点就是防止多查询或者删除数据**



```
mysql> INSERT INTO `test` (`name`, `password`) VALUES  ('1212', 'aaa'),('1212a', 'aaa');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> SELECT * FROM `test`;
+----+-------+-----------+
| id | name  | password  |
+----+-------+-----------+
|  1 | test1 | password1 |
|  2 | test2 | password2 |
|  3 | aaa   | aaaa      |
|  4 | 55aaa | 55aaaa    |
|  5 | 1212  | aaa       |
|  6 | 1212a | aaa       |
+----+-------+-----------+
6 rows in set (0.00 sec)

mysql> SELECT * FROM `test` WHERE name = 1212;
+----+-------+----------+
| id | name  | password |
+----+-------+----------+
|  5 | 1212  | aaa      |
|  6 | 1212a | aaa      |
+----+-------+----------+
2 rows in set, 5 warnings (0.00 sec)

mysql> SELECT * FROM `test` WHERE `name`='1212';
+----+------+----------+
| id | name | password |
+----+------+----------+
|  5 | 1212 | aaa      |
+----+------+----------+
1 row in set (0.00 sec)
```
上面的例子本意是查询id为5的那一条记录，结果把id为6的那一条也查询来了。 我想说明什么情况呢？有时候我们的数据库表中的一些列是varchar类型，但是存储的值为'1123'这种的纯数字的字符串值，一些同学写sql的时候又不习惯加引号。这样当进行SELECT, UPDATE 或者 DELETE的时候就可能会多操作一些数据。所以应该加引号的地方别忘记了。

**关于字符串转数字的一些说明**


```
mysql> SELECT 'a' = 0;
+---------+
| 'a' = 0 |
+---------+
|       1 |
+---------+
1 row in set, 1 warning (0.00 sec)

mysql> SELECT '1a' = 1;
+----------+
| '1a' = 1 |
+----------+
|        1 |
+----------+
1 row in set, 1 warning (0.01 sec)

mysql> SELECT '1a1b' = 1;
+------------+
| '1a1b' = 1 |
+------------+
|          1 |
+------------+
1 row in set, 1 warning (0.00 sec)

mysql> SELECT 'a1a1b' = 0;
+-------------+
| 'a1a1b' = 0 |
+-------------+
|           1 |
+-------------+
1 row in set, 1 warning (0.00 sec)
```
从上面的例子可以看出，当把字符串转换数的时候，其实是从左边处理 。
* 如果字符串的第一个字符就是非数字的字符，那么转换数字就是0
* 如果字符串中都是数字，那么转换为数字就是整个字符串对应的数字
* 如果字符串中存在非数字，那么转换为的数字就是开头的那些数字对应的值








