# CURRENT\_USER\(\), CURRENT\_USER\(\)

Returns the user name and host name combination for the MySQL account that the server used to authenticate the current client. This account determines your access privileges. The return value is a string in the **utf8** character set.

The value of **CURRENT\_USER\(\)** can differ from the value of **USER\(\)**

```
mysql> SELECT USER();
    -> 'davida@localhost'
mysql> SELECT * FROM mysql.user;
ERROR 1044: Access denied for user ''@'localhost' to database 'mysql'
mysql> SELECT CURRENT_USER();
    -> '@localhost'
```

The example illustrates that although the client specified a user name of **davida**(as indicated by the value of **USER()** function), the server authenticated the client using an anonymous user  account(as seen by the empty user name part of the **CURRENT_USER()** value). One way this might occur is that there is no account listed in the grant tables for **davida**.

Within a stored program or view, **CURRENT\_USER()** returns the account for the user who defined the object (as given by its **DEFINER** value) unless defined with the **SQL SECURITY INVOKER** characteristic. In the latter case, **CURRENT\_USER()** returns the object's invoker.

Triggers and events have on option to define the **SQL SECURITY** characteristic, so for these objects, **CURRENT\_USER()** returns the account for the user who defined the object. To return the invoker, use **USER()** or **SESSION_USER()**.

The following statements support use of the **CURRENT\_USER()** function to take the place of the name (and, possibly, a host for) an affected user or a definer; in such cases, CURRENT\_USER() is expanded where and as needed:
