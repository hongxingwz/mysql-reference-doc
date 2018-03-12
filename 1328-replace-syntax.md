# 13.2.8 REPLACE 语法

```
REPLACE [LOW_PRIORITY | DELAYED]
    [INTO] tbl_name
    [(col_name [, col_name] ...)]
    {VALUES | VALUE} (value_list) [, (value_list)] ...

REPLACE [LOW_PRIORITY | DELAYED]
    [INTO] tbl_name
    SET assignment_list

REPLACE [LOW_PRIORITY | DELAYED]
    [INTO] tbl_name
    [(col_name [, col_name] ...)]
    SELECT ...

value:
    {expr | DEFAULT}

value_list:
    value [, value] ...

assignment:
    col_name = value

assignment_list:
    assignment [, assignment] ...
```
REPLACE工作方式和INSERT很像，除了如果在插入一条新记录时，一个主键或唯一索引的值与老记录相同，在新记录插入之前会把老的记录给删掉。参阅 13.2.5 "INSERT 语法"。

REPLACE是MySQL对SQL标准的拓展。他既是插入，或者是删除插入。另名MYSQL对标准SQL的拓展 -- 既是插入或者是更新 -- 查看 13.2.5.2 "INSERT ... ON 
DUPLICATE KEY UPDATE Syntax"。

DELAYED 插入和替换在MySQL5.6中被废弃。在MySQL5.7中，DELAYED不在被支持。服务会识别但会忽略掉DELAYED关键词，把其处理为非延时的替换，并且生成一个ER\_WARN\_LEGACY\_SYNTAX\_CONVERTED警告。("REPLACE DELAYED is no longer supported. The statement was converted to REPLACE.")DELAYED 关键字在将来的正式版本中会被移除。

>注 当一个表中具有主键或唯一索引时REPLACE才有意义。否则，其会与INSERT等价，因为没有索引用来发现一个新记录是否与其它记录重复了。


列的值是REPLACE语句指定的值，任何缺失的列被设置为其默认值，就像INSERT一样，你不可以从当前行引用值，并在新行中使用。如果你使用了像`SET col_name = col_name + 1`这样的赋值语句时，在右侧引用的列名被视为`DEFAULT(col_name)`,因此赋值等价于`SET col_name = DEFAULT(col_name) + 1`

要使用`REPLACE`,你必须拥有表的INSERT和DELETE权限。

如果一个生成列被显示替换时，仅允许的值是DEFAULT。关于生成列，参阅"Section 13.1.18.8", "CREATE TABLE and Generate Columns"

REPLACE支持显示的分片选择使用PARTITION关键字后跟以逗号分割的分片，或子分片，或者二者结合。像INSERT一样，如果不能在这些分片上插入一条新记录，REPLACE语句会失败并产生一个错误**Found a row not matching the given partition set.** 关于更多的信息和示例，参阅 "Section 22.5 Partition Selection"

REPLACE语句返回一个数表示显示行的数量。这是删除行和插入行数量的和。如果对于一个单行的REPLACE语句的返回数 1，说明一行被插入没有行被删除。如果数量大于1，一个或多个老的记录被删除然后新的记录被插入。

返回的影响行的数量使其容易决定REPLACE语句是仅添加了一行还是其替换了好多行：检查返回的数量是否是1（添加）或大于1（替换）

如果你使用的是C API，影响的行数可以通过使用mysql\_affected\_rows()函数获取

**You cannot replace into a table and select from the same table in a subquery**

MySQL使用下面的算法用于REPLACE(和LOAD DATA ... REPLACE)：
* 试着将新行插入表中
* 当因为主键或唯一索引重复而插入失败时：
    * 从表中删除具有重复键的冲突的行
    * 尝试在插入新行到表中
    

在遇到重得键错误时，存储引擎可能把REPLACE执行成更新而不是删除加插入，便是评议是相同的。没有对于用户可见的影响除了存储引擎怎么增加`Handler_xxx`状态变量的值。

因为`REPLACE ... SELECT`语句的结果受`SELECT`语句返回的行数的顺序的影响，并且顺序并不一定没次都确保一末端，当从库同步主库的数据时。基于这样的原因，`REPLACE ... SELECT`语句被标识为不安全的复制语句。这样的语句产生一条错误信息当使用statement-based mode and are written to the binary log using the row-based format when using MIXED mode. See also Section 16.2.1.1, "Advantages and Disadvantages of Statement-Based and Row-Based Replication"。


当你把一个不是分片的表改为分片的表，或更改一个已经是分片的表时，你可能考虑要更改表的主键(see Section 22.6.1, "Partitiong Kyes, Primary Keys, and Unique keys")。你应该意识到，如果你这样作了，REPLACE语句的影响可能会改变，仅因为你更改了没有分片表的主键。考虑像下面语句创建的表：


```
CREATE TABLE test (
  id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  data VARCHAR(64) DEFAULT NULL,
  ts TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
);
```
当我们创建这样的表并运行你下面mysql客户端显示的语句时，结果像下面这样


```
mysql> REPLACE INTO test VALUES (1, 'Old', '2014-08-20 18:47:00');
Query OK, 1 row affected (0.04 sec)

mysql> REPLACE INTO test VALUES (1, 'New', '2014-08-20 18:47:42');
Query OK, 2 rows affected (0.04 sec)

mysql> SELECT * FROM test;
+----+------+---------------------+
| id | data | ts                  |
+----+------+---------------------+
|  1 | New  | 2014-08-20 18:47:42 |
+----+------+---------------------+
1 row in set (0.00 sec)
```

现在我们创建第二个表与第一个表大体相同，除了主键包含2列，像下面


```
CREATE TABLE test2 (
  id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  data VARCHAR(64) DEFAULT NULL,
  ts TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id, ts)
);
```
当我们在test2表上运行在test表上运行的两条语句时，我们获得了不同的结果：


```
mysql> REPLACE INTO test2 VALUES (1, 'Old', '2014-08-20 18:47:00');
Query OK, 1 row affected (0.05 sec)

mysql> REPLACE INTO test2 VALUES (1, 'New', '2014-08-20 18:47:42');
Query OK, 1 row affected (0.06 sec)

mysql> SELECT * FROM test2;
+----+------+---------------------+
| id | data | ts                  |
+----+------+---------------------+
|  1 | Old  | 2014-08-20 18:47:00 |
|  1 | New  | 2014-08-20 18:47:42 |
+----+------+---------------------+
2 rows in set (0.00 sec)
```
这是由于，我们在test2运行，id和ts必须全部匹配已存在的行才替换，否则，一条记录会被插入。



A REPLACE statement affecting a partitioned table using a storage engine such as MyISAM that employs table-level locks locks only those partitions containing rows that match the REPLACE statement WHERE clause, as long as none of the table partitioning columns are updated; otherwise the entire table is locked. (For storage engines such as InnoDB that employ row-level locking, no locking of partitions takes place.) For more information, see Section 22.6.4, “Partitioning and Locking”.







