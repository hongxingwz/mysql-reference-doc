# 13.3.2 Statements That Cannot Be Rolled Back
Some statements cannot be rolled back. In general, these include data definition language(DDL) statement, such as those that create or drop databases, those that create, drop, or alter tables or stored routines.

You should design your transactions not to include such statements. If you issue a statement early in a transaction that cannot be rolled back, and then another statement later fails, the full effect of the transaction cannot be rolled back in such cases by issuing a **ROLLBACK** statement