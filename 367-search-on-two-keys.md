# Searching on Two Keys

OR语句使用单一的KEY优化的很好，就像处理AND一样

一个棘手的情况是，在两个不同的key上搜索用OR结合在一起:

```
SELECT field1_index, field2_index FROM test_table WHERE  field1_index = '1' OR field2_index='1'
```

这种情况也会被优化。查看Section 8.2.13, "Index Merge Optimiation"

你也可以使用UNION有效的解决此问题，UNION语句把两个分离的SELECT语句结合在一起了。查看Section 13.2.9.3 "UNION Syntax"。

每个SELECT仅使用一个key这样的语句可以被优化

```
SELECT field1_index, field2_index
    FROM test_table WHERE field1_index ='1'
UNION
SELECT field1_index, field2_index
    FROM test_table WHERE field2_index = '1';
```



