# 13.6.6.3 Cursor FETCH Syntax

![](/assets/1504961848044.png)This statement fetches the next row for the **SELECT** statement associated with the specified cursor\(which must be open\), and advances the cursor pointer. If a row exists, the fetched columns are stored in the named variables. The number of columns retrieved by the **SELECT** statement must match the number of output variables specified in the **FETCTH **statement.

If no more rows are available, a No Data condition occurs with SQLSTATE value '02000'. To detect this condition, you can set up a handler for it\(or for a NOT FOUND condition\). For an example, see Section 13.6.6, "Cursors".

Be aware that another operation, such as a **SELECT** or another **FETCH**, may also cause the handler to execute by raising the same condition. If it is necessary to distinguish which operation raised the condition, place the operation within its own **BEGIN ... END** block so that it can be associated with its own handler.

