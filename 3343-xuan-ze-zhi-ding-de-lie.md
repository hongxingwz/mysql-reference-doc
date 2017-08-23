# Selecting Particular Columns

如果你不想查看你表中所有的行，指定你感兴趣的列就可以了，通过逗号分割。例如，你想知道你的动物是什么时候出生的，选择name和birth列:

```
mysql> select name, birth from pet;
+----------+------------+
| name     | birth      |
+----------+------------+
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Buffy    | 1989-05-13 |
| Fang     | 1990-08-27 |
| Bowser   | 1989-08-31 |
| Chirpy   | 1998-09-11 |
| Whistler | 1997-12-09 |
| Slim     | 1996-04-29 |
| Puffball | 1999-03-30 |
+----------+------------+
```

查找谁拥有pets，使用下面的查询:

```
mysql> select owner from pet;
+--------+
| owner  |
+--------+
| Harold |
| Gwen   |
| Harold |
| Benny  |
| Diane  |
| Gwen   |
| Gwen   |
| Benny  |
| Diane  |
+--------+
```

注意查询仅从每条记录撮owner列，一些owner出现了不只一次。为了最小化输出，通过添加DISTINCT关键字来提取唯一的输出：

```
mysql> select DISTINCT owner from pet;
+--------+
| owner  |
+--------+
| Harold |
| Gwen   |
| Benny  |
| Diane  |
+--------+
```

你可以使用WHERE语句来结合行选择和列选择。例如，获取仅获取猫和狗的出生日期，使用下面的查询:

```
mysql> select name, species, birth from pet
    -> where species = 'dog' or species = 'cat';
+--------+---------+------------+
| name   | species | birth      |
+--------+---------+------------+
| Fluffy | cat     | 1993-02-04 |
| Claws  | cat     | 1994-03-17 |
| Buffy  | dog     | 1989-05-13 |
| Fang   | dog     | 1990-08-27 |
| Bowser | dog     | 1989-08-31 |
+--------+---------+------------+
```



