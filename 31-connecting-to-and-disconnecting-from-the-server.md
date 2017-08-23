# 3.1 Connecting to and Disconnectiong from the Server

## 登陆

  
连接至服务，你将需要提供一个MySQL用户名在调用**mysql**,通常情况下，一个密码。如果你的服务运行在另的机器而不是你登陆的地方，也需要指定host名。联系你的管理员找到你连接时需要用到的连接参数。当你知道确切的参数，下可以像下面这样连接：

```bash
shell> mysql -h host -u user -p
Enter password: ********
```

_host_和_user_ 代表你的MySQL服务运行在哪台机器和你的MySQL账号的用户名。替换相应的设置值。_\*\*\*\*\*\*\*\*_代表你的密码；在mysql显示**Enter password:**提示符时输入。

如果一切工作正常，你就会看到一些介绍信息，后面紧接一个mysql&gt;提示符：

```bash
jiangxuedeMacBook-Pro:~ jianglei$ mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 5.7.17 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

**mysql&gt;**提示符告诉你**mysql**已经准备好接受你输入的SQL语句了。

如果你在MySQL运行的同一台机器上登陆,你可漏掉host,像下面这样简单的使用：

```java
shell> mysql -u user -p
```

如果，当你尝试登陆时，你得到的是错误的信息如ERROR 2002 \(HY000\): Can't connect to local MYSQL server through socket '/tmp/mysql.sock'\(2\),这意味着你的MySQL服务进程\(Unix\)或服务\(Windows\)没有运行。咨询管理员或参阅Chapter2, Installing and Upgrading MySql 适用于你操作系统的部分。

获取登陆经常遇到的问题的帮助，参阅Section B.5.2, "Common Errors When Using MySQL Programs"。

一些MySQL安装时允许在服务运行的同一台机器上匿名登陆。如果你的机器上也是这样，你可以不用任何参数连接mysql的服务：

```bash
shell> mysql
```

## 退出

在你连接成功之后，你可以任何时候在mysql&gt;提示符后键入**QUIT\(或 \q\)**来取消连接：

```
mysql> QUIT
Bye
```

在Unix，你可以通过输入Control + D取消连接

接下来章节的大多数例子假设你已经连接到服务。这通过**mysql&gt;**提示符表示

