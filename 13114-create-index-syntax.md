# 13.1.14 CREATE INDEX Syntax

![](/assets/2017-09-05-08-08-01.png)

CREATE INDEX 映射到 ALTER TABLE语句来创建索引。参阅Section 13.1.8 "ALTER TABLE Syntax".CREATE INDEX不能用来创建PRIMARY KEY; 使用ALTER TABLE代替。关于更多的关于索引的信息，参阅 Section 8.3.1, "HOW MySQL Uses Indexes".

通常，你在创建表的同时会创建索引使用CREATE TABLE语句。参阅Section 13.1.18 "CREATE TABLE Syntax"。这个方针对于InnoDB表由其重要，因为primary key 决定了数据文件的物理布局。CREATE INDEX使你可以在已经存在的表中添加索引。

以\(col1, col2, ...\)形式列出的列创建multiple-column。索引的值是指定列的值连接起来。

对于字符串列，可以用该列值的前面部分来创建索引，使用col\_name\(length\)语法来指定一个索引的前缀长度。

* Prefixes 可以用于CHAR, VARCHAR, BINARY 和 VARBINARY 列的索引。
* 对于BLOB和TEXT列的索引一定要指定prefixes
* Prefix的限制以byte为单位，而CREATE TABLE, ALTER TABLE和CREATE INDEX语句在对于非二进制的字符串类型\(CHAR, VARCHAR, TEXT\)的前缀长度被解释为字符的数量，对于二进制字符串类型\(BINARY, VARBINARY, BLOB\)解释为字符的数量.当你在一个非二进制字符串使用multibyte character set指定前缀长度时要考虑到这一点
* 对于spatial列，不可以指定前缀值，我们在稍后的章节

下面的语句展示了创建一个索引使用name列的前10个字符\(假设name是一个非二进制类型的字符串\)

```
CREATE INDEX part_of_name ON customer(name(10))
```

如果name列中通常前十个字符不同，那么此索引不会比创建全部的名字列的索引慢多少。而，使用列前缀来创建索引可以使索引变得非常小，使你可以节省许多硬盘的空间也可以提升插入操作的速度。

Prefix支持的长度由存储引擎决定的。例如，对于InnoDB表来说前缀可以达到767字节，如果InnoDB表设置了innodb\_large\_prefix则最的索引长度是3072字节。对于MyISAM表，前缀的长度是1000字节。NDB存储引擎不支持前缀\(参阅 21.1.6.6章， "Unsupported or Missing Features in NDB Cluster"\)

一个UNIQUE 索引创建一个限制索引上所有的值都必须是有区别的。如果你尝试插入一个新的记录的值与已经存在的值相匹配，则会发生一个错误。对于所有的引擎，UNIQUE索引允许多个NULL值如果该列允许存储NULL值。如果你为UNIQUE index指定了前缀，前缀内的值一定要是唯一的。

FULLTEXT仅支被InnoDB和MyISAM表的CHAR, VARCHAR, 和TEXT列支持。Indexing always happens over the entire column;列前缀索引不被支持如果指定了任何的前缀也地被忽略。参阅12.9 "Full-Text Search Functions"来获取更详细的操作。

MyISAM, InnoDB, NDB和ARCHIVE存储引擎支持spatial列例如\(POINT和GEOMETRY\)\(Section 11.5, "Extensions for Spatial Data", 描述了空间类型\)然而，各在各引擎之间对空间列索引的支持确不相同。空间和非空间的索引适用于下列的规则  
Spatial索引\(创建索引时使用SPATIAL INDEX\)有如下的特性

* 仅适用于MyISAM和InnoDB表。指定SPATIAL INDEX用于别的引擎造成一个错误
* 索引列一要是NOT NULL
* 列前缀是禁止的。每列的全部宽被索引

nonspatial索引的特性\(用INDEX, UNIQUE, PRIMARY KEY创建的\)

