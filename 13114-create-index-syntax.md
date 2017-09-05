# 13.1.14 CREATE INDEX Syntax

![](/assets/2017-09-05-08-08-01.png)

CREATE INDEX 映射到 ALTER TABLE语句来创建索引。参阅Section 13.1.8 "ALTER TABLE Syntax".CREATE INDEX不能用来创建PRIMARY KEY; 使用ALTER TABLE代替。关于更多的关于索引的信息，参阅 Section 8.3.1, "HOW MySQL Uses Indexes".

通常，你在创建表的同时会创建索引使用CREATE TABLE语句。参阅Section 13.1.18 "CREATE TABLE Syntax"。这个方针对于InnoDB表由其重要，因为primary key 决定了数据文件的物理布局。CREATE INDEX使你可以在已经存在的表中添加索引。

以\(col1, col2, ...\)形式列出的列创建multiple-column。索引的值是指定列的值连接起来。

对于字符串列，可以用该列值的前面部分来创建索引，使用col\_name\(length\)语法来指定一个索引的前缀长度。

* Prefixes 可以用于CHAR, VARCHAR, BINARY 和 VARBINARY 列的索引。
* 对于BLOB和TEXT列的索引一定要指定prefixes
* Prefix的限制以byte为单位，而CREATE TABLE, ALTER TABLE和CREATE INDEX语句在对于非二进制的字符串类型(CHAR, VARCHAR, TEXT)的前缀长度被解释为字符的数量，对于二进制字符串类型(BINARY, VARBINARY, BLOB)解释为字符的数量.当你在一个非二进制字符串使用multibyte character set指定前缀长度时要考虑到这一点
* 对于spatial列，不可以指定前缀值，我们在稍后的章节

下面的语句展示了创建一个索引使用name列的前10个字符(假设name是一个非二进制类型的字符串)


```
CREATE INDEX part_of_name ON customer(name(10))
```
如果name列中通常前十个字符不同，那么此索引不会比创建全部的名字列的索引慢多少。而，使用列前缀来创建索引可以使索引变得非常小，使你可以节省许多硬盘的空间也可以提升插入操作的速度。

Prefix支持的长度由存储引擎决定的。例如，对于InnoDB表来说前缀可以达到767字节，如果InnoDB表设置了innodb\_large\_prefix则最的索引长度是3072字节。对于MyISAM表，前缀的长度是1000字节。NDB存储引擎不支持前缀(参阅 21.1.6.6章， "Unsupported or Missing Features in NDB Cluster")

一个UNIQUE 索引创建一个限制索引上所有的值都必须是有区别的。如果你尝试插入一个新的记录的值与已经存在的值相匹配，则会发生一个错误。对于所有的引擎，UNIQUE索引允许多个NULL值如果该列允许存储NULL值。如果你为UNIQUE index指定了前缀，前缀内的值一定要是唯一的。

FULLTEXT仅支被InnoDB和MyISAM表的CHAR, VARCHAR, 和TEXT列支持。Indexing always happens over the entire column;列前缀索引不被支持如果指定了任何的前缀也地被忽略。参阅12.9 "Full-Text Search Functions"来获取更详细的操作。




