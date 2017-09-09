# 23.3.1 Trigger Syntax and Examples

To create a trigger or drop a trigger, use the **CREATE TRIGGER** or **DROP TRIGGER **statement, described in Section 13.1.20, "CREATE TRIGGER Syntax", and Section [13.1.31, "DROP TRIGGER Syntax"](13131-drop-trigger-syntax.md).

```
mysql> CREATE TABLE account(acct_num INT, amount DECIMAL(10, 2));
    -> 
    -> CREATE TRIGGER ins_sum BEFORE INSERT ON account
    -> FOR EACH ROW SET @sum = @sum + NEW.amount;
    -> $$
Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.05 sec)
```

The CREATE TRIGGER statement creates a trigger named **ins\_sum** that is associated with the **account **table. It also includes clauses that specify the trigger action time, the triggering event, and what to do when the trigger activates:

* The keyword **BEFORE** indicates the trigger action time. In this case, the trigger activates before each row inserted into the table. The other permitted keyword here is **AFTER.**
* The keyword **INSERT **indicates the trigger event; that is, the type of operation that activates the trigger.In the example, **INSERT** operations cause trigger activation. You can also create triggers for **DELETE** and **UPDATE** operations.
* The statement following **FOR EACH ROW** defines the trigger body; that is, the statement to execute each time the trigger activates, which occurs once for each row affected by the triggering event. In the example, the trigger body is a simple **SET** that accumulates into a user variable the values inserted into the amount column. The statement refers to the column as **NEW.amount** which means "the value of the **amount **column to be inserted into the new row."

To use the trigger, set the accumulator variable to zero, execute an **INSERT** statement, and then see what value the variable has afterward:

```
mysql> SET @sum = 0;
    -> INSERT INTO account VALUES(137, 14.98), (141, 1937.50), (97, -100.00);
    -> SELECT @sum as 'Total amount inserted';

+-----------------------+
| Total amount inserted |
+-----------------------+
|               1852.48 |
+-----------------------+
```

In this case, the value of **@sum **after the **INSERT** statement has executed is **14.98 + 1937.50 -100,** or **1852.48.**

To destroy the trigger, use a [DROP TRIGGER](/13131-drop-trigger-syntax.md "DROP TRIGGER") statement. You must specify the schema name if the trigger is not in the default schema:

```sql
mysql> DROP TRIGGER test0907.ins_sum;
```

If you drop a table, any triggers for the table are also dropped.

Trigger names exist in the schema namespace, meaning that all triggers must have unique names within a schema. Triggers in different schemas can have the same name.

As of MySQL 5.7.2, it is possible to define multiple triggers for a given table that have the same trigger event and action time. For example, you can have two **BEFORE UPDATE** triggers for a table. By Default, triggers that have the same trigger event and action time activate in the order they were created. To affect trigger order, specify a clause after **FOR EACH ROW** that indicates **FOLLOWS** or **PRECEDES** and the name of an existing trigger that  has the same trigger event and action time. With **FOLLOWS**, the new trigger activates after the existing trigger. With **PRECEDES**, the new trigger activates before the existing trigger.

For example, the following trigger definition defines another **BEFORE INSERT** trigger for the **account** table:

```
CREATE TRIGGER ins_transaction BEFORE INSERT ON account
FOR EACH ROW PRECEDES ins_sum
SET
@deposits = @deposits + IF(NEW.amount > 0, NEW.amount, 0),
@withdrawals = @withdrawals + IF(NEW.amount < 0, -NEW.amount, 0);
```

This trigger, **ins\_transactioin**, is similar to **ins\_sum** but accumulates deposits and withdrawals separately. It has a **PRECEDES** clause that causes it to activate before **ins\_sum**; without that clause, it would activate after **ins\_sum** because it is created after **ins\_sum**.

