# Retreving Information from a Table

SELECT语句用来从表中拉取信息。该语句最普通的形式是：

```
SELECT what_to_select
FROM which_table
WHERE conditions_to_satisfy;
```

what/_to/_select表示你想要查询什么。这可以是一系列列(columns),或者/*表示所有的列。which\_talbe表示你想从哪个表里面提取数据。WHERE语句是可选的。如果它出现了，conditions\_to\_satisfy指定了一个或多个条件(提取的行一定要具有资格)。



