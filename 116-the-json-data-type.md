# The JSON Data Type

* Creating JSON Values
* Normalization, Merging, and Autowrapping of JSON Value
* Searching and Modifying JSON Values
* Comparison and Ordering of JSON values
* Converting between JSON and non-JSON values
* Aggregation of JSON Values

在MySQL 5.7.8 ，MySQL支持原生的JSON数据类型根据[RFC 7159](https://tools.ietf.org/html/rfc7159) 这样可以有效的访问JSON\(JavaScript Object Notation\)文档中的数据。JSON数据类型提供了以下的优势比在存储JSON格式的字符串到字符列：

* 自动校验存储在JSON文档的列。无效的文档产生错误
* 优化存储格式。JSON文档存储在JSON列被转换成内置的格式允许你快速的读取文档内容。服务器稍后必须读取以二进制格式存储的JSON值，不需要从文本表示中解析值。这种二进制的结构可以使服务直接的查询子对象或嵌套的值通过键或数组索引，而不用读取在文档中该值前或后的所有值

## Creating JSON Values

JSON数组包含一些由逗号分割的，封闭在‘\[’, '\]'之间

```
["abc", 10, null, true, false]
```

JSON对象包含一系列的键值对并由逗号分割，封闭在{和}之间

```
{"k1":"value", "k2": 10}
```

你上面举例展示的，JSON数组和对象可以包含多种值如\(字符串，数字，null字面量，JSON布尔值 true或false字面量\)。在JSON对象的键一定要是字符串。时间\(date, time,或datetime\)标题也是允许的：

```
["12:18:29.000000", "2015-07-29 12:18:29.000000"]
```

嵌套JSON数组和JSON对象也是允许的

```
[99, {"id":"HK500", "cost": 75.99}, ["hot", "cold"]]
{"k1":"value", "k2":[10, 20]}
```

你可以获得JSON值从一系列Mysql提供的函数中获取JSON值\(参阅 12.16.2 "Functions That Create JSON Values"\)或把别的类型的值转换为JSON类型使用CAST\(value AS JSON\)\(参阅 Converting between JSON and non-JSON values\)。下面的几段描述了MySQL怎么处理JSON值由输入提供的

在MySQL中，JSON值被写成字符串。MySQL会把任何字符串转换为JSON在需要JSON值的上下文下，如果字符串对于JSON是无效的则会产生一个错误。这些上下文包括在JSON类型列中插入值 和 给期待JSON值的函数值中传递参数\(在Mysql JSON函数的文档中通常展示为json\_doc或json\_val\)，像下面例子操作示范的：

* 尝试向一个JSON列插入值，如果值是有效的JSON值则会成功，如果不是将会失败：

```
mysql> insert into t1 values(‘[1,2,’);
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '[1,2,’)' at line 1
```

* JSON\_TYPE\(\)函数期待一个JSON参数并尝试把它解析为JSON值。如果参数是有效的则会返回一个JSON类型，如果不是产生一个错误

```
mysql> select json_type('["a", "b", 1]');
+----------------------------+
| json_type('["a", "b", 1]') |
+----------------------------+
| ARRAY                      |
+----------------------------+
1 row in set (0.00 sec)

mysql> select json_type('"hello"');
+----------------------+
| json_type('"hello"') |
+----------------------+
| STRING               |
+----------------------+
1 row in set (0.00 sec)

mysql> select json_type('hello');
ERROR 3141 (22032): Invalid JSON text in argument 1 to function json_type: "Invalid value." at position 0.
```

MySQL以utf8mb4字符集和utf8mb4\_bin校难集来处理在JSON上下文中使用的字符串。别的字符集被转换为utf8mb4必要时（对于ascii或utf8字符集，没有必要转换因为ascii和utf8是utf8mb4的子集）

作为以字符串字面量的形式来写JSON值，存在函数来从组件元素值来组成JSON值。JSON\_ARRAY\(\)takes a\(possibly empty\) list of values 并返回包含这些值的JSON数组 ：

```
mysql> select json_array('a', 1, NOW());
+----------------------------------------+
| json_array('a', 1, NOW())              |
+----------------------------------------+
| ["a", 1, "2017-08-30 13:06:38.000000"] |
+----------------------------------------+
1 row in set (0.01 sec)
```

JSON\_OBJECT\(\) takes a\(possibly empty\) list of key-value pairs 并返回一个包含这些键值对的JSON对象

```
mysql> select json_object("key", 1, "key2", "abc");
+--------------------------------------+
| json_object("key", 1, "key2", "abc") |
+--------------------------------------+
| {"key": 1, "key2": "abc"}            |
+--------------------------------------+
```

JSON\_MERGE\(\) takes two or more JSON文档并返回一个结合的结果：

```
mysql> select json_merge('{"a":1}', '{"b": 2, "a": 11}');
+--------------------------------------------+
| json_merge('{"a":1}', '{"b": 2, "a": 11}') |
+--------------------------------------------+
| {"a": [1, 11], "b": 2}                     |
+--------------------------------------------+
```

关于合并的规则，查看标准化，合并和自动包裹JSON值：

```
mysql> select json_merge('{"a":1}', '{"b": 2, "a": 11}');
+--------------------------------------------+
| json_merge('{"a":1}', '{"b": 2, "a": 11}') |
+--------------------------------------------+
| {"a": [1, 11], "b": 2}                     |
+--------------------------------------------+
```

JSON也可以赋予用户定义的变量:

```
mysql> set @j = json_object("key", "value")；
Query OK, 0 rows affected (0.00 sec)

mysql> select @j;
+------------------+
| @j               |
+------------------+
| {"key": "value"} |
+------------------+
```

然而，用户定义的变量不能是JSON数组类型，尽管前面的列子看起来像JSON值且具有一样的字符集和检验集，但他不是JSON数组类型。相反的，当赋予变量时JSON\_OBJECT\(\)函数返回的值将被转换为字符串。

由JSON转换产生的字符串具有utf8mb4的字符集和utf8mb4\_bin校验集：

```
mysql> select charset(@j), collation(@j);
+-------------+---------------+
| charset(@j) | collation(@j) |
+-------------+---------------+
| utf8mb4     | utf8mb4_bin   |
+-------------+---------------+
1 row in set (0.00 sec)
```

因为utf8mb4\_bin是二进制的校验集，比较JSON值是对大小写敏感的

```
mysql> select json_array('x') = json_array('X');
+-----------------------------------+
| json_array('x') = json_array('X') |
+-----------------------------------+
|                                 0 |
+-----------------------------------+
```

大小写敏感性也适用于JSON的null, true, false字面量值，他们永远以小写的形式输写：

```
mysql> select json_valid('null'), json_valid('Null'), json_valid('NULL');
+--------------------+--------------------+--------------------+
| json_valid('null') | json_valid('Null') | json_valid('NULL') |
+--------------------+--------------------+--------------------+
|                  1 |                  0 |                  0 |
+--------------------+--------------------+--------------------+
1 row in set (0.00 sec)

mysql> select cast('null' as json);
+----------------------+
| cast('null' as json) |
+----------------------+
| null                 |
+----------------------+
1 row in set (0.00 sec)

mysql> select cast('NULL' as json);
ERROR 3141 (22032): Invalid JSON text in argument 1 to function cast_as_json: "Invalid value." at position 0.
```

JSON字面量与SQL的NULL,TRUE,FALSE值的不同是，他们可以被写成任意的大小写：

```
mysql> select isnull(null), isnull(Null), isnull(NULL);
+--------------+--------------+--------------+
| isnull(null) | isnull(Null) | isnull(NULL) |
+--------------+--------------+--------------+
|            1 |            1 |            1 |
+--------------+--------------+--------------+
```

一些时候描述或插入引号字符\("或‘\)到JSON文件中。假定此例中你想插入一些JSON对象包含代表MySQL的语句展示的一样：

```
mysql> CREATE TABLE facts (sentence JSON);
```

键值对是这样的：

```
mascot: The MySQL mascot is a dolphin named "sakila".
```

一种插入方式是作为JSON对象插入到facts表中使用MySQL的JSON\_OBJECT\(\)函数。在这种情况下，你一定要使用反斜杠转换每个引号字符，你下面展示的：

```
mysql> insert into facts values(JSON_OBJECT("mascot", "Our mascot is a dolphin named \"Sakila\"."));
Query OK, 1 row affected (0.00 sec)
```

如果你以同样的方式插入JSON对象的字符串将不会工作，在这种情况下，你一定要使用两个反斜杠的转义序列，像这样：

```
mysql> insert into facts values
    ->   ('{"mascot":"Our mascot is a dolphin named \\"Sakila\\"."}');
```

```
mysql> select sentence from facts;
+---------------------------------------------------------+
| sentence                                                |
+---------------------------------------------------------+
| {"mascot": "Our mascot is a dolphin named \"Sakila\"."} |
| {"mascot": "Our mascot is a dolphin named \"Sakila\"."} |
+---------------------------------------------------------+
```

查询

```
mysql> select sentence->"$.mascot" from facts;
+---------------------------------------------+
| sentence->"$.mascot"                        |
+---------------------------------------------+
| "Our mascot is a dolphin named \"Sakila\"." |
| "Our mascot is a dolphin named \"Sakila\"." |
+---------------------------------------------+
```

这样保留反斜线，要展示使用mascot为key的渴望的值，不包括任何引号的转义，使用-&gt;&gt;操作符：

```
mysql> select sentence->>"$.mascot" from facts;
+-----------------------------------------+
| sentence->>"$.mascot"                   |
+-----------------------------------------+
| Our mascot is a dolphin named "Sakila". |
| Our mascot is a dolphin named "Sakila". |
+-----------------------------------------+
2 rows in set (0.00 sec)

```



