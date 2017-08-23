# Working with NULL Values

当你执到使用NULL值会感到相当惊讶，NULL表示"缺失的未知值"然后对待该值时与其他值有点不同。

检测NULL，使用IS NULL和IS NOT NULL操作符，像下面展示的:

```
mysql> select 1 is null, 1 is not null;
+-----------+---------------+
| 1 is null | 1 is not null |
+-----------+---------------+
|         0 |             1 |
+-----------+---------------+
```

你不可以使用算术运行符例如=, &lt;, &lt;&gt;来检测NULL。自己示范一下，试着下面的查询:

```
mysql> SELECT 1 = NULL, 1<>NULL, 1 < NULL, 1 > NULL;
+----------+---------+----------+----------+
| 1 = NULL | 1<>NULL | 1 < NULL | 1 > NULL |
+----------+---------+----------+----------+
|     NULL |    NULL |     NULL |     NULL |
+----------+---------+----------+----------+
```

因为任何与NULL的比较结果都为NULL，你不能获得任何有意义的结果从这样的比较中。

在MySQL， 0或NULL代表false，任何其他值代表true。从布尔操作符返回的默认true值是1。

为什么要特别的对待NULL值呢，在前面的章节中，使用death IS NOT NULL而不是death &lt;&gt; NULL来决定哪个动物已经不在活着是必须的。

在GROUP BY语句中两个NULL值被视作相等的

当在ORDER BY，NULL值出现在第一个如果你作ORDER BY ... ASC ,NULL值会出现在最后一个如果你作ORDER BY ... DESC。

通常的错误是当你与NULL一起工作时通常会假设不能向一个定义为NOT NULL的列里插入一个0或空字符串，但这不是问题。这是真实的值，而NULL表示”不具有值“，你可以很容易使用IS \[NOT\] NULL检测他们

```
mysql> select 0 is null, 0 is not null, '' is null, '' is not null;
+-----------+---------------+------------+----------------+
| 0 is null | 0 is not null | '' is null | '' is not null |
+-----------+---------------+------------+----------------+
|         0 |             1 |          0 |              1 |
+-----------+---------------+------------+----------------+
```

向一个定义为NOT NULL列中插入0或空字符串是完全可以的，因为他们在事实上是NOT NULL。参阅Section B 5.4.3, ”Problems with NULL Values“.

