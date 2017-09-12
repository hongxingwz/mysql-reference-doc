# 13.7.4.3 SET NAMES Syntax

![](/assets/1505177677845.png)This statement sets the three session system variables `character_set_client`, `character_set_connection` and `character_set_results` to the given character set. Setting `character_set_connection` to `charset_name` also sets `collation_connection` to the default collation for `charset_name`. See Section 10.1.4, "Connection Character Sets and Collations".

The optional COLLATE clause may be used to specify a collation explicitly. If given, the collation must one of the permitted collations for `charset_name`.

`charset_name` and `collation_name` may be quoted or unquoted.

The default mapping can be restored by using a value of **DEFAULT**. The default depends on the server configuration.

usc2, utf16, and utf32 cannot be used as a client character set, which means that they do not work for **SET NAMES**

