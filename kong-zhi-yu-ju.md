case 1 when 1 then "b" else " c" end

# mysql三目运算实现if, else的效果，避免不必要的查询
if(expr1, expr2, expr3)
如果expr1为真,返回expr2，则否返回expr3