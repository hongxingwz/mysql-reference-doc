# MySQL数据库表名、列名、别名区分大小写的问题

MySQL在Linux下数据库，表名，列名，别名大小写规则是这样的：

* 数据库名与表名是严格区分大小写的
* 表的别名是严格区分大小写的
* 列名与列的别名在所有情况下均是忽略大小写的
* 变量名也是严格区分大小写的



MySQL在Windows下都不区分大小写



所以在不同操作系统中为了能使程序和数据库都能正常运行，最好的办法是在设计的时候都转为小写，但如果在设计的时候已经规范大小写了，那么在Windows环境下只要对数据库的配置做下改动就行了。具体操作如下：

在MySQL的配置文件中my.ini \[mysqld\]中增加一行

lower\_case\_table\_names=1

参数解释：
0：区分大小写
1：不区分大小写

在MySQL中，数据库和表对应那些目录下的目录和文件。因而，操作系统的敏感性决定数据库和表命名的大小写敏感。这就意味着数据库和表名在Windows中是大小写不敏感的，而在大多数类型的Unix系统中是大小写敏感的。

奇怪的是列名与列的别名在所有的情况下均是忽略大小写的，而表的别名又是区分大小写的。

要避免这个问题，你最要在定义数据命名规则的时候就全部采用小写字母加下列线的组合，而不使用任何的大写字母。

或者也可以强制以 -O lower\_case\_table\_names=1参数启动mysqld(如果使用--defaults-file=...\my.cnf参数来读取指定的配置文件启动mysqld的话，你需要在配置文件的[mysqld]区段下增加一行lower_case_table_names=1)。这样MySQL将在创建与查找时将所有的表名自动转换为小写字符(这个选项缺省地在Windows中为1，在Unix中为0)

当你更改这个选项时，你必须在启动mysqld前首先将老的表名转换为小写字母。

换句话说，如果你希望在数据库里面创建表的时候保留大小写字符状态，则应该把这个参数置为0：

   lower\_case\_table\_names=1。否则的话你会发现同样的sqldump脚本在不同的操作系统下最终导入的结果不一样(在Windows下所有的大写字符都变成小写了)。
   


# Mysql字符串区分大小写的问题
CREATE TABLE NAME(name VARCHAR(10));
对这个表，缺省情况下，下面两个查询的结果是一样的：
   


```
 SELECT * FROM TABLE NAME WHERE name='clip';

 SELECT * FROM TABLE NAME WHERE name='Clip';
```

Mysql默认查询是不区分大小写的，如果需要区分，必须在建表时，Binary标识敏感的属性。



```
   CREATE TABLE NAME(
   name VARCHAR(10) BINARY
   );
```

或在SQL语句中实现`SELECT * FROM TABLE NAME WHERE BINARY name='Clip'`






 



