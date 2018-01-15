# 总：Mysql事务

## 事务的特征

以下内容出处《高性能MySQL》第三版，了解事务的ACID及四种隔离级别有助于我们更好的理解事务动作。

下面举一个银行应用是解释事务必要性的一个经典的例子。假如一个很行的数据库有两张表：支票表\(checking\)和储蓄表\(savings\)。现在要从用户Jane的支票帐户转移200美元到她的储蓄帐户，那么至少需要三个步骤：

* 检查支票帐户的余额高于或等于200美元
* 从支票帐户余额中减去200美元。
* 在储蓄帐户余额中增加200美元。

上述三个步骤的操作必须打包在一个事务中，任何一个步骤失败，则必须回滚所有的步骤。

可以用START TRANSACTION语句开始一个事务，然后要么使用COMMIT提交将修改的数据持久保存，要么使用ROLLBACK撤销所有的修改。事务SQL的样本如下：

```
1. start transaction
2. select balance from checking where customer_id=10233276
3. update checking set balance= balance-200.00 where customer_id=10233276
4 update savings set balance = balance + 200.00 where customer_id = 10233276
5. commit;
```

ACID

* 原子性（atomicity）

  一个事务必须被视为一个不可分割的最小工作单元，整个事务中的所有操作要么全部提交成功，要么全部失败回滚，对于一个事务来说，不可能只执行其中的一部分操作，这就是事务的原子性。

* 一致性（consistency）

  数据库总是从一个致性的状态转换到另一个一致性的状态。（在前面的例子中，一致性确保了，即使在执行第三，四条语句之间时系统崩溃，支票帐户中也不会损失200美元，因为事务最终没有提交，所以事务中所做的修改也不会保存取数据库中）

* 隔离性（isolation）

  通常来说，一个事务所做的修改在最终提交以前，对其他事务是不可见的。（在前面的例子中，当执行完第三条语句，第四条语句还未开始，此时有另外的一个帐户汇总程序开始运行，则其看到支票帐户的余额并没有被减去200美元）

* 持久性（durability）

  一旦事务提交，则其所做的修改会永久保存到数据库。（此时即使系统崩溃，修改的数据也不会丢失。持久性是个有点模糊的概念，因为实际上持久性也分很多不同的级别。有些持久性策略能够提供非常强的安全保障，而有些则未必，而且不可能有做到100%的持久性保证的策略。）

## 事务的隔离级别

* READ UNCOMMITTED\(未提交读\)
* READ COMMIT\(提交读\)
* REPEATABLE READ\(可重复读\)
* SERIALIZABLE\(可串行化\)

**设置事务隔离级别的语句**

```
set tx_isolation="READ-UNCOMMITTED"
```

**显示事务隔离级别的语句**

```
select @@tx_isolation;
show variables like "%tx_isolation%"
```

**第1级别：Read Uncommitted（读取未提交内容）**

* 所有事务都可以看到其他未提交事务的执行结果
* 本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少
* 该级别引发的问题是--脏读\(Dirty Read\): 读取到了未提交的数据

```
#首先，修改隔离级别
mysql> set tx_isolation="READ-UNCOMMITTED";
Query OK, 0 rows affected (0.00 sec)

mysql> select @@tx_isolation;
+------------------+
| @@tx_isolation   |
+------------------+
| READ-UNCOMMITTED |
+------------------+
1 row in set (0.00 sec)

#事务A: 启动一个事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+
3 rows in set (0.00 sec)

# 事务B: 也启动一个事务(那么两个事务交叉了，在事务B中执行更新语句，且不提交)

mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> update tx set num=10 where id=1;
Query OK, 1 row affected (0.00 sec)

# 事务A:那么这时候事务A能看到这更新了的数据吗？
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |   10 | ----> 可以看到！说明我们读到了事务B还没有提交的数据
|  2 |    2 |
|  3 |    3 |
+----+------+

# 事务B: 事务B回滚，仍然未提交
rollback;

select * from tx;
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+

# 事务A: 在事务A里面看到的也是B没有提交的数据
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |   ----> 脏读意味着我们在这个事务中（A中），事务B虽然没有提交，但它任何一条数据变化，我都可以看到！
|  2 |    2 |
|  3 |    3 |
+----+------+
```

