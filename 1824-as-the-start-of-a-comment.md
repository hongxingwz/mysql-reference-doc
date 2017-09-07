# 1.8.2.4 '--' as the Start of a Comment

Standard SQL uses the C syntax /\* this is a comment \*/ for comments, and MySQL Server supports this syntax as well. MySQL also support extensions to this syntax that enable Mysql -specific SQL to be embedded in the comment, as described in Section 9.6, "Comment Syntax"

Standard SQL use "--" as a start-comment sequence. MySQL Server uses **#** as the start comment character. MySQL also supports a variant of the **--** comment style. That is, the **--** start-comment sequence must be followed by a space(or by a control character such as a newline).The space is required to prevent problems with automatically generated SQL queries that use constructs such as the following, where we automatically insert the value of the payment for **payment**:


```
UPDATE account set credit=credit-payment
```
Consider about what happens if **payment** has a negative value such as -1:


```
UPDATE account set credit=credit--1;
```
**credit--1** is a valid expression in SQL, but **--** is interpreted as the start of a comment, part of the expression is discarded. The result is a statement that has a completely different meaning than intended:


```
UPDATE account SET  credit = credit
```
The statement produces no change in value at all. This illustrates that permitting comments to start with -- can have serious consequences.

Using our implementation requires a space following the **--** for it to be recognized as a start-comment sequence in MySQL Server. Therefore, **credit--1** is safe to use.

Another safe feature is that the **mysql** command-line client ignores lines that start with --.




