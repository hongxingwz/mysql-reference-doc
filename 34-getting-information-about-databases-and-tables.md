# 3.4 Getting Information About Databases and Tables

如果你忘记了数据库和表是什么名字，或指定的table是什么结构\(例如，列的名字叫什么\)？ MySQL通过几条提供语句来提供数据库和表信息来定位这样的问题。

你在之前看到了SHOW DATABASES, 可以列出由server管理的数据库。来找出当前选择了哪个数据库，使用DATABASE\(\)函数：

```
mysql> select database();
+------------+
| database() |
+------------+
| menagerie  |
+------------+
```

如果你还没有选择任何的数据库，那么返回的结果是NULL

找出默认的数据库包含哪些表\(例如，当你不确定表名时\),使用下面的语句:

```
mysql> show tables;
+---------------------+
| Tables_in_menagerie |
+---------------------+
| event               |
| pet                 |
+---------------------+
```

此语句产生的输出列的名字总是Tables\_in\_db\_name,db\_name是数据库的名字。查看13.7.5.37章，获取更多的信息。

如果你想找出表的结构，DESCRIBE语句是很有用的；他展示了表中每列的信息

```
mysql> desc pet;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
6 rows in set (0.01 sec)
```

Field表示列的名字，Type是列的类型，NULL表示此列能否包含NULL值，Key表示此列是否是索引，Default指定了列的默认值。Extra展示了此列的特殊信息：如果创建时具有AUTO\_INCREMENT选项，那么此列的值将会是auto\_increment而不是空的。

DESC是DESCRIBLE的简写形式。查看Section 13.8.1 "DESCRIBE Syntax"，来获取更详细的信息。

你必要时可以获取创建表的信息通过使用CREATE TABLE语句。查阅 13.7.5.10 "SHOW CREATE TABLE Syntax".

如果在表上有索引，SHOW INDEX FROM tb1\_name产生关于索引的信息。查看Section 13.7.5.22 "SHOW INDEX Syntax",获取更多关于此语句的信息。

