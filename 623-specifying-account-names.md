# 6.2.3 Specifying Account Names
MySQL account names consist of a user name and a host name. This enables creation of accounts for users with the same name who can connect from different hosts. This section describes how to write account names, including special values and wildcard rules.

In SQL statements such as **CREATE USER**, **GRANT**, and **SET PASSWORD**, account names follow these rules:
* Account name syntax is 'user\_name'@'%'. For example, 'me' is equivalent to 'me'@'%'.

* The user name and host name need not be quoted if they are legal as unquoted identifiers. Quotes are necessary to specify a **user\_name** string containing special characters (such as space or **-**), or a **host\_name** string containing special characters or wildcard characters such as (**.** or **%**); for example, **'test-user'@'%.com'**.

* Quote user names and host names as identifiers or as strings, using either backticks, single quotation marks('), or double quotation marks ("). For string-quoting and identifier-quoting guidelines, see Section 9.1.1, "String Literals", and Section 9.2, "Schema Object Names".

* The user name and host name parts, if quoted, must be quoted separately. That is write 'me'@'localhost', not 'me@localhost'; the latter is actually equivalent to 'my@localhost'@'%'
* Other grant tables indicate privileges an account has for databases and objects within databases. These tables have **User** and **Host** columns to store the account name, Each row in these tables associates with the account in the **user** that has the same **User** and **Host** values
* For access-checking purposes, comparisons of User values are case sensitive. Comparisons of Host values are not case sensitive.

User names and host names have certain special values or wildcard conventions, as described following.

The user name part of an account name is either a nonblank value that literally matches the user name for incoming connection attempts, or a blank value (empty string) that matches any user name. An account with a blank user name is an anonymous user. To specify an anonymous user is SQL Statements, use a quoted empty user name part, such as ''@'localhost'.

The host name part of an account name can take many forms, and wildcards are permitted:
* A host value can be a host name or an IP address (IPv4 or IPv6). The name 'localhost' indicates the local host. The IP address '127.0.0.1' indicates the IPv4 loopback interface. The IP address '::1' indicates the IPv6 loopback interface. 
* The **%** and **_** wildcard characters are permitted in host name or IP address values. These have the same meaning as for pattern-matching operations performed with the **LIKE** operator. For example, a host value of '%' matches any host name, whereas a value of '%.mysql.com' matches any host in the **mysql.com** domain. '192.168.1.%' matches any host in the 192.168.1 class C network.

Because IP wildcard values are permitted in host values(for example, '192.168.1.%' to match every host on a subnet), someone could try to exploit this capability by naming a host **192.168.1.somewhere.com**. To foil such attempts, MySQL does not perform matching on host names that start with digits and  a dot.For example, if a host is named **1.2.example.com**, its name never matches the host part of account names. An IP wildcard value can match only IP addresses, not host names.

* For a host value specified as an IPv4 address, a netmask can be given to indicate how many address bits to use for the network number. Netmask notation cannot be used for IPv6 addresses.

 The syntax is _host\_ip/netmask_. For example: