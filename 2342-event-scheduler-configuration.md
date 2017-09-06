# 23.4.2 Event Scheduler Configuration

Events are executed by a special event scheduler thread; when we refer to the Event Scheduler, we actually refer to this thread. When running, the event scheduler thread and its current state can be seen by users having the **PROCESS** privilege in the output of **SHOW PROCESSLIST**, as shown in the discussion that follows.

The global **event\_scheduler** system variable determines whether the Event Scheduler is enabled and running on the server. It has one of these 3 values, which affect event scheduling as described here:

* **OFF**: The Event Scheduler is stopped. The event scheduler thread does not run, is not shown in the output of **SHOW PROCESSLIST**, and no scheduled events are executed. **OFF** is the default value for **event\_scheduler**

  When the Event Scheduler is stopped\(event\_scheduler is OFF\), it cat be started by setting the value of event\_scheduler to ON\(See next item.\)

* **ON**: The Event Scheduler is started; the event scheduler thread runs and executes all scheduled events.

  When the Event Scheduler is ON, the event scheduler thread is listed in the output of **SHOW PROCESSLIST** as a daemon process,  
  and its state is represented as shown here:

```sql
mysql> show processlist \G
*************************** 4. row ***************************
     Id: 1277
   User: event_scheduler
   Host: localhost
     db: NULL
Command: Daemon
   Time: 10
  State: Waiting on empty queue
   Info: NULL
4 rows in set (0.01 sec)
```

Event scheduling can be stopped by setting the value of **event\_scheduler** to **OFF**

* **DISABLED**:This value renders the Events Scheduler nonoperational. When the Event Scheduler is **DISABLED**, the event scheduler thread does not run \(and so does not appear in the output of **SHOW PROCESSLIST**\).In addition, the Event Scheduler state cannot be changed at runtime.

if the Event Scheduler status has not been set to **DISABLED**, **event\_scheduler** can be toggled between **ON** and **OFF**\(using **SET**\). It is also possible to use **0** for **OFF**, and **1** for **ON**  when setting this  variable. Thus, any of the following 4 statements can be used in the **mysql** client to turn on the Event Scheduler;

```
mysql> SET GLOBAL event_scheduler = ON;
mysql> SET @@global.event_scheduler = ON;
mysql> SET GLOBAL event_scheduler = 1;
mysql> SET @@global.event_scheduler = 1;
```

Similarly, any of these 4 statements can be used to turn off the Event Scheduler:

```
mysql> SET GLOBAL event_scheduler = OFF;
mysql> SET @@global.event_scheduler = OFF;
mysql> SET GLOBAL event_scheduler = 0;
mysql> SET @@global.event_scheduler = 0;
```

Although **ON** and **OFF** have numeric equivalents, the value displayed for **event\_scheduler** by **SELECT** or **SHOW VARIABLES** is always one of **OFF**, **ON**, or **DISABLED**. **DISABLED**  has no numeric equivalent. For this reason, **ON** and **OFF** are usually preferred over 1 and 0 when setting this variable.

Note that attempting to set event\_scheduler without specifying it as a global variable causes an error:

```
mysql> set event_scheduler=ON;
ERROR 1229 (HY000): Variable 'event_scheduler' is a GLOBAL variable and should be set with SET GLOBAL
```

To disable the event scheduler,use one of the following two methods:

* as a command-line option when starting the server:

```
--event_scheduler=DISABLED
```

* In the server configuration file\(my.cnf, or my.ini on Windows systems\), include the line where it will be read by the server\(for example, in a \[mysqld\] section\):

```
event_scheduler=DISABLED
```

To enable the Event Scheduler, restart the server without the --event-scheduler=DISABLED command-line option, or after removing or commenting out the line containing event-scheduler=DISABLED in the server configuration file, as appropriate.Alternatively, you can use ON\(or 1\) or OFF \(or 0\) in place of the DISABLED value when starting the server.



> Note



Starting the MySQL server with the **--skip-grant-tables** options causes **event\_scheduler to be **to be set to **DISALBED, **overriding any other value set either on the command line or in the **my.cnf **or **my.ini**\(Bug \#26807\).

For SQL statements used to create, alter, and drop events, see Section 23.4.3, "Event Syntax".

MySQL provides an EVENTS table in the INFORMATION_SCHEMA database. This table can be queried to obtain information about scheduled events which have been defined on the server. See Section 23.4.4 "Event Metadata", and Section 24.7 "The INFORMATION\_SCHEMA", for more information.

For information regarding event scheduling and the MySQL privilege system, see Section 23.4.6, "The Event Scheduler and MySQL Privileges".



