# 4.2.6 Using Option Files
Most MySQL programs can read startup options from option files(sometimes called configuration files).Option files provide a convenient way to specify commonly used options so that they need not be entered on the command line each time you run a program.

To determine whether a program reads option files, invoke it with the --help option.（For mysqld, use --verbose and --help) If the program reads option files, the help message indicates which files it looks for and which option groups it recognizes.

>注
>
> A MySQL program started with the **--no-defaults** option reads no option files other than **.mylogin.cnf**

Many option files are plain text files, created using any text editor. The exception is the **.mylogin.cnf** file that contains login path options. This is an encrypted file created by the mysql\_config\_editor utility.[See Section 4.6.6, "mysql\_config\_editor - MySQL Configuration Utility"](/466-mysqlconfig-editor-mysql-configuration-utility.md). A "login path" is an option group that permits only certain options: **host**, **user**, **password**, **port** and **socket**. Client programs specify which login path to read from **.mylogin.cnf** using the **--login-path** option.

To specify an alternative login path file name, set the **MYSQL\_TEST\_LOGIN\_FILE** environment variable.This variable is used by the **mysql-test-run.pl** testing utility, but also is recognized by **mysql\_config\_editor** and by MySQL clients such as **mysql**, **mysqladmin**, and so forth.

MySQL looks for option files in the order described in the following discussion and reads any that exist. If an option file you want to use does not exist, create it using the appropriate method, as just discussed.

> Note
>
> Option files used with NDB Cluster programs are covered in Section 23.3, "Configuration of NDB Cluster".

On Unix and Unix-like systems, MySQL programs read startup options from the files shown in the following table, in the specified order(top files are read first, files read later take precedence)

> NOTE
>
> On unix platforms, MySQL ignores configuration files that are wolrd-writable. This is intentional as security measure.

Table 4.2 Option Files Read on Unix and Unix-Like Systems

|File Name| Purpose|
|----|----|
|/etc/my.cnf|Global options|
|/etc/mysql/my.cnf|Global options|
|SYSCONFDIR/my.cnf|Global options|
|$MYSQL\_HOME/my.cnf|Server-specific options(server only)|
|defaults-extra-file|The file specified with **--defaults-extra-file**, if any|
|~/.my.cnf|User-specific options|
|~/.mylogin.cnf|User-specific login path options(clients only)|

In the preceding table, **~** represents the current user's home directory(the value of **$HOME**).

**SYSCONFDIR** represents the directory specified with the **SYSCONFDIR** option to **CMake** when MySQL was built. By default, this is the **etc** directory located under the compiled-in installation directory.

**MYSQL\_HOME** is an environment variable containing the path to the directory in which the server-specific **my.cnf** file resides. If **MYSQL\_HOME** is not set and you start the server using the **mysql\_safe** program, **mysqld\_safe** sets it to **BASEDIR**, the MySQL base installation directory.

**DATADIR** is commonly **/usr/local/mysql/data**, although this can vary per platform or installation method. The value is the data directory location built in when MySQL was  compiled, not the location specified with the **--datadir** option when **mysqld** starts. Use of **--datadir** at runtime has no effect on where the server looks for option files that it reads before processing any options.

If multiple instances of a give option are found, the last 

Empty lines in option files are ignored. Nonempty lines can take any of the following forms:
* **#comment, ;comment**
 Comment lines start with **#** or **;** A **#** comment can start in the middle of a line as well.
 
* **[group]**
 **_group_** is the name of the program or group for which you want to set options. After a group line, any option-setting lines apply to the named group until the end of the option file or another group line is given. Option group names are not case sensitive.
 
 * **opt\_name**
 
   This is equivalent to **--option\_name** on the command line.
 * **opt\_name=value**
 
   This is equivalent to **--opt\_name=value** on the command line. In an option file, you can have spaces around the **=** character, something that is not true on the command line. The value optionally can be enclosed within single quotation marks or double quotation marks, which is useful if the value contains a **#** comment character.

Leading and trailing spaces are automatically delete from option names and values.

You can use the escape sequences **\b**, **\t**, **\n**, **\r**, **\\**, and **\s** in option values to represent the backspace, tab, newline, carriage return, backslash, and space characters. In option files, these escaping rules apply:
* A backslash followed by a valid escape sequence character is converted to the character represented by the sequence. For example, **\s** is converted to a space.

* A backslash not followed by a valid escape sequence character remains unchanged. For example, **\S** is retained as is.

