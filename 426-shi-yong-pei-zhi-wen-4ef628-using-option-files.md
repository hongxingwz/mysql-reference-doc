# 使用配制文件(Using Option Files)

绝大多数的MySQL程序可以从选项文件中读取启动选项(有些时间也叫配制文件)。选项文件提供了一个方便的方式来指定通常使用的选项，因此你不用每次在运行程序时在命令行中输入命令。

要发现是否一个程序读取配制文件时，使用`--help`选项唤醒他(对于`mysqld`，使用`--verbose` 和 `--help`)。如果程序读取配制文件，帮助信息会声明其会查找哪些配制文件和其会识别哪些选项组。

>注意：一个MySQL程序以`--no-defaults`开始时，不读取任务选项文件除了`.mylogin.cnf.`

许多选项文件都是普通的文本文件，使用任何文本编辑器创建。但包含登陆路径信息的`.mylogin.cnf`是个例外。此文件是由`myql_config_editor`工具创建的编码的文件。参阅**Section 4.6.6, "mysql\_config\_editor - MySQL配制工具"**。"login path"是一个选项组仅允许确定的选项host, user, password, port 和socket。客户端程序可以指定从哪里读取`.mylogin.cnf`使用`--login-path`选项。