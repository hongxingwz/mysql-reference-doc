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

如果你把一个值9赋予此列，其二进制值是1001，因此第1和第4个SET值‘a'和'd'被选择然后回返的结果是'a,d'.