The preceding rules mean that a literal backslash can be given as **\\\\**, or as **\\** if it is not followed by a valid escape sequence character.

The rules for escape sequences in option files differ slightly from the rules for escape sequences in string literals in SQL statements. In the latter context, if "x" is not a valid escape sequence character, **\x** becomes "**x**" rather than **\x**. [See Section 9.1.1, "String Literals"](/911-string-literals.md).

The escaping rules for option file values are especially pertinent for Windows path names, which use **\**  as a path name separator. A separator in a Windows path name must be written as **\\** if it is followed by an escape sequence character. It can be written as **\\\\** or **\\** if it not. Alternatively, **/** may be used in Windows path names and will be treated as **\\**. Suppose that you want to specify a base directory of **C:\Program Files\MySQL Server 5.7** in an option file. This can be done several ways. Some examples:


```
basedir = "C:\Program Files\MySQL\MySQL Server 5.7"
basedir = "C:\\Program Files\\MySQL\\MySQL Server 5.7"
basedir = "C:/Program Files/MySQL/MySQL Server 5.7"
basedir = C:\\Program\sFiles\\MySQL\\MySQL\sServer\s5.7
```

If an option group name is the same as a program name, options in the group apply specifically to that program. For example, the **[mysqld]** and **[mysql]** groups apply to the **mysqld** server and the **mysql** client program, respectively.

The **[client]** option group is read by all client programs provided in **MySQL** distributions(but not by mysqld). To understand how third-party client programs that use the C API can use option files, see the C API documentation at [Section 27.8.7.50, "mysql_options()"](a).

The **[client]** group enables you to specify options that apply to all clients. For example, **[client]** is the appropriate group to use to specify the password for connection to the server.(But make sure that the option file is accessible only by yourself, so that other people cannot discover your password.) Be sure not to put an option in the **[client]** group unless it is recognized by all client programs that you use. Programs that do not understand the option quit after displaying an error message if you try to run them.

List more general option groups first and more specific groups later. For example, a **[client]** group is more general because it is read by all client programs, whereas a **[mysqldump]** group is read only by **mysqldump**. Options specified later override options specified earlier, so putting the option groups in the order **[client]**, **[mysqldump]** enables **mysqldump**-specific options to override **[client]** options.

Here is a typical global option file:


```
[client]
port=3306
socket=/tmp/mysql.sock

[mysqld]
port=3306
socket=/tmp/mysql.sock
key_buffer_size=16M
max_allowed_packet=8M

[mysqldump]
quick
```

Here is a typical user option file:


```
[client]
# The following password will be sent to all standard MySQL clients
password = "my password"

[mysql]
no-auto-rehash
connect_timeout=2
```
To create option groups to be read only by **mysql** servers from specific MySQL release series, use groups with names of **[mysqld-5.6]**,**[mysqld-5.7]**, and so forth. The following group indicates that the sql\_mode should be used only by MySQL servers with 5.7.x version numbers:


```
[mysql-5.7]
sql_mode=TRADITIONAL
```
It is possible to use **!include** directives in option files to include other option files and **!includedir** to search specific directories for option files. For example, to include the **/home/mydir/myopt.cnf** file, use the following directive:


```
!include /home/mydir/myopt.cnf
```
To search the **/home/mydir** directory and read option files found there, use this directive:


```
!includedir /home/mydir
```
MySQL makes no guarantee about the order in which option files in the directory will be read.

>NOTE
>
>Any files to be found and included using the **!includedir** directive on Unix operation systems must have file names ending in **.cnf**. On Windows, this directive checks for files with the **.ini** or **.cnf** extension.

Write the contents of an included option file like any other option file. That is, it should contain groups of options, each preceded by a **[group]** line that indicates the program to which the options apply.

While an included file is being processed, only those options in groups that the current program is looking for are used. Other groups are ignored. Suppose that a **my.cnf** file contains this line:


```
!include /home/mydir/myopt.cnf
```
And suppose that **/home/mydir/myopt.cnf** looks like this:


```
[mysqladmin]
force

[mysqld]
key_buffer_size=16M
```
If **my.cnf** is processed by **mysqld**, only the **[mysqld]** group in **/home/mydir/myopt.cnf** is used. If the file is processed by **mysqladmin**, only the [mysqladmin] group is used. If the file is processed by any other program, no options in **/home/mydir/myopt.cnf** are used.

The **!includedir** directive is processed similarly except that all option files in the named directory are read.

If an option file contains **!include** or **!includedir** directives, files named by those directives are processed whenever the option file is processed, no matter where they appear in the file













