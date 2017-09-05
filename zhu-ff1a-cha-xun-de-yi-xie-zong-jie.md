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



