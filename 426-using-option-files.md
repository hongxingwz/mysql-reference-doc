# 4.2.6 Using Option Files
Most MySQL programs can read startup options from option files(sometimes called configuration files).Option files provide a convenient way to specify commonly used options so that they need not be entered on the command line each time you run a program.

To determine whether a program reads option files, invoke it with the --help option.（For mysqld, use --verbose and --help) If the program reads option files, the help message indicates which files it looks for and which option groups it recognizes.

>注
>
> A MySQL program started with the **--no-defaults** option reads no option files other than **.mylogin.cnf**

Many option files are plain text files, created using any text editor. The exception is the **.mylogin.cnf** file that contains login path options. This is an encrypted file created by the mysql\_config\_editor utility.[See Section 4.6.6, "mysql\_config\_editor - MySQL Configuration Utility"](/466-mysqlconfig-editor-mysql-configuration-utility.md). A "login path" is an option group that permits only certain options: **host**, **user**, **password**, **port** and **socket**. Client programs specify which login path to read from **.mylogin.cnf** using the **--login-path** option.

To specify an alternative login path file name, set the **MYSQL\_TEST\_LOGIN\_FILE** environment variable.This variable is used by the **mysql-test-run.pl** testing utility, but also is recognized by **mysql\_config\_editor** and by MySQL clients such as **mysql**, **mysqladmin**, and so forth.

MySQL looks for option files in the order described in the folowing discussion and reads any that exist. If an option file youwant to use does not exist, create it using the appropriate method, as just discussed.