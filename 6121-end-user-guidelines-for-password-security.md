# 6.1.2.1 End-User Guidelines for Password Security
MySQL users should use the following guidelines to keep passwords secure.

When you run a client program to connect to the MySQL server, it is inadvisable to specify your password in a way that exposes it to discovery by other users. The methods you can use to specify you password when you run client programs are listed here, along with an assessment of the risks of each method.In short, the safest methods are to have the client program prompt for the password or to specify the password in a properly protected option file.
* Use the **mysql\_config\_editor** utility, which enables you to store authentication credentials in an encrypted login path file named  **.mylogin.cnf**. The file can be read later by MySQL client programs to obtain authentication credentials for connecting to MySQL Server. See Section 4.6.6, ["mysql\_config\_editor - MySQL Configuration Utility"]().