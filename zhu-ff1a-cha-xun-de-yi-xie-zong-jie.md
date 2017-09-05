# 查询的一些总结

* 查询最贵的商品

建表语句

```mysql
create table if not exists goods(
good_id bigint not null primary key auto_increment,
good_name varchar(30) not null,
good_price decimal(10, 2) not null default -1.0
)engine InnoDB, charset=utf8;

insert into goods (good_name, good_price) values ("a", 11.11), ("b", 11.11),("a", 1.11),("a", 2.11),("a", 3.11);

select * from goods;
+---------+-----------+------------+
| good_id | good_name | good_price |
+---------+-----------+------------+
|       1 | a         |      11.11 |
|       2 | b         |      11.11 |
|       3 | a         |       1.11 |
|       4 | a         |       2.11 |
|       5 | a         |       3.11 |
+---------+-----------+------------+
```

```mysql
#解决方案1
select a.*  from goods as a left join goods as b  on a.good_price < b.good_price where b.good_id is  null;

#解决方案2
select a.* from goods as a where a.good_price = (select max(good_price) from goods );
+---------+-----------+------------+
| good_id | good_name | good_price |
+---------+-----------+------------+
|       1 | a         |      11.11 |
|       2 | b         |      11.11 |
+---------+-----------+------------+
```

* 查询出前三贵的商品

```
#我的第一种解决方案
select a.* from goods as a where a.good_price in (select distinct good_price from goods  order by good_price limit 3);
#但是在执行之后报了如下的错误，也就是说MySQL的这个版本不支持子查询出现LIMIST & IN/ALL/ANY/SOME 语句
#Error Code: 1235. This version of MySQL doesn't yet support 'LIMIT & IN/ALL/ANY/SOME subquery'

#我的第二种解决方案
mysql> select a.* from goods as a  inner join (select distinct good_price from goods  order by good_price desc limit 3) as b on a.good_price = b.good_price;
+---------+-----------+------------+
| good_id | good_name | good_price |
+---------+-----------+------------+
|       1 | a         |      11.11 |
|       2 | b         |      11.11 |
|       4 | a         |       2.11 |
|       5 | a         |       3.11 |
+---------+-----------+------------+
```

* 查询出总成绩前三名\(含并列\)

建表

```
create table if not exists student_score(
	id bigint   auto_increment primary key,
    name varchar(255),
    lesson varchar(255),
    mark  tinyint unsigned
);

insert into student_score (name, lesson, mark) values
("john", "Math", 59),
("john", "History", 56),
("Mike", "Eng", 51),
("Mike", "Math", 59),
("Mike", "History", 55),
("Mark", "Eng", 71),
("Mark", "Math", 89),
("Mark", "History", 95),
("mm", "Eng", 61),
("mm", "Math", 79),
("mm", "History", 85),
("f", "Eng", 51),
("f", "History", 95),
("na", "Math", 79),
("na", "Eng", 61),
("na", "History", 85);

mysql> select * from student_score;
+----+------+---------+------+
| id | name | lesson  | mark |
+----+------+---------+------+
|  1 | john | Math    |   59 |
|  2 | john | History |   56 |
|  3 | Mike | Eng     |   51 |
|  4 | Mike | Math    |   59 |
|  5 | Mike | History |   55 |
|  6 | Mark | Eng     |   71 |
|  7 | Mark | Math    |   89 |
|  8 | Mark | History |   95 |
|  9 | mm   | Eng     |   61 |
| 10 | mm   | Math    |   79 |
| 11 | mm   | History |   85 |
| 12 | f    | Eng     |   51 |
| 13 | f    | History |   95 |
| 14 | na   | Math    |   79 |
| 15 | na   | Eng     |   61 |
| 16 | na   | History |   85 |
+----+------+---------+------+
16 rows in set (0.00 sec)

#思路
#首先查询前3的总成绩(含并列)
mysql> select distinct sum(mark) as total_mark from student_score group by name order by total_mark desc limit 3;
+------------+
| total_mark |
+------------+
|        255 |
|        225 |
|        165 |
+------------+

# 查询出每个人的总成绩
mysql> select * from (select sum(mark) as total_mark , name  from student_score as a group by name) as b;
+------------+------+
| total_mark | name |
+------------+------+
|        146 | f    |
|        115 | john |
|        255 | Mark |
|        165 | Mike |
|        225 | mm   |
|        225 | na   |
+------------+------+

# 内连接查询
mysql> select b.name, b.total_mark from (select sum(mark) as total_mark , name  from student_score as a group by name) as b inner join  (select distinct sum(mark) as total_mark from student_score group by  name order by total_mark desc limit 3) as c on b.total_mark = c.total_mark;
+------+------------+
| name | total_mark |
+------+------------+
| Mark |        255 |
| mm   |        225 |
| na   |        225 |
| Mike |        165 |
+------+------------+

```




