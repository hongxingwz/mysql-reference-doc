# CONNECTION_ID()

Returns the connection ID(thread ID) for the connection. Every connection has an ID that is unique among the set of currently connected clients.

The value returned by **CONNECTION_ID()** is the same type of value as displayed in the **ID** column of the **INFORMATION\_SCHEMA.PROCESSLIST** table, and **Id** column of **SHOW PROCESSLIST** output, and the **PROCESSLIST\_ID** column of the Performance Schema **threads** tables.

##总结：
* INFORMATION_SCHEMA.PROCESSLIST.Id
* performance_schema.threads.PROCESSLIST\_ID 