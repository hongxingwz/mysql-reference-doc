# Using AUTO\_INCREMENT

AUTO\_INCREMENT属性可以用来为新记录生成唯一的标识符：

```
mysql> CREATE TABLE animals(
    -> id mediumint not null auto_increment,
    -> name char(30) not null,
    -> primary key (id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO animals (name) VALUES
    ->     ('dog'),('cat'),('penguin'),
    ->     ('lax'),('whale'),('ostrich');

select * from animals;
```

将会返回：

```
+----+---------+
| id | name    |
+----+---------+
|  1 | dog     |
|  2 | cat     |
|  3 | penguin |
|  4 | lax     |
|  5 | whale   |
|  6 | ostrich |
+----+---------+
```

没有为AUTO\_INCREMENT列指定任何值，因此MySQL自动的赋与自动增长的值。

你也可以显式的赋给该列0值来获取自动增长的值,除非NO\_AUTO\_VALUE\_ON\_ZERO SQL模式被激活。如果该列被声明为NOT NULL,也可以赋值给该列NULL来获得自动增加的值。当你插入任何其他值到AUTO\_INCREMENT列时，自动增长的值会被重置为该列最大的值

你可以提取最近生成的AUTO\_INCREMENT值通过LAST\_INSERT\_ID\(\) SQL函数或 mysql\_insert\_id\(\) C API函数。这些函数都是连接特定的，因此他们的返回值不受其他也执行了插入操作的连接影响。

使用最小的整数数据类型对于AUTO\_INCREMENT列就已经足够。当该列到达数据类型最大的限制时，下次尝试生成自增值会失败。使用UNSIGNED属性如果可以的话来生成更大一点的范围。例如，你使用TINYINT，最大可能到达的值是127。对于TINYINT UNSIGNED,最小值可能是255 查看 Section 11.2.1, “Integer Types \(Exact Value\) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT” 获取各种整形类型的范围

```
mysql> insert into animals values (0, "a");
mysql> select * from animals;
+----+---------+
| id | name    |
+----+---------+
|  1 | dog     |
|  2 | cat     |
|  3 | penguin |
|  4 | lax     |
|  5 | whale   |
|  6 | ostrich |
|  7 | a       |
+----+---------+
```

> 注：
>
> 对于同时插入一行，LAST\_INSERT\_ID\(\)和mysql\_insert\_id\(\)真正返回的AUTO\_INCREMENT值是最先插入的一行。This enables multiple-row inserts to be reproduced correctly on other servers in a replication setup.

让AUTO\_INCREMENT值不是以1开始，用CREATE TABLE或ALTER TABLE语句，像这样

```
mysql> alter table animals AUTO_INCREMENT=8;
```



