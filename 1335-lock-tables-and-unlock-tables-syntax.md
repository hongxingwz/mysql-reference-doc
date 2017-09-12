# 13.3.5 LOCK TABLES AND UNLOCK TABLES Syntax

![](/assets/1505214347506.png)MySQL enables client sessions to acquire table locks explicitly for the purpose of cooperating with other sessions for access to tables, or to prevent  other sessions from modifying tables during periods when a session requires exclusive access to them. A session can acquire or release lock only for itself. One session cannot acquire locks for another session or release locks held by another session.

**LOCK TABLES** explicitly acquires table locks for the current client session. Table locks can be acquired for base tables or views. You must have the** LOCK TABLES** privilege, and the SELECT privilege for each object to be locked.

For view locking, **LOCK TABLES** adds all base tables used in the view to the set of tables to be locked and locks them automatically. If you lock a table explicitly with **LOCK TABLES**, any tables used in triggers are also locked implicitly, as described in Section 13.3.5.2,  "LOCK TABLES and Triggers".

UNLOCK TABLES explicitly releases any table locks held by the current session. LOCK TABLES implicitly releases any table locks held by the current session before acquiring new locks.

Another use for UNLOCK TABLES is to release the global read lock acquired with the FLUSH TABLES WITH READ LOCK statement, which enables you to lock all tables in all databases. See Section 13.7.6.3, "FLUSH Syntax".\(This is a very convenient way to get backups if you have a file system such as Veritas that can take snapshots in time.\)

A table lock protects only against inappropriate reads or writes by other sessions. A session holding a **WRITE **lock can perform table-level operations such as** DROP TABLE **or **TRUNCATE TABLE. **For sessions holding a **READ** lock, **DROP TABLE** and **TRUNCATE TABLE** operations are not permitted.

The following discussion applies only to non-**TEMPORARY** tables. **LOCK TABLES **is permitted\(but ignored\) for a **TEMPORARY** table. The table can be accessed freely by the session within which it was created, regardless of what other locking may be in effect. No lock is necessary because no other session can see the table.

For information about other conditions on the use of **LOCK TABLES **and statements that cannot be used while** LOCK TABLES, **see Section 13.3.5.3, "Table-Locking Restrictions and Conditions"

**Rules for Lock Acquisition**

to acquire table lock within the current session, use the **LOCK TABLES** statement. The following lock types are available:

READ \[LOCAL\] lock:

* The session that holds the lock can read the table \(but not write it\).
* Multiple sessions can acquire a READ lock for the table at the same time.
* Other sessions can read the table without explicitly acquiring a **READ** lock.
* The **LOCAL** modifier enables nonconflicting INSERT statements \(concurrent inserts\) by other sessions to execute while the lock is held. \(See Section 8.11.3, "Concurrent Inserts"\). However, **READ LOCAL** cannot be used if you are going to manipulate the database using processes external to the server while you hold the lock. For InnoDB tables, **READ LOCAL** is the same as **READ**

\[LOW\_PRIORITY\] WRITE lock:

* The session that holds the lock can read and write the table.
* Only the session that holds the lock can access the table. No other session can access it until the lock is released.
* Lock requests for the table by other sessions block while the **WRITE** lock is held.
* The **LOW\_PRIORITY** modifier has no effect. In previous versions of MySQL, it affected locking behavior, but this is no longer true. It is now deprecated and its use produces a warning. Use **WRITE** without **LOW\_PRIORITY** instead.

If the **LOCK TABLES** statement must wait due to locks held by other sessions on any of the tables.it blocks until all locks can be acquired.

A session that requires locks must acquire all the locks that it need in a single **LOCK TABLES** statement. While the locks thus obtained are held, the session can access only the locked tables. For example, in the following sequence of statements, an error occurs for the attempt to access **t2** because it was not locked in the **LOCK TABLES** statement:

```
mysql> LOCK TABLES t1 READ;$$
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT COUNT(*) FROM t1;$$
+----------+
| COUNT(*) |
+----------+
|        8 |
+----------+
1 row in set (0.00 sec)

mysql> SELECT COUNT(*) FROM t2;$$
ERROR 1100 (HY000): Table 't2' was not locked with LOCK TABLES
mysql>
```

Tables in the **INFORMATION\_SCHEMA** database are an exception. They can be accessed without being locked explicitly even while a session holds table locks obtained with **LOCK TABLES**.

You cannot refer to locked table multiple times in a single query using the same name. Use aliases instead, and obtain a separate lock for the table and each alias:

```
LOCK TABLE t1 write, t1 as t11 READ;
INSERT INTO t1 name SELECT name FROM t1;
ERROR 1100: Table 't' was not locked with LOCK TABLES
INSERT INTO t1 name SELECT name FROM t11;
```

The error occurs for the first **INSERT** because there are two references to the same name for a locked table. The second **INSERT** succeeds because the references to the table use different names.

If you statements refer to a table by manes of an alias, you must lock the table using that same alias. It does not work to lock the table without specifying the alias:

```
mysql> LOCK TABLE t1 READ;
    -> SELECT * FROM t1 AS myalias;$$
ERROR 1100 (HY000): Table 'myalias' was not locked with LOCK TABLES
```

**WRITE** locks normally have higher priority than **READ** locks to ensure that updates are processed as soon as possible. This means that if one session obtains a **READ** lock and then another session requests a **WRITE** lock, subsequent READ lock requests wait until the session that requested the **WRITE** lock has obtained the lock and released it.

**LOCK TABLES** acquires locks as follows:

* Sort all tables to be locked in an internally defined order. From the user standpoint, this order is undefined.
* If a table is to be locked with a read and a write lock, put the write lock request before the read lock request.
* Lock one table at a time until the session gets all locks.

This policy ensures that table locking is deadlock free.

> NOTE
>
> **LOCK TABLES** or **UNLOCK TABLES**, when applied to a partitioned table, always locks or unlocks the entire table; these statements do not support partition lock pruning. See Section 22.6.4, "Partitioning and Locking".

When the table locks held by a session are released, they are all released at the same time. A session can release its locks explicitly, or locks may be release implicitly under certain conditions.

* A session can release its locks explicitly with **UNLOCK TABLES**.

* If a session issues a LOCK TABLES statement to acquire a lock while already holding locks, its existing locks are released implicitly before the new locks are granted.

* If a session begins a transaction\(for example, with **START TRANSACTION**\), an implicit **UNLOCK TABLES** is performed, which causes existing locks to be released.\(For additional information about the interaction between table locking and transactions, see Section 13.3.5.1, "Interaction of Table Locking and Transactions".\)

If the connection for a client session terminates,  whether normally or abnormally, the server implicitly releases all table locks held by the session\(transactional and nontransactional\). If the client reconnects, the locks will no longer be in effect. In addition, if the client had an active transaction, the server rolls back the transaction upon disconnect, and if reconnect occurs, the new session begins with autocommit enabled. For this reason, clients may wish to disable auto-reconnect. With auto-reconnect in effect. the client is not notified if reconnect occurs but any table locks or current transaction will have bee lost. With auto-reconnect disabled. If the connection drops,  an error occurs for the next statement issued. The client can detect the error and take appropriate action such as reacquiring the locks or redoing the transaction. See Section 27.8.20, "C API Automatic Reconnection Control"

> Note
>
> If you use **ALTER TABLE** on a locked table, it may become unlocked. For example, if you attempt a second **ALTER TABLE** operation, the result may be an error Table 'tbl_name' was not locked with **LOCK TABLES**. To handle this, lock the table again prior to the second alteration.  See also Section B.5.6.1, "Problems with ALTER TABLE".





