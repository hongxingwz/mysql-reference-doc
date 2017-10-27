

# mysql三目运算实现if, else的效果，避免不必要的查询
if(expr1, expr2, expr3)
如果expr1为真,返回expr2，则否返回expr3


### case when then else end 结构

```
CASE case_value
    WHEN when_value THEN statement_list
    [WHEN] when_value THEN statement_list] ...
    [ELSE statement_list]
END CASE
```

Or:


```
CASE 
    WHEN search_condition THEN statement_list
    [WHEN search_condition] THEN statement_list] ...
    [ELSE statement_list]
END CASE
```




case 1 when 1 then "b" else " c" end

case when ifnull(name, '')='' then 0 else 1 end 

