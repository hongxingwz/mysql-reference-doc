# 3.3.1 创建和选择数据库

如果管理员在设置你权限时创建了你的数据库，你可以开始即用他。否则，你需要自己创建他：

```bash
mysql> create database menagerie;
Query OK, 1 row affected (0.00 sec)
```

在Unix下，数据名是大小写敏感的\(不像SQL关键字\),因此你应该使用menagerie来引用你的数据库，而不是Menagerie, MENAGERIE，或其他的变体。这也适用于表名。\(在Windows下，这样的限制不适用，但是你必须使用相同的大小写\)

> 注：
>
> 当你尝试创建数据库时，你得到这样ERROR 1044\(42000\):Access denied for user 'micah'@'localhost' to database 'menagerie'的错误，这表示你用户账号没有需要的权限这创建数据库。与管理员讨论或参阅在6.2章 “The MySQL Access Privilege System”

创建数据库之后并没有选择使用他；你必须显式的这样做；使menagerie成为当前的数据库，使用这样的语句：

```sql
mysql> USE menagerie
Database changed
```

你的数据库仅需要创建一次，但每次你使用他的时候都必须选择他当你开始一个mysql session时。当可以像上面例子那样显式的使用USE语句。可选的，你可以使用命令行参数来选择数据库当你调用mysql命令时。仅需要指定数据库的名字在任何连接参数之后。例如：

```bash
shell>mysql -h host -u user -p mengerie
Enter password: *********
```

> 重要的
>
> 在命令中的menagerie不是你的密码。如果你想提供密码在命令行的-p参数之后，你必需不要有空格\(例如：像-pmypassword, 而不是-p mypassword\)。然而，在命令中输入你的密码是不推荐的，因为这样会暴露密码给在你机器上登陆的其他用户

> 注：
>
> 你可以在任何时候查看当前使用的数据库使用SELECT DATABASE\(\).



