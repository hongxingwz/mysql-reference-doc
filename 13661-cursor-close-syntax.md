# 13.6.6.1 Cursor CLOSE Syntax

![](/assets/1504961056607.png)This statement closes a previously opened cursor. For an example, see Section 13.6.6, "Cursors".

An error occurs if the cursor is not open.

If not closed explicitly, a cursor is closed at the end of the** BEGIN ... END **block in which it was declared.

