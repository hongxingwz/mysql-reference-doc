# Using User-Defined Variables

你可以雇佣MySQL用户变量来记住结果而不用把他们存储在临时变量中\(查看 Section 9.4, "User-Defined Variables".\)

例如，找出最贵和最低价格的articles你可以这样做：

```
mysql> select @min_price:=min(price), @max_price:=max(price) from shop;
mysql> select * from shop where price=@min_price or price=@max_price;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0003 | D      |  1.25 |
|    0004 | D      | 19.95 |
+---------+--------+-------+
```

> 注：
>
> 以用户变量的形式存储数据库对象的名字例如表名或列名然后再SQL语句中使用他们是可以的；然而，这要求你使用prepared语句，查看Section 13.5 "Prepared SQL Statement Syntax",获取更多的信息



