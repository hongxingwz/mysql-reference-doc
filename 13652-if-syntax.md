# 13.6.5.2 If Syntax

![](/assets/1504947402618.png)

The IF statement for stored programs implements a basic conditional construct.

> Note
>
> There is also an IF\(\) function, which differs from the IF statement described here. See Section 12.4, "Control Flow Functions". The **IF** statement can have **THEN**, **ELSE**, and **ELSEIF** clauses, and it is terminated with **END IF**.

if the **search\_condition** evaluates to true, the corresponding **THEN** or **ELSEIF** clause **statement\_list** executes. If no **search\_condition** matches, the **ELSE** clause **statement\_list** executes.

Each **statement\_list** consists of one or more SQL statements; an empty **statement\_list** is not permitted.

An **IF ... END IF** block, like all other flow-control blocks used within stored programs, must be terminated with a semicolon, as shown in this example:

```
DELIMITER $$

CREATE FUNCTION SimpleCompare(n INT, m INT)
    RETURNS VARCHAR(20)
    BEGIN
        DECLARE s VARCHAR(20);

        IF n > m THEN SET s = '>';
        ELSEIF n = m THEN SET s = '=';
        ELSE SET s = '<';
        END IF;

        SET s = CONCAT(n, ' ', s, ' ', m);

        RETURN s;
    END
$$
```



As with other flow-control constructs, **IF ... END IF **blocks may be nested within other flow-control constructs, including other **IF** statements. Each **IF **must be terminated by its own **END IF **followed by a semicolon. You can use indentation to make nested flow-control blocks more easily readable by humans \(although this is not required by MySQL\), as show here:

```
DELIMITER $$

CREATE FUNCTION VerboseComapre(n INT, m INT)
	RETURNS VARCHAR(50)

	BEGIN
		DECLARE s VARCHAR(50);
		IF n = m THEN SET s = 'equals';
		ELSE
			IF n > m THEN SET s = 'greater';
			ELSE SET s = 'less';
			END IF;

			SET s = CONCAT('is ', s, 'than');
		END IF;

		SET s = CONCAT(n, ' ', s, ' ', m, '.');

		RETURN s;
	END
$$
```

In this example, the inner **IF** is evaluated only if **n** is not equal to **m**.

