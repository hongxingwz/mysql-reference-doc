# Functions and Operations

表达式可以用在SQL语句中的几处，例如在SELECT语句的ORDER BY 或 HAVING语法，在SELECT, DELETE, 或UPDATE语句的WHERE语法，或者在SET语句中。表达式可以使用字面量值，列值，NULL，内置的函数，存储的函数，用户定义的函数，和操作符。此章描述了函数和操作符在MYSQL允许出现在表达式中。对于定义stored functions 和 userdefined functions在23.2节 "Using Stored Routines\(Procedures and Functions\)"和28.4节， "Adding New Functions to MySQL"。9.2.4节"Function Name Parsing and Resolution"，描述了服务解释引用不同种类函数的规则。

一个包含NULL的表达值永远返回NULL除非另外声明在文档中为特别的函数或操作

> 注：
>
> 默认情况下，在函数和他接下来的括号之间一定不要有空白字符。这帮助MySQL解释器区分函数调用和对表或列的引用\(如果函数和列或表具有相同的同名\)，然而，在函数参数之间的空格是允许的。

你可以让MySQL服务接受在函数名后出现空格通过指定--sql-model=IGNORE\_SPACE选项\(5.1.8章 "Serval SQL Models"\)单独的客户端程序可以通过mysql\_real\_connect\(\)。CLIENT\_IGNORE\_SPACE选项。在这两种情况下，所有函数名都成为保留字。

出于简洁的目的，此章的大多数例子以缩写的形式展示mysql程序的输出。而不是以下面的形式展式

```
mysql> select mod(29, 9);
+------------+
| mod(29, 9) |
+------------+
|          2 |
+------------+
1 row in set (0.01 sec)
```

而是用下面的形式列出：

```
mysql> select mod(29, 9);
        -> 2
```



