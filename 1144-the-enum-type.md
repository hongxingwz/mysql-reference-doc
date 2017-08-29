# 11.4.4 The ENUM Type

一个ENUM是一个在创建表时显示的在列说明中枚举的一系列允许的值中的一个的字符对象。他具有如下的优点：

* 在位置上存储有限的值，该列对可能存储的值有限制
* 重复的读取和查询。数据会被翻译成相对应的数字在查询结果中

下面这些专业的问题执得考虑:

* 如果你把枚举值弄得看起来像数字，很容易把字面量值和他们的内置的索引值混合，在Enumeration Limitations中有解释
* 在枚举列中使用ORDER BY需要额外的小心，在Enumeration Sorting
* Creating and Using ENUM Columns
* Index Values for Enumeration Literals
* Handling of Enumeration Literals
* Empty or Null Enumeration Values
* Enumeration Sorting
* Enumeration Limitations

## Creating and Using ENUM Columns

枚举值必须是由引号包裹的字符串字面量。例如，你可以创建一个表具有ENUM列，像下面这样：

```sql
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

## Index Values for Enumeration Literals

每个枚举值具有一个索引：

* 在列说明列出的元素会被赋予索引，以1开始。
* 空字符串的索引值是0。这表示你可以使用下面的语句SELECT语句来找出哪些行被赋予了无效的ENUM值

```
mysql> SELECT * FROM tbl_name WHERE enum_col =0
```

* NULL值的索引为NULL
* 这里的“索引”一词是指枚举值列表中的一个位置。这与表索引无关。

例如，一个列指定为ENUM\('Mercury', 'Venus', 'Earth'\)可以具有下面展示的任何值。每个值的索引也展示在下面。

| Value | Index |
| :--- | :--- |
| NULL | NULL |
| ' ' | 0 |
| 'Mercury' | 1 |
| 'Venus' | 2 |
| 'Earth' | 3 |

一个枚举列可能具有65536个区别的元素\(实际的限制是少于3000\)。一个表可以有不超过255独特的元素列表定义中的枚举和set列。获取更多的信息对于这些限制，参阅C.10.5章， “Limits Imposed by .frm File Structure”

如果你在数值上下文中提取一个枚举值，返回列值的索引。列如，你可以提取数值值从ENUM列像这样：

```
mysql> SELECT enum_col+0 FROM tbl_name;
```

SUM\(\)或AVG\(\)函数期待一个数值参数并把参数转换一个数值如果可以的话。对于ENUM值，索引值用于计算。

## Handling of Enumeration Literals

尾部的空格会被自动的删除从ENUM成员中在表定义当一个表被创建时

    mysql> alter table shirts add column size2 enum('a ', 'b ', 'c   ');
    mysql> show create table shirts \G;
    *************************** 1. row ***************************
           Table: shirts
    Create Table: CREATE TABLE `shirts` (
      `name` varchar(40) DEFAULT NULL,
      `size` enum('x-small\n','small','medium','large','x-large') DEFAULT NULL,
      `size2` enum('a','b','c') DEFAULT NULL
    ) ENGINE=InnoDB DEFAULT CHARSET=latin1
    1 row in set (0.00 sec)

当提取时，在ENUM存储的值使用列定义的lettercase展示。注意ENUM列可以被赋予字符集和校验集。对于二进制或case-sensitive检验集，当给一列赋值时会考虑到lettercase

如果你存储一个数值到枚举列，数值会被看成可能值的索引，然后值被看作成具有该索引的枚举成员的值，然后值被以枚举成员的索引存储\(然而，此不与LOAD DATA工作，会把所有的输入看成字符串\)如果数字被引号包裹，它仍被解释成索引，如果没有匹配的字符串在枚举的列表中。出于这些原因，不能定义一个枚举列并且他的值看起来像数字，因为这很容易使人变得困惑。例如，下面的列具有枚举成员'0', '1', '2‘，但数字的索引确是1,2,3

```sql
numbers ENUM('0', '1', '2')
```

如果你存储2,他被解释为索引值，然后变成’1‘\(具有索引2的值\)。如果你存储'2'，他们匹配到枚举值，因此被存储为'2'。如果你存储'3'，它不会匹配任何枚举值，因此它把其看作索引然后变成了'2'\(具有索引3的值\)

```sql
mysql> create table if not exists shirts2 ( numbers enum('0','1','2') )charset=utf8;
Query OK, 0 rows affected (0.02 sec)

mysql> select * from shirts2;
Empty set (0.00 sec)

mysql> insert into shirts2 (numbers) values (2), ('2'), ('3');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select * from shirts2;
+---------+
| numbers |
+---------+
| 1       |
| 2       |
| 2       |
+---------+
```

决定所有的可能性从一列值中，使用SHOW COLUMNS FROM tbl\_name LIKE 'enum\_col'在输出的Type列解析ENUM的定义

## Empty or NULL Enumeration Values

枚举值也可以是空字符串\(''\)或NULL在必要的情况下：

* 如果你插入一个无效的值到ENUM列\(字符串没有出现在允许值的列表中\)，空字符串被插入而不是出现一个指定的错误。这个字符串可以通过索引来与“普通”的字符串区分开因为在实际上这个字符串具有数值为0的索引。参阅“Index Values for Enumeration Literals”来获取关于枚举值更详细的信息。

在严格SQL模式下，尝试插入一个无效的ENUM值会返回一个错误。

* 如果ENUM列被声明为允许为NULL，NULL值对于该列就是有效的值，并且默认值为NULL。如果ENUM列被声明为NOT NULL，默认值的是允许值的第一个。

## Enumeration Sorting

**ENUM**值的存储基于他们的索引值，索引值取决于他们在枚举成员列表中的顺序在列定义。例如,排序'b'在’a'之前对于ENUM\('b', 'a'\)。空字符串在非空字符串之前，NULL值在所有枚举成员之前。

阻止意外的结果当使用ORDER BY语句在ENUM列时，你要下面的技巧：

* 指定ENUM列以字母的顺序排序
* 确保以词法上排序而不是以索引值排序通过ORDER BY CAST\(col AS CHAR\) 或 ORDER BY CONCAT\(col\)

## Enumeration Limitations

对于枚举值不能是表达式，即使是执行之后是一个字符串值。  
例如，这个创建表CREATE TABLE语句不会工作因为CONCAT函数不能用来构造一个枚举值：

```
mysql> CREATE TABLE sizes(
    -> size ENUM('small', CONCAT('med', 'ium'), 'large')
    -> );
```

你也不可以使用用户变量作为枚举值。下面的语句也不会工作：

```
mysql> create table sizes(
    -> size enum('small', @mysize, 'large')
    -> );
```

我们强烈建议你不要使用数值作为枚举值，因为他不会存储超过appropriate TINYINT 或 SMALLINT类型，并且容易把字符串与索引混合起来如果你不正确的包裹ENUM值。

如果你使用一个数字作为枚举值，永远把值用字符串括起来。如果括号缺失了，数值会作为索引。参阅"Handling of Enumeration Literals"来查看用引号括起来的数字可能被错误的当数值索引来使用。

在定义中重复的值会生成一个警告，如果是严格SQL模式启用的话会生成一个错误。

