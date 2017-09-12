# 13.3.5 LOCK TABLES AND UNLOCK TABLES Syntax

![](/assets/1505214347506.png)MySQL enables client sessions to acquire table locks explicitly for the purpose of cooperating with other sessions for access to tables, or to prevent  other sessions from modifying tables during periods when a session requires exclusive access to them. A session can acquire or release lock only for itself. One session cannot acquire locks for another session or release locks held by another session.

**LOCK TABLES** explicitly acquires table locks for the current client session. Table locks can be acquired for base tables or views. You must have the** LOCK TABLES** privilege, and the SELECT privilege for each object to be locked.

For view locking, **LOCK TABLES** adds all base tables used in the view to the set of tables to be locked and locks them automatically. If you lock a table explicitly with **LOCK TABLES**, any tables used in triggers are also locked implicitly, as described in Section 13.3.5.2,  "LOCK TABLES and Triggers".

UNLOCK TABLES explicitly releases any table locks held by the current session. LOCK TABLES implicitly releases any table locks held by the current session before acquiring new locks.

Another use for UNLOCK TABLES is to release the global read lock acquired with the FLUSH TABLES WITH READ LOCK statement, which enables you to lock all tables in all databases. See Section 13.7.6.3, "FLUSH Syntax".\(This is a very convenient way to get backups if you have a file system such as Veritas that can take snapshots in time.\)

A table lock protects only against inappropriate reads or writes by other sessions. A session holding a **WRITE **lock can perform table-level operations such as** DROP TABLE **or **TRUNCATE TABLE. **For sessions holding a **READ** lock, **DROP TABLE** and **TRUNCATE TABLE** operations are not permitted.

