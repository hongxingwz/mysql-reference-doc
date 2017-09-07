# 9.2.4 Function Name Parsing and Resolution

MySQL 5.7 supports built-in\(native\) functions, user-defined functions\(UDFs\), and stored functions. This section describes how the server recognizes whether the name of a built-in function is used as a function call or as an identifier, and how the server determines which function to use in cases when functions of different types exist with a given name.

## Built-In Function Name Parsing

The parser uses default rules for parsing names of built-in functions. These rules can be changed by enabling the IGNORE\_SPACE SQL mode.

When the parser encounters a word that is the name of a built-in function, it must determine whether the name signifies a function call or is instead a nonexpression reference to an identifier such a table or column name. For example, in the following statements, the first reference to **count** is a function call, whereas the second reference is a table name:

```
SELECT COUNT(*) FROM  mytable;
CREATE TABLE count(i INT);
```

The parser should recognize the name of a built-in function as indicating a function call only when parsing what is expected to be an expression. That is, in nonexpression context, function names are permitted as identifiers.

However, some built-in functions have special parsing or implementation considerations, so the parser uses the following rules by default to distinguish whether their names are being used as function calls or as identifiers in nonexpression context:

* To use the name as a function call in an expression, there must no whitespace between the name and following **\( **parenthesis character.
* Conversely, to use the function name as an identifier, it must not be followed immediately by a parenthesis.



The requirement that function calls be written with no whitespace between the name and the parenthesis applies only to the built-in functions that have special considerations. COUNT is one such name. The sql/lex.h source file list the names of these special functions for which following whitespace determines their interpretation: names defined by the **SYM\_FN\(\)  **macro in **symbols\[\] **array.

In MySQL 5.7, there are about 30 such function names. You may find it easiest to treat the no-whitespace requirement as applying to all function calls.

The following table names the functions that are affected by the **IGNORE\_SPACE**  setting and listed as special in the sql/lex.h source file.

| ADDDATE | BIT\_AND | BIT\_OR | BIT\_XOR |
| :--- | :--- | :--- | :--- |
| CAST | COUNT | CURDATE | CURTIME |
| DATE\_ADD | DATE\_SUB | EXTRACT | GROUP\_CONCAT |
| MAX | MID | MIN | NOW |
| POSITION | SESSION\_USER | STD | STDDEV |
| STDDEV\_POP | STDDEV\_SAMP | SUBDATE | SUBSTR |
| SUBSTRING | SUM | SYSDATE | SYSTEM\_USER |
| TRIM | VARIANCE | VAR\_POP | VAR\_SAMP |

For functions not listed as special in **sql/lex.h**, whitespace dose not matter. They are interpreted as function calls only when used in expression context and may be used  freely as identifiers otherwise. ASCII is one such name. However, for these nonaffected function names, interpretation may vary in expression context: func\_name\(\) is interpreted as built-in function if there is one with the give name; if not, func\_name () is interpreted as a user-defined function or stored function if one exists with that name.

The IGNORE\_SPACE SQL mode can be used to modify how the parser treats function names that are whitespace-sensitive:
* With IGNORE\_SPACE disabled, the parser interprets the name as a function call when there is no whitespace between the name and the following parenthesis.This occurs even when the function name is used in nonexpression context:


```
mysql> CREATE TABLE count(i INT);
mysql> CREATE TABLE count(i INT);
ERROR 1064 (42000): You have an error in your SQL syntax...
```
To eliminate the error and cause the name to be treated as an identifier, either use whitespace following the name or write it as quoted identifier(or both):


```
CREATE TABLE count (i INT);
CREATE TABLE `count`(i INT);
CREATE TABLE `count` (i INT);
```

* With IGNORE\_SPACE enabled, the parser loosens the requirement that there be no whitespace between the function name and the following parenthesis. This provides more flexibility in writing function calls. For example, either of the following function calls are legal:


```
SELECT COUNT(*) FROM mytable;
SELECT COUNT (*) FROM mytable;

```
However, enabling IGNORE\_SPACE also has the side effect the parser treats the affected function names as reserved words(see Section 9.3 "Keywords and Reserved Words"). This means that a space following the name on longer signifies its use as an identifier. The name can be used in function calls with or without following whitespace, but causes a syntax error in nonexpression context unless it it quoted.For example, with IGNORE\_SPACE enabled, both of the following statements fail with a syntax error because the parser interprets count as a reserved word:


```
CREATE TABLE count (i INT);
CREATE TABLE count(i INT);
```
To use the function name in nonexpression context, write it as a quoted identifier:


```
CREATE TABLE `count` (i INT);
CREATE TABLE `count`(i INT);

```
To enable the IGNORE\_SPACE SQL mode, use this statement:


```
SET sql_mode = 'IGNORE_SPACE';
```
IGNORE\_SPACE is also enabled by certain other composite modes such as **ANSI** that include it in their value:


```
SET sql_mode = 'ANSI'
```
Check Section 5.1.8,"Server SQL Modes", to see which composite modes enable INGORE\_SPACE

To minimize the dependency of SQL code on the IGNORE\_SPACE setting use these guidelines:
* Avoid creating UDFs or stored functions that have the same name as a built-in function.
* Avoid using function names in nonexpression context. For example, these statements use **count**(one of the affected function affected by IGNORE\_CASE),so they fail with or without whitespace following the name if IGNORE\_SPACE is enabled:


```
CREATE TABLE count(i INT);
CREATE TABLE count (i INT);

```
If you must use a function name in nonexpression context, write it as a quoted identifier:


```
CREATE TABLE `count`(i INT);
CREATE TABLE `count` (i INT);
```

## Function Name Resolution

















