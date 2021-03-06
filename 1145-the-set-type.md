# 11.4.5 The SET Type

一个SET是一个字符对象可以有0个多个值，每个值从可以从当表创建时允许的指定的value值中选择一个。SET列的值由多个set成员由指定的成员通过","分割。作为结果就是SET成员值本身应该不包含","号。

例如，一个指定为SET\("one", 'two'\) NOT NULL的值可以具有下面的任何值:

```
''
'one'
'two'
'one, two'
```

一个SET列的最大成员可以最大具有64个不同的成员。一个表应该不超过255个唯一的元素定义在ENUM和SET被作为一组时。获取更多关于限制的信息，参阅C.10.5 "Limits Imposed by .frm File Structure"

重得的值在定义中造成一个错误，如果是严格模式将产生一个错误。

当一个表创建时在表中定义的SET成员的尾部的空格将自动的删除。

在提取时，存储在SET列中的值将以列定义中的lettercase展示。注解SET列可以被赋予字符集和检验集。对于二进制或case-senstive检验集，要考虑到单词的大小写当赋值给该列时。

MySQL用数字存储SET值，with the low-order bit of the stored value corresponding to the first set member.如果在数字环境下提取一个SET值，the value retrieved has bits set corresponding to the set member that make up the column value. 例如，你可以从SET列中像这样提取数字:

```
mysql> SELECT set_col+0 FROM tbl_name;
```

如果一个数值被存储进SET列，the bits that are set in the binary representation of the number determine the set members in the column value.对于一个指定为SET\('a', 'b', 'c', 'd'\)列，该成员具有下面的数字和二进制值

| SET Member | Decimal Value | Binary Value |
| :--- | :--- | :--- |
| 'a' | 1 | 0001 |
| 'b' | 2 | 0010 |
| 'c' | 4 | 0100 |
| 'd' | 8 | 1000 |

如果你把一个值9赋予此列，其二进制值是1001，因此第1和第4个SET值‘a'和'd'被选择然后回返的结果是'a,d'。

对于一个包含超过一个SET元素的值，无论在你插入时是以任何顺序，无论值在list中出现了多少次。当你最后提取值的时候，每个值仅出现一次，元素出现的顺序是根据创建表时指定的顺序出现，假定，列被指定为SET\('a','b','c','d'\):

```
mysql> create table myset (col set('a', 'b', 'c', 'd'));
Query OK, 0 rows affected (0.02 sec)
```

如果你插入值'a,d', 'd,a', 'a,d,d', 'a,d,a' 和 ’d,a,d':

```
mysql> insert into myset (col) values 
    -> ('a,d'), ('d,a'), ('a,d,a'), ('a,d,d'), ('d,a,d');
Query OK, 5 rows affected (0.00 sec)
Records: 5  Duplicates: 0  Warnings: 0
```

所有值作为'a,d'出现当提取的时候

```
mysql> select * from myset;
+------+
| col  |
+------+
| a,d  |
| a,d  |
| a,d  |
| a,d  |
| a,d  |
+------+
5 rows in set (0.00 sec)
```

如果你设置了一个SET列不支持的值，value值会被忽略然后产生一个警告：

```
mysql> insert into myset(col) values ('a,d,d,s');
Query OK, 1 row affected, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+------------------------------------------+
| Level   | Code | Message                                  |
+---------+------+------------------------------------------+
| Warning | 1265 | Data truncated for column 'col' at row 1 |
+---------+------+------------------------------------------+

mysql> select col from myset;
+------+
| col  |
+------+
| a,d  |
| a,d  |
| a,d  |
| a,d  |
+------+
```

如果严格模式启用了，尝试插入无效的结果值将产生一个错误。

SET值以数值排序。NULL值排在non-NULL的SET值前面。

SUM\(\)或AVG\(\)函数期待一个数值参数并把参数转型为数值如果必须的话。对于SET值，cast操作可以把set值用于数字。

通常情况下，你使用FIND\_IN\_SET\(\)或LIKE操作来搜索SET里面的值

```
mysql> SELECT * FROM tbl_name WHERE FIND_IN_SET('value', set_col) > 0;
mysql> SELECT * FROM tbl_name WHERE set_col LIKE '%value%';
```

第一个语句查找set\_col包含value set成员。第二个也一样，但不全部一样：他查找set\_col列的任何地方包含value，即使是其他set成员的子串

下面的语句也是允许的：


```
mysql> SELECT * FROM tbl_name WHERE set_col & 1;
mysql> SELECT * FROM tbl_name WHERE set_col = 'va1l, val2'
```
第一个语句查找包含第一个set成员的行。第二个查找精确的匹配。小心的使用第二种类型。比较‘val1, val2’跟'val2, val1'相比返回不同的结果。你应该指定与列定义时相同的顺序的值。

从一个列中发现所有可能的值，使用SHOW COLUMNS FROM tbl\_name LIKE set\_col 解析SET的定义并在Type列输出。
在C api中，set的值以字符串返回。获取区别原始字符串和集合字符串，参阅27.8.5 "C API Data Structures"。



