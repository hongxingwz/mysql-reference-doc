# 5.1.8 Server SQL Modes

The MySQL server can operate in different SQL modes, and can apply these modes differently for different clients, depending on the value of the **sql\_mode** system variable. DBAs can set the global SQL mode to match site server operation requirements, and each application can set its session SQL mode to its own requirements.

Modes affect the SQL syntax MySQL supports and data validation checks it performs. This makes it easier to use MySQL in different environments and to use MySQL together with other database servers.

* Setting the SQL Mode
* The Most Important SQL Modes
* Full List of SQL Mode
* Combination SQL Modes
* Strict SQL Mode
* Comparison of the Ignore Keyword and Strict SQL Mode
* SQL Model Changes in MySQL 5.7

For answers to questions often asked about server SQL modes in MySQL, see Section A.3 "MySQL 5.7 FAQ: Server SQL Mode"

When working with InnoDB tables, consider also the innodb\_strict\_mode system variable. It enables additional error checks for InnoDB talbes.

## Setting the SQL Mode

The default SQL mode in MySQL 5.7 includes these modes: ONLY\_FULL\_GROUP\_BY,  STRICT\_TRANS\_TABLES, NO\_ZERO\_IN\_DATE, NO\_ZERO\_DATE, ERROR\_FOR\_DIVISION\_BY\_ZERO, NO\_AUTO\_CREATE\_USER, and NO\_ENGINE\_SUBSTITUTION.

The ONLY\_FULL\_GROUP\_BY and STRICT\_TRANS\_TABLES modes were added in MySQL 5.7.5. The NO\_AUTO\_CREATE\_USER mode was added in MySQL 5.7.7. The ERROR\_FOR\_DIVISION\_BY\_ZERO, NO\_ZERO\_IN\_DATE modes were added in MySQL 5.7.8. For additional discussion regarding these changes to the default SQL model value, see SQL Mode Changes in MySQL 5.7.

To set the SQL mode at server startup, use the --sql-mode="modes" option on the command line, or sql-mode="modes" in an option file such as my.cnf\(Unix operating systems\) or my.ini\(Windows\).modes is a list of different modes separated by commas. To clear the SQL mode explicitly, set it to an empty string using --sql-mode="" on the command line, or sql-mode="" in an option file.

> NOTE
>
> MySQL installation programs may configure the SQL mode during the installation process. For example, mysql\_install\_db creates a default option file named my.cnf in the base installation directory. This file contains a line that sets the SQL mode; see Section 4.4.2, "mysql\_install\_db --Initialize MySQL Data Directory"
>
> If the SQL model differs from the default or from what you expect, check for a setting in an option file that the server reads at startup.

To change the SQL mode at runtime, set the global or session sql\_mode system variable using a SET  statement:

```sql
SET GLOBAL sql_mode = 'modes';
SET SESSION sql_mode = 'modes';
```

Setting the GLOBAL variable requires the SUPER privilege and affects the operation of all clients that connect from that time on. Setting the SESSION variable affects only the current client. Each client can change its session sql\_mode     value at any time.

To determine the current global or session sql\_mode value, use the following statements:

```
SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;
```

> **Important**
>
> **SQL mode and user-defined partitioning.** Changing the server SQL mode after creating and inserting data into partitioned tables can cause major changes in the behavior of such tables, and could lead to loss or corruption of data. it is  strongly recommended that you never change the SQL mode once you have created tables employing user-defined partitioning.
>
> When replicating partitioned tables, differing SQL modes on master and slave can also lead to problems. For best results, you should always use the same server SQL mode on the master and on the slave.
>
> See Section 22.6, "Restrictions and Limitations on Partitioning", for more information.

## Full List of SQL Modes

* **ANSI\_QUOTES **

 Treat " as an identifier quote character\(like the \` quote character\) and not as a string quote character. You can still use to quote identifiers with this mode enabled. With **ANSI\_QUOTES** enabled, you cannot use double quotation marks to quote literal strings, because it it interpreted as an identifier.



