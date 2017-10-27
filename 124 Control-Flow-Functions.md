# Control Flow Functions

|Name|Description |
|---|---|
|CASE| CASE operator|
|IF()| if/ else construct|
|IFNULL()| Null if/else construct|
|NULLIF() | Return NULL if expr1 = expr2|

## CASE
* CASE value WHEN [compare_value] THEN result [WHEN [compare\_value] THEN result ...] [ELSE result] END

* CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...][ELSE result] END

The first CASE syntax returns the result for the first value=compare\_value comparison that is true. The second syntax returns the result for the first condition that is true. If no comparison or condition is true, the result after ELSE is returned, or NULL if there is no ELSE part.


> **Note**
>
> The syntax of the CASE expression described here differs slightly from that of the SQL CASE statement described in Section 13.6.5.1, "CASE Syntax", for use inside stored programs. The CASE statement cannot have an ELSE NULL clause, and it is terminated with END CASE instead of END.

The return type of a CASE expression result is the aggregated type of all result values.



```
# 第一种语句类似switch
mysql> SELECT CASE 1 WHEN 1 THEN 'one'
    -> WHEN 2 THEN 'two' ELSE 'more' END;
    
    -> 'one'

# 第二种语句类似 if elseif
mysql> SELECT CASE WHEN 1>0 THEN 'true' ELSE 'false' END;

    -> 'true'
    
mysql> SELECT CASE BINARY 'B' 
    -> WHEN 'a' THEN 1 WHEN 'b' THEN 2 END;
    
    -> null 
```



