# 13.6.5.1 CASE Syntax

![](/assets/1504945251200.png)The **CASE** statement for stored programs implements a complex conditional construct.

> Note
>
> There is also a **CASE** expression, which differs from the **CASE** statement described here. See Section 12.4, "Control Flow Functions". The **CASE** statement cannot have an **ELSE NULL** clause, and it is terminated with **END CASE** instead of **END**

For the first syntax, **case\_value** is an expression. This value is compared to the **when\_value** expression in each **WHEN** clause until one of them is equal. When an equal when\_value is found, the corresponding **THEN** clause **statement\_list** executes. If no when\_value is equal, the ELSE clause statement\_list executes, if there is one.

This syntax cannot be used to test for equality with **NULL** because **NULL = NULL** is false. see Section 3.3.4.6, "Working with NULL Values".

For the second syntax, each **WHEN** clause **search\_condition** expression is evaluated until one is true, at which point its corresponding **THEN** clause **statement\_list** executes. If no **search\_condition** is equal, the ELSE clause **statement\_list** executes, if there is one.

If no **when\_value** or **search\_condition** matches the value tested and the **CASE** statement contains no **ELSE** clause, a **Case not found for CASE statement** error results.

Each **statement\_list** consists of one or more SQL statements; an empty **statement\_list** is not permitted.

To handle situations where no value is matched by any **WHEN** clause, use and **ELSE** containing an empty **BEGIN ... END** block, as shown in this example.(The indentation used here is the ELSE clause is for purposes of clarity only, and is not otherwise significant.)



```
DELIMITER |

CREATE PROCEDURE p()
    BEGIN
        DECLARE v INT DEFAULT 1;
        
        CASE v
            WHEN 2 THEN SELECT v;
            WHEN 3 THEN SELECT 0;
            ELSE
                BEGIN
                END;
        END CASE;
    END;
$$   
```



