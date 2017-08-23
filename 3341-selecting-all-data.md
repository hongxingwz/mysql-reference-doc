# Selecting All Data

从一个表中提取所有数据的最简单的形式

```
mysql> select * from pet;
+----------+--------+---------+------+------------+------------+
| name     | owner  | species | sex  | birth      | death      |
+----------+--------+---------+------+------------+------------+
| Fluffy   | Harold | cat     | f    | 1993-02-04 | NULL       |
| Claws    | Gwen   | cat     | m    | 1994-03-17 | NULL       |
| Buffy    | Harold | dog     | f    | 1989-05-13 | NULL       |
| Fang     | Benny  | dog     | m    | 1990-08-27 | NULL       |
| Bowser   | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
| Chirpy   | Gwen   | bird    | f    | 1998-09-11 | NULL       |
| Whistler | Gwen   | bird N  | 1    | NULL       | NULL       |
| Slim     | Benny  | snake   | m    | 1996-04-29 | NULL       |
| Puffball | Diane  | hamster | f    | 1993-03-30 | NULL       |
+----------+--------+---------+------+------------+------------+

```

如果你想重现表中所有的的数据SELECT的形式是非常有用的，例如，当你在加载了你的初始化数据集。例如，你可能发现Bowser的出生日期不是很正确。咨询你原始的家庭而，你发现正确的日期应该是1989,而不是1979。

这里至少有两种方式来修正：

* 编辑pet.txt文件来修正错误，然后使用DELETE和LOAD DATA来清空表重新加载数据:

```
mysql> DELETE FROM pet;
mysql> LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet;
```

然而，如果你这样做，你必须要重新输入Puffball的记录。

* 使用UPDATE语句来修正错误的记录:

```
mysql> UPDATE pet SET birth="1989-08-31" WHERE name = 'Bowser';
```

UPDATE仅不更改有问题的记录，不需要你重新加载表

