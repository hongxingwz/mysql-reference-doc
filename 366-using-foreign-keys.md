# Using Foreign Keys

在MySQL中，InnoDB表支持检查外键限制。查看14章，The InnoDB Storage Engine,和 1.8.2.3节， ”Foreign Key Differences“

仅仅关联两个表时外键约束不是必须的。对于不是InnoDB的存储引擎，当使用REFERENCES tbl\_name\(col\_name\)语句时，是没有真正的效果的，服务器仅作为一个提醒或注释为你当前正在定义打算引用其他表的列时。当你使用此语法时注意下面的事项是非常重要的：

* MySQL不会执行任何形式的检查来确保col\_name真正的存在于tbl\_name\(甚至tbl\_name自身都不会存在\)
* MySQL不会执行任何形式的动作在tbl\_name中，例如 此语句不包含ON DELETE 或ON UPDATE的行为（尽管你可以定义ON DELETE 或ON UPDATE语句作为REFERENCES语句的一部分，但他会被忽略）
* 此语法创建一列，但不会创建任何形式的索引



