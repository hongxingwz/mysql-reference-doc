# 3.3.4.9 Using More Than one Table

pet表保持对你有哪些宠物的追踪。如果你想记录别的信息，例如他们一生中访问了多少次的兽医或他们的小孩什么时候出生，你需要别外的表。这样的表看起来像什么样呢？它需要包含下面的信息:

* 宠物的名字因此你知道此事件与哪个名字相关
* 一个日期因此你知道事件什么时候发生
* 一个字段来描述事件
* 事件类型字段，如果你想对事件分类。

给定了这些思考，event表格的CREATE TABLE语句看起来应该像这样:

```
mysql> CREATE TABLE event (name varchar(20), date DATE, type varchar(15), remark varchar(255));
```

像pet表一样，可以很容易的加载初始数据通过创建一个由tab分割的文本文件包含下面的信息：

| name | date | type | remark |
| --- | --- | --- | --- |
| Fluffy | 1995-05-15 | litter | 4 kittens, 3 female, 1 male |
| Buffy | 1993-06-23 | litter | 5    puppies, 2 female, 3 male |
| Buffy | 1994-06-19 | litter | 3 puppies, 3 female |
| Chirpy | 1999-03-21 | vet | needed beak straightened |
| Slim | 1997-08-03 | vet | broken rib |
| Bowser | 1991-10-12 | kennel |  |
| Fang | 1991-10-12 | kennel |  |
| Fang | 1998-08-28 | birthday | Gave him a new chew toy |
| Claws | 1998-03-17 | birthday | Gave him a new flea collar |
| Whistler | 1998-12-09 | birthday | First birthday |

像这样加载记录:

```
mysql> LOAD DATA LOCAL INFILE 'event.txt' INTO TABLE event;
```

基于你已经在pet表中运行和学习的查询，你应该可以在event表中检索记录；原则是一样的。但仅使用event表不能够充分的回答你要寻问的问题？

假定你想找出宠物在多大的时候拥有了小孩。我们在之前知道怎么计算年龄在两个日期之间。小孩的出生日期在event表，但计算出他的年龄你需要知道他的出生日期，在pet表中存储。这竟味着你需要查询两个表:

```
mysql> select pet.name, timestampdiff(year, birth, date) as age,
    -> remark
    -> from pet inner join event on pet.name = event.name where event.type = 'litter'
    -> ;
+--------+------+-----------------------------+
| name   | age  | remark                      |
+--------+------+-----------------------------+
| Fluffy |    2 | 4 kittens, 3 female, 1 male |
| Buffy  |    4 | 5 puppies, 2 female, 3 male |
| Buffy  |    5 | 3 puppies, 3 female         |
+--------+------+-----------------------------+
```

这有几点需要注意的事情在查询中:

* FROM语句连接了两个表因为查询需要从这两张表中拉取信息
* 当将多个表结合时，你需要指定一个表中的记录怎么才能与其他表中的记录匹配。这很容易因为他们都有一个name属性



