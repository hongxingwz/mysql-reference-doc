# CREATE EVENT Syntax

![](/assets/1504658227324.png)

This statement creates and schedules a new event. The event will not run unless the Event Scheduler is enabled. For information about checking Event Scheduler status and enabling it if necessary, see Section 23.4.2 "Event Scheduler Configuration".

CREATE EVENT requires the EVENT privilege for the schema in which the event is to be created.It might also require the SUPER privilege, depending on the DEFINER value, as described later in this section.

The minimum requirements for a valid CREATE EVENT statement are as follows:
* The keywords CREATE EVENT plus an event name, which uniquely identifies the event in a database schema.
* An ON SCHEDULE clause, which determines when and how often the event executes.
* A DO clause, which contains the SQL statement to be executed by an event.

This is an example of a minimal CREATE EVENT statement:


```
CREATE EVENT myevent
 ON SCHEDULE AT CURRENT_TIMESTAMP + INTERVAL 1 HOUR
 DO
  UPDATE myschema.mytable SET mycol = mycol + 1;
```
The previous statement creates an event named **myevent**. This event executes once - one hour following its creation - by running an SQL statement that increments the value of the **myschema.mytable** table's **mycol** column by 1

The event\_name must be valid MySQL identifier with a maximum length of **64** characters. Event names are not case sensitive,so you cannot have two events named **myevent** and **MyEvent** in the same schema. In general, the rules  governing event names are the same as those for names of stored routines. See Section 9.2, "Schema Object Names".



