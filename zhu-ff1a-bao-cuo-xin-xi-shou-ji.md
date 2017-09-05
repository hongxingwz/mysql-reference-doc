# 报错信息收集

## 创建唯一索引时失败

**语句：**

```
create unique  index pk_name4 using BTREE  on message(name4(30) desc) comment "name4 index";
```

但是创建失败，返回的结果是：

```
Error Code: 1062. Duplicate entry '' for key 'pk_name4'
```

**原因：**

```
Database changed
mysql> select * from test0903.MESSAGE;
+----+----------+-----+-------+-------+-------+
| ID | MESSAGE  | id2 | name2 | name3 | name4 |
+----+----------+-----+-------+-------+-------+
|  1 | jianglei |  -1 |       |       |       |
|  2 | dengyi   |  -1 |       |       |       |
+----+----------+-----+-------+-------+-------+
```

该列有重复的值，所以不能创建唯一索引

**结论：将name4的所有值改为不重复，即可用上述语句新建索引成功**

```
mysql> select * from test0903.MESSAGE;
+----+----------+-----+-------+-------+----------+
| ID | MESSAGE  | id2 | name2 | name3 | name4    |
+----+----------+-----+-------+-------+----------+
|  1 | jianglei |  -1 |       |       | dengyi   |
|  2 | dengyi   |  -1 |       |       | jianglei |
+----+----------+-----+-------+-------+----------+
```



