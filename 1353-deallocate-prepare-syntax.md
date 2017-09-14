# 13.5.3 DEALLOCATE PREPARE Syntax

![](/assets/1505375048806.png)To deallocate a prepared statement produced with PREPARE, use a **DEALLOCATE PREPARE** statement that refers to the prepared statement name. Attempting to execute a prepared statement after deallocating it results in an error. If too many prepared statements are created and not deallocated by either the **DEALLOCATE PREPARE** statement or the end of the session, you might encounter the upper limit enforced by the **max\_prepared\_stmt\_count** system variable.

For examples, see [Section 13.5, "Prepared SQL Statement Syntax"](/135-prepared-sql-statement-syntax.md).



