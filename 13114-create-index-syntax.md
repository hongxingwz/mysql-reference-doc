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





