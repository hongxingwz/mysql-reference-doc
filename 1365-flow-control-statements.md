# 13.6.5 Flow Control Statements
MySQL supports the **IF**, **CASE**, **ITERATE**, **LEAVE LOOP**, **WHILE**, and **REPEAT** constructs for flow control within stored programs. It also supports **RETURN** within stored functions.

Many of these constructs contains other statements, as indicated by the grammar specifications in the follwing sections, Such constructs may be nested. For example, an **IF** statement might contain a **WHILE** loop, which itself contains a **CASE** statement.

MySQL dose not support **FOR** loops.

