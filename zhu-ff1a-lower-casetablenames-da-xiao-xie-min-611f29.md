# lower\_case\_table\_names\(大小写敏感\)

## 简介

在MySQL中，数据库对应数据目录中的目录。数据库中的每个表至少对数据库目录中的一个文件\(也可能是多个，取决于存储引擎\)。因此，所使用操作系统的大小写敏感性决定了数据库名的大小写敏感性。

在多大多数Unix中数据库名和表名对大小写敏感，而在Windows中对大小写不敏感。一个显著的例外情况是Mac OS X,它基于Unix但使用默认文件系统类型\(HFS+\)，对大小写不敏感。然而，Mac OS x也支持UFS卷，该卷对大小写敏感，就像Unix一样。

变量lowser\_case\_file\_system说明是否数据目录所在的文件系统对文件名的大小写敏感。ON说明对文件名的大小写不敏感，OFF表示敏感。

例如在Linux下查看：

```
mysql> show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 1     |
+------------------------+-------+
2 rows in set (0.00 sec)
```

说明linux系统对大小写敏感，MySQL也默认设置为对大小写不敏感。

## 大小写区分规则

Linux下：

* 数据库名与表名是严格区分大小写的；
* 表的别名是严格区分大小写的；
* 列名与列的别名在所有情况下均是忽略大小写的；
* 变量名也是严格区分大小写的；

Windows下：

* 都不区分大小写

Mac OS 下\(非UFS卷\)

* 都不区分大小写

## 参数说明\(lower\_case\_table\_names\)

Unix下lowser\_case\_table\_names默认值为0. Windows下默认值是1. Mac OS X下默认值是2.

| 参数值 | 解释 |
| --- | --- |
| 0 | 使用CREATE TABLE 或 CREATE DATABASE语句指定的大小写字母在硬盘上保存表名和数据库名。名称比较对大小写敏感。在大小写不敏感的操作系统如Windows或Mac OS X 上我们不能将该参数设置为0，如果在大小写不敏感的文件系统上将 -- lower\_case\_table\_names强制设置为0，并且使用不同的大小写访问MyISAM表象，可能会导致索引破坏。 |
| 1 | 表名在硬盘上以小写保存，名称比较对大小写敏感。MySQL将所有表名转换为小写在存储和查找表上。该行为也适合数据库名和表的别名。该值为Windows的默认值 |
| 2 | 表名和数据库在硬盘上使用CREATE TABLE 或 CREATE DATABASE语句指定的大小写字母进行保存，但MySQL将它们转换为小写在查找表上。名称比较对大小写不敏感，即按照大小写来保存，按照小写来比较。注释：只在对大小写不敏感的文件系统上适用！InnoDB表名用小写保存 |



