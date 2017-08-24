# 3.3.4.8 Counting Rows

数据库经常用来回答问题，”一个确定的类型在表中出现了多少次？“例如，你可能想知道你拥有多个少个宠物，或者每个拥有者有多少宠物，或你可能执行各种行样的统计计算在你的动物上。

计算动物的数量与”在pet表中有多少行记录是一样的“？因为每一个宠物一行。COUNT\(\*\)计算行的数量，因此计算动物数量的查询看起来像这样:

```
mysql> SELECT COUNT(*) FROM pet;
+----------+
| COUNT(*) |
+----------+
|        9 |
+----------+
```

之前，你提取拥有宠物的人的名字。你可以使用COUNT\(\)如果你想找出每个拥有者拥有多少个宠物:

```
mysql> SELECT OWNER, COUNT(*) FROM pet GROUP BY owner;
+--------+----------+
| OWNER  | COUNT(*) |
+--------+----------+
| Benny  |        2 |
| Diane  |        2 |
| Gwen   |        3 |
| Harold |        2 |
+--------+----------+
```

前面的查询使用GROUP BY 来把所有的记录用每个owner分组。COUNT\(\)与GROUP BY结合使用在各种各样的组中描述你的数据是很有用的。下面的例子展示了执行不同的动物统计操作。

每种宠物的数量:

```
mysql> select species, count(*) from pet group by species;
+---------+----------+
| species | count(*) |
+---------+----------+
| bird    |        2 |
| cat     |        2 |
| dog     |        3 |
| hamster |        1 |
| snake   |        1 |
+---------+----------+
```

每种性别的动物的数量:

```
mysql> SELECT sex, COUNT(*) FROM pet group by sex;
+------+----------+
| sex  | COUNT(*) |
+------+----------+
| NULL |        1 |
| f    |        4 |
| m    |        4 |
+------+----------+
```

\(在此输出中,NULL表示未知的性别\)

种类和性别结合的动物的数量:

```
mysql> SELECT species, sex, count(*) from pet group by species, sex;
+---------+------+----------+
| species | sex  | count(*) |
+---------+------+----------+
| bird    | NULL |        1 |
| bird    | f    |        1 |
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
| hamster | f    |        1 |
| snake   | m    |        1 |
+---------+------+----------+
```

你不需要从全表中提取当你使用COUNT\(\)。例如，前面的查询，在仅在dogs和cats上执行时，看起来像下面:

```
mysql> select species, sex, count(*) from pet where species = 'dog' or species = 'cat'
    -> group by species,sex;
+---------+------+----------+
| species | sex  | count(*) |
+---------+------+----------+
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
+---------+------+----------+
```

或，你想知道已知性别的动物的数量:

```
mysql> select species, sex, count(*) from pet
    -> where sex is not null
    -> group by species, sex;
+---------+------+----------+
| species | sex  | count(*) |
+---------+------+----------+
| bird    | f    |        1 |
| cat     | f    |        1 |
| cat     | m    |        1 |
| dog     | f    |        1 |
| dog     | m    |        2 |
| hamster | f    |        1 |
| snake   | m    |        1 |
+---------+------+----------+
```

如果你想在COUNT\(\)后面添加额外的名字列，那么在GROUP BY 语句后面也要具有同样的列。否则，下面将会发生:

* 如果ONLY\_FULL\_GROUP\_BY SQL模式被激活，一个错误将会发生

```
mysql> select owner, count(*) from pet;
ERROR 1140 (42000): In aggregated query without GROUP BY, expression #1 of SELECT list contains nonaggregated column 'menagerie.pet.owner'; this is incompatible with sql_mode=only_full_group_by
```



