# Maximum of Column per Group

任务：找出每个article下最贵的价格

```
mysql> select article, max(price) as price
    -> from shop
    -> group by article;
+---------+-------+
| article | price |
+---------+-------+
|    0001 |  3.99 |
|    0002 | 10.99 |
|    0003 |  1.69 |
|    0004 | 19.95 |
+---------+-------+
```



