# 总：查看mysql数据库及表编码格式

## 查看数据库编码格式

```
mysql> show variables like 'character_set_database'
```

## 查看数据表的编码格式

```
mysql> show create table <表名>;
```

## 创建数据库时指定数据库的字符集

```
mysql> create database <数据库名> character set utf8;
```

## 创建数据表时指定数据表的编码格式

```
create table tb_books (
    name varchar(45) not null,
    price double not null,
    bookCount int not null,
    author varchar(45) not null ) default charset = utf8;
```

## 修改数据库的编码格式

```
mysql>alter database <数据库名> character set utf8;
```

## 修改数据表格编码格式

如果用户想改变表的默认字符集和所有的字符列的字符集到一个新的字符集，使用下面的语句：
```
mysql> alter table tbl_name CONVERTO TO CHARACTER SET <charset_name>
```
>警告：上述操作是在字符集中转换列值。

如果用户在字符集(如gb2312)中有一个列，便存储的值使用的是其它的一些不兼容的字符集(如utf8)，那么该操作将不会得到用户期望的结果。在这种情况下，用户必须对每一列做如下操作：


仅仅改变一个表的缺省字符集，可以使用下面的语句
```
mysql>alter table <表名> character set utf8;
```




## 修改字段编码格式

```
mysql>alter table <表名> change <字段名> <字段名> <类型> character set utf8;

mysql>alter table user change username username varchar(20) character set utf8 not null;
```

## 添加外键

```
mysql>alter table tb_product add constraint fk_1 foreign key(factoryid) references tb_factory(factoryid);

mysql>alter table <表名> add constraint <外键名> foreign key<字段名> REFERENCES <外表表名><字段名>;
```

## 删除外键

```
mysql>alter table tb_people drop foreign key fk_1;
mysql>alter table <表名> drop foreign key <外键名>;
```



