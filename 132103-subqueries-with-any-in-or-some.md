# 13.2.10.3 Subqueries with ANY, IN, or SOME

Syntax:

![](/assets/1505209091245.png)
Where **comparison\_operator** is one of these operators:


```
= > < >= <= <> !=
```
The **ANY** keyword, which must follow a comparison operator, means "return TURE if the comparison is TRUE for ANY of the values in the column that subquery returns." For  example:


```
SELECT s1 FROM t1 WHERE s1 > ANY(SELECT s1 FROM t2);
```
Suppose that there is a row in table **t1** containing **(10)**. The expression is **TRUE** if table **t2** contains **(21, 14, 7)** because there is a value **7** in **t2** that is less than **10**. The expression is **FALSE** if table **t2** contains**(20, 10)**, or if table **t2** is empty. The expression is **_unknown_**(that is, **NULL**) if table t2 contains **(NULL, NULL, NULL)**.

When used with a subquery, the word **IN** is an alias for **= ANY**. Thus, these two statements are the same:


```
SELECT s1 FROM t1 WHERE s1 <> ANY (SELECT s1 FROM t2);
SELECT s1 FROM t1 WEHRE s2 <> SOME (SELECT s1 FROM t2);
```

Use of the word **SOME** is rare, but this example shows why it might be useful. To most people, the English phrase 'a is not equal to any b' means "there is no b which is equal to a", but that is not what is meant by the SQL syntax. The syntax means "there is some b to which a is not equal." Using **<> SOME** instead helps ensure that everyone understands the true meaning of the query.