* 允许支持spatial列除了ARCHIVER的所有引擎
* 列可以为NULL除非索引是primary key
* 对于一个在non-SPATIAL索引列的spatial列除了POINT列，列前缀的长度一定要指定。\(这对于BLOB列的索引有着相同的需求\)。前缀的长度以byte为单位
* 对于一个non-SPATIAL索引的类型取决于存储引擎。目前用B-tree。
* 你仅可以在InnoDB, MyISAM，MEMORY表在一个可以允许有NULL值有列添加索引
* 你可以在BLOB或TEXT列中添加一个索引仅在使用InnoDB或MyISAM表。
* 当激活innodb\_stats\_persistent设置被激活时,对于InnoDB表，在这个表上创建索引后要运行ANALYZE TABLE语句。

InnoDB支持第二个索引在虚拟列上。更多的信息， 参阅13.1.18.9 "Secondary Indexes and Generated Columns"

index\_col\_name说明可以以ASC或DESC结尾。这两个关键字用以未来的扩展指定升序或降序排序，目前他们被解析但被忽略；索引的值一直以升序存储。

在索引列表后，索引的选项可以被指定。一个index\_option可以是下面的任何值：

* KEY\_BLOCK\_SIZE \[=\] value
  对于MyISAM表，KEY\_BLOCK\_SIZE以字节为单位指定了index key块的大小。该值被视为提示；如果必要的话可以使用不用的size。KEY\_BLOCK\_SIZE值指定用于独立的索引定义覆盖表级别的KEY\_BLOCK\_SIZE值。
  
  KEY\_BLOCK\_SIZE在InnoDB表中不支持索引级别的定义。参阅Section 13.1.18 "创建TABLE语法"
* index\_type
一些存储引擎允许你指定索引类型当创建索引时。例如：


```
CREATE TABLE lookup (id INT) ENGINE = MEMORY;
CREATE INDEX id\_index ON lookup (id) USING BTREE;

```
Table 13.1, "Index Types Per Storage Engine"展示了不同存储引擎支持的索引类型。当多个索引类型被列出时，使用第一个默认的索引类型当没有索引类型被指定时。存储引擎没有在表列出时不支持index\_type在索引定义语句

Table 13.1 Index Types Per Storage Engine

|Storage Engine| Permissible Index Types|
|---|---|
|InnoDB| BTREE|
|MyISAM| BTREE|
|MEMORY/HEAP| HASH, BTREE|
|NDB| HASH, BTREE(see note in text)|

index\_type语句不能用于FULLTEXT, INDEX 或 SPATIAL INDEX说明书。Full-text index依赖于不同的存储引擎的实现。Spatial索引由R-tree索引实现

BTREE indexes are implemented by the NDB storage engine as T-Tree indexes.

> 注
For indexes on NDB table columns, the USING option can be specified only for a unique index or primary. USING HASH prevents the creation of an ordered index; otherwise creating a unique index or primary key on an NDB table automatically results in the creation of both an ordered index and a hash index, each of which indexes the same set of columns.

> For unique indexes that include one or more NULL columns of an NDB table, the hash index can be used only to look up literal values, which means that IS [NOT] NULL conditions require a full scan of the table. One workaround is to make sure that a unique index using one or more NULL columns on such a table is always created in such a way that it includes the ordered index; that is, avoid employing USING HASH when creating the index.

If you specify an index type that is not valid for a given storage engine, but another index type is available that the engine can use without affecting query results, the engine uses the available type. The parser recognizes RTREE as a type name,The parser recognizes BTREE as a type name, but currently this cannot be specified for any storage engine. 

> Use of the index\_type option before the ON tbl_name is deprecated; support for use of the option in this position will be removed in a future MySQL release. If an index\_type option is given in both the earlier and later positions, the final option applies.

TYPE type\_name 被视为USING type\_name的同义词。然而，USING是首先的形式。

对于存储引擎对index\_type选项的支持，Table 13.2 "Storage Engine Index Characteristics"展示了索引使用的特性。


Table note:

1. 如果使用了USING HASH会阻止隐式的顺序的索引的创建。


* WITH PARSER parser_name
此选项只能用于FULLTEXT索引。如果是全文索引他把特殊的解析插件与索引相关联并对于搜索操作需要特殊的处理。InnoDB和MyISAM支持全文索引插件。参阅Full-Text Parser Plugins and Section 28.2.4.4, "Writing Full-Text Parser Plugins"获取更多的细节。

* COMMENT 'string'
索引的定义可以包括可选的注释，其最大存储为1024字符


