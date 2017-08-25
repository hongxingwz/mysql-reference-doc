# The Row Holding the Maximum of a Certain Column

任务：找出最贵商品的number, dealer, 和 price。

用一个子查询很容易的作到了:

```
mysql> select article, dealer, price from shop 
    -> where price = (select max(price) from shop);
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
```

其他的解决方案是使用LEFT JOIN或以price降序排列所有行，使用MySQL特有的语句LIMIT仅获取第一行就可以了：

```
mysql> SELECT s1.article, s1.dealer, s1.price from shop s1 
     > left join shop s2 on s1.price < s2.price where s2.price is null;
```

```
mysql> select article, dealer, price from shop
    -> order by price desc limit 1;
+---------+--------+-------+
| article | dealer | price |
+---------+--------+-------+
|    0004 | D      | 19.95 |
+---------+--------+-------+
1 row in set (0.00 sec)
```

> 注
>
> 如果有好多个最贵的商品，都是19.95，LIMIT的解决办法只会取出他们其中的一个



