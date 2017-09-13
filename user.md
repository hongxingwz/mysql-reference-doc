# USER()
Returns the current MySQL user name and host name as a string in the **utf8** character set.


```
mysql> select  USER();
    -> 'yunhemysql@121.69.13.130'
```
The value indicates the user name you specified when connecting to the server, and the client host from which you connected. The value be different from that of CURRENT_USER().

