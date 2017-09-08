# 13.6.3 DECLARE Syntax
The **DECLARE** statement is used to defined various items local to program:
* Local variables. See Section 13.6.4, "Variables in Stored Programs".
* Conditions and handlers. See Section 13.6.7, "Condition Handling".
* Cursors. See Section 13.6.6, "Cursors".

**DECLARE** is permitted only inside a BEGIN ... END compound statement and must be at its start, before any other statements.

Declarations must follow a certain order. Cursor declarations must appear before handler declarations.Variable and condition declarations must appear before cursor or handler declarations. 