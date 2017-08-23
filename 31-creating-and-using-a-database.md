# 3.3 Creating and Using a Database

一旦你知道怎么输入SQL语句时，你已经准备好访问数据库了。

假定你有一些宠物在你家\(你的动物园\)并你保持对他们几种信息类型的追踪。你可以通过创建表,载入需要的信息。然后访问不同的类型问题关于你的动物的通过从表中提取信息。此章展示了怎么执行下面的操作：

* 创建一个数据库
* 创建一个表
* 加载数据到表中
* 以不同的方式从表中提取数据
* 使用多个表

这个动物园数据库很简单\(故意的\)，不难想象在真实世界中使用类型的数据库。例如，像这样的数据库保持对动物的跟踪，兽医保持对病例的跟踪。动物园例子中包含的一些查询和样例数据可以从MySQL网站中获取。

使用**SHOW**语句找出目前此服务上的数据库有哪些：

```bash
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| blog               |
| menagerie          |
| mysql              |
| performance_schema |
| sys                |
| test02             |
| test0816           |
| test1              |
+--------------------+
```

mysql数据库描述了用户访问的权利。test数据库经常用来作为用尝试的地方。

在你的机器上显示的数据库可能不同；SHOW DATABASES不会展示没有权限的数据库或你没有SHOW DATABASES权限。参阅章节13.7.5.14, “SHOW DATABASES Syntax”

如果test数据库存在，尝试访问他：

```
mysql> use test
Database changed
```

USE, 像QUIT一样，不需要分号。\(你也可以以分号终止这样的语句如果你喜欢；这样做没有任何危害。\) USE语句是特殊的；该语句一定要在一行中指定。

你可以使用test数据库\(如果你可以访问他\)，但你在此数据库中创建的任何东西都可以被任何可以访问此数据库的人删除。因此，你可以向你MySQL管理员申请一个只有你权限访问的数据库。假定你想调用你的menagerie。管理员需要执行像下面的语句：

```
mysql> GRANT ALL ON menagerie.* TO 'your_mysql_name'@'your_client_host';
```

your\_mysql\_name是赋给你的MySQL用户名your\_client\_host是你连接服务的地址

