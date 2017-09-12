# 13.7.4.2 SET CHARACTER SET Syntax

![](/assets/1505177090626.png)This statement maps all strings sent between the server and the current client with the given mapping. `SET CHARACTER SET` sets three session system variables: `character_set_client` and `character_set_results` are set to the given character set, and `character_set_connection` to the value of `character_set_database`. See Section 10.1.4, "Connection Character Sets and Collations".

`charset_name` may be quoted or unquoted.

The default character set mapping can be restored by using the value **DEFAULT**. The default depends on the server configuration.

**ucs2**, **utf16**, and **utf32** cannot be used as a client character set, which means that they do not work for `SET CHARACTER SET`



