# MySQL大小写的敏感性



MySQL在Linux下数据库名，表名，列名，别名大小写规则是这样的

* 数据库名与表名是严格区分大小写的。
* 表的别名是严格区分大小写的。
* 列名与列的别名在所有的情况下均是忽略大小写的。
* 字段内容默认情况下是大小写不敏感的。

mysql中控制数据库表名的大小写敏感由参数lower\_case\_table\_names控制，为0时表示区分大小写，为1时，表示将名字转化为小写后存储，不区分大小写






