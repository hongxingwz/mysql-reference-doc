# 3.6 Examples of Common Queries

这有一些例子用MySQL解决一些通用的问题。

一些示例用表shop来保存每种商品\(编号\)对于经销商。假定每个经销商对于每种商品具有单一固定的价格,然后商品是记录的主键

开起mysql的命令行工具然后选择一个数据:

```
shell>mysql your-database-name
```

\(在大多数的MySQL安装版中，你可以使用名叫test的数据库\)

你可以创建和填充示例表用这些语句:

```
mysql> CREATE TABLE shop (
    ->     article INT(4) UNSIGNED ZEROFILL DEFAULT '0000' NOT NULL,
    ->     dealer  CHAR(20)                 DEFAULT ''     NOT NULL,
    ->     price   DOUBLE(16,2)             DEFAULT '0.00' NOT NULL,
    ->     PRIMARY KEY(article, dealer));
Query OK, 0 rows affected (0.03 sec)

mysql> INSERT INTO shop VALUES
    ->     (1,'A',3.45),(1,'B',3.99),(2,'A',10.99),(3,'B',1.45),
    ->     (3,'C',1.69),(3,'D',1.25),(4,'D',19.95);
Query OK, 7 rows affected (0.00 sec)
```

在执行这些语句后，这个表应该具有下面的内容:

```
mysql> select * from shop;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0001 | A      |  3.45 |
|    0001 | B      |  3.99 |
|    0002 | A      | 10.99 |
|    0003 | B      |  1.45 |
|    0003 | C      |  1.69 |
|    0003 | D      |  1.25 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
```



