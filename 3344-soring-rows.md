# Sorting Rows

你可能注意到在前面的例子中返回的行显示时没有特别的顺序。当行以有意义的方式进行排序是很容易检测查询。排序结果，使用ORDER BY语句.

这是动物的出生日期，以日期排序:

```
mysql> select name, birth from pet order by birth;
+----------+------------+
| name     | birth      |
+----------+------------+
| Buffy    | 1989-05-13 |
| Bowser   | 1989-08-31 |
| Fang     | 1990-08-27 |
| Fluffy   | 1993-02-04 |
| Claws    | 1994-03-17 |
| Slim     | 1996-04-29 |
| Whistler | 1997-12-09 |
| Chirpy   | 1998-09-11 |
| Puffball | 1999-03-30 |
+----------+------------+
```

在字符类型的列，排序-像其他的比较一样-通常以大小写不敏感的方式执行。这意味着当列相同只是大小写不同的顺序是未定义。你可以强制使用大小写敏感的列排序通常指定BINARY像这样:ORDER BY BINARY col\_name。

默认的排序是升序的，最小值在第一个。以相反的方式排序\(降序\)，在排序的列名字之后添加DESC关键字:

```
mysql> select name, birth from pet order by birth desc;
+----------+------------+
| name     | birth      |
+----------+------------+
| Puffball | 1999-03-30 |
| Chirpy   | 1998-09-11 |
| Whistler | 1997-12-09 |
| Slim     | 1996-04-29 |
| Claws    | 1994-03-17 |
| Fluffy   | 1993-02-04 |
| Fang     | 1990-08-27 |
| Bowser   | 1989-08-31 |
| Buffy    | 1989-05-13 |
+----------+------------+
```

你可以在多个列上排序，你可以在多个列上以多不同的方向排序。例如，动物的种类升序，出生日期降序使用下面的查询:

```
mysql> SELECT name, species, birth from pet order by species, birth desc;
+----------+---------+------------+
| name     | species | birth      |
+----------+---------+------------+
| Chirpy   | bird    | 1998-09-11 |
| Whistler | bird    | 1997-12-09 |
| Claws    | cat     | 1994-03-17 |
| Fluffy   | cat     | 1993-02-04 |
| Fang     | dog     | 1990-08-27 |
| Bowser   | dog     | 1989-08-31 |
| Buffy    | dog     | 1989-05-13 |
| Puffball | hamster | 1999-03-30 |
| Slim     | snake   | 1996-04-29 |
+----------+---------+------------+
```

DESC关键自仅适用于在他之前的列名\(birth\)；不会影响到species列的排序顺序。

