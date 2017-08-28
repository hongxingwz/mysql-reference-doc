# 11.4.4 The ENUM Type
一个ENUM是一个在创建表时显示的在列说明中枚举的一系列允许的值中的一个的字符对象。他具有如下的优点：

## Creating and Using ENUM Columns
枚举值必须是由引号包裹的字符串字面量。例如，你可以创建一个表具有ENUM列，像下面这样：


```
mysql> create table shirts(
    -> name varchar(40),
    -> size enum('x-small
    '> ', 'small', 'medium', 'large', 'x-large'));
mysql> insert into shirts (name, size) values ('dress shirt', 'large'), ('t-shirt', 'medium'), ('polo shirt', 'small');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select name, size from shirts;
+-------------+--------+
| name        | size   |
+-------------+--------+
| dress shirt | large  |
| t-shirt     | medium |
| polo shirt  | small  |
+-------------+--------+

```
往表中插入1亿行具有'medium'值的数据将需要1亿字节的存储空间，而相反的是如果你存储真正的字符串"medium"在VARCHAR列中将需要6亿字节。