**第2级别：Read Committed\(读取提交内容\) **  
1. 这是大多数数据库系统的默认隔离级别\(但不是MySQL默认的\)  
2. 它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变  
3. 这种隔离级别出现的问题--不可重复读\(Nonrepeatable Read\): 不可重复读意味着我们在同一个事务中执行完全相同的select语句时可能看到不一样的结果。

导致这种情况的原因可能有：\(1\)有一个交叉的事务有新的commit，导致了数据的改变。\(2\)一个数据库被多个实例操作时，同一事务的其他实例在该实例处理期间可能会有新的commit

```
# 首先修改隔离级别
mysql> set tx_isolation="read-committed";
Query OK, 0 rows affected (0.00 sec)

mysql> select @@tx_isolation;
+----------------+
| @@tx_isolation |
+----------------+
| READ-COMMITTED |
+----------------+
1 row in set (0.00 sec)

# 事务A: 启动一个事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+
3 rows in set (0.00 sec)

事务B: 也启动一个事务（那么两个事务交叉了，在这事务中更新数据，且未提交）
mysql> begin;
Query OK, 0 rows affected (0.00 sec)

mysql> update tx set num=10 where id = 1;
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |   10 |
|  2 |    2 |
|  3 |    3 |
+----+------+


# 事务A: 这个时候 们在事务A中能看到数据的变化吗？
(并不能看到！）
select * from tx;

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+

# 事务B: 如果提交了事务B呢？
commit;

# 事务A:
因为事务B已经提交了，所以在A中我们看到了数据变化
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |   10 |
|  2 |    2 |
|  3 |    3 |
+----+------+

但是两次select的语句不一致
```

**第3级别： Repeatable Read\(可重读\)**

* 这是MySQL的默认事务隔离级别
* 它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行
* 此级别可能出现的问题--幻读\(Phantom Read\): 当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的"幻影"行
* InnoDB和Falcon存储引擎通过多版本并发控制\(MVCC, Multiversion Concurrency Control\)机制解决了该问题

```
set tx_isolation='repeatable-read';
select @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+

#事务A: 启动一个事务
mysql> start transaction;
Query OK, 0 rows affected (0.00 sec)

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |
|  2 |    2 |
|  3 |    3 |
+----+------+
3 rows in set (0.00 sec)

# 事务B: 开启一个新事务(那么这两个事务交叉了) 在事务B中更新数据，并提交
start transaction;

mysql> update tx set num=10 where id=1;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |   10 |
|  2 |    2 |
|  3 |    3 |
+----+------+
3 rows in set (0.00 sec)

mysql> commit;
Query OK, 0 rows affected (0.00 sec)

#事务A: 这时候即使事务B已提交了，但A能不能看到数据变化？
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |    1 |  ---> 还是看不到！(为和级别2不一样，也说明级别3解决不可重复读问题)
|  2 |    2 |
|  3 |    3 |
+----+------+

# 事务A: 只有当事务A也提交了，它才能够看到数据变化
mysql> select * from tx;
+----+------+
| id | num  |
+----+------+
|  1 |   10 |
|  2 |    2 |
|  3 |    3 |
+----+------+
3 rows in set (0.00 sec)
```

**第4级别： Serializable\(可串行化\)**

* 这是最高的隔离级别
* 它通过强制事务百序，使之不可能相互冲突，从而解决幻读问题。简言这，它是在每个计的数据行上加上共享锁。
* 在这个级别，可能导致大量的超时现象和锁竞争

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| :--- | :--- | :--- | :--- |
| 读未提交（Read-uncommitted） | 1 | 1 | 1 |
| 读已提交（Read-committed） | 0 | 1 | 1 |
| 可重复读（Repeatable read） | 0 | 0 | 1 |
| 可串行化（Serializable） | 0 | 0 | 0 |



