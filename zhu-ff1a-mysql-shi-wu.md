# MySQL事物处理实现

MySQL的事务处理主要有两种方法

1.用begin, rollback, commit来实现

begin 开始一个事务

rollback事务回滚

commit 事务确认

2.直接用set来改变mysql的自动提交模式

set autocommit = 0 禁止自动提交

set autocommit = 1 开启自动提交



