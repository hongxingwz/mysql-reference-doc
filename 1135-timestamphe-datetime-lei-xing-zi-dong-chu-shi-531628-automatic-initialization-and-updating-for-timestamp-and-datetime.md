# TIMESTAMP和DATETIME类型自动初始化(Automatic Initialization and Updating for TIMESTAMP and DATETIME)

TIMESTAMP 和 DATETIME 列可以自动初始化和更新到当前的日期和时间（也就是，当前的时间戳）。

对于一个表中的TIMESTAMP 或 DATETIME列来说，你可以赋予当前时间为默认值或更新值，或者全部。

* 如果该列没有指定值，一个自动初始化值的列会被设置为当前的时间戳

* 一个自动更新的列会自动更新到当前的时间戳，如果该行任何其它列的值从当前值改变成另外的值。自动更新列的值保持不变如果其它列的值设置为他们的当前值。为了阻止自动更新列自动更新，需要显示设置其值为当前值。要显示的更新自动更新的列的值即使其他列的值没有改变，显示的设置他应该有的值(例如，设置其值为CURRENT_TIMESTAMP)。

额外的，如果`explict_defaults_for_timestamp`系统变量被禁用，你可以初始化或更新作何TIMESTAMP(但不是DATETIME)列到当前日期和时间通过给其赋值NULL，除非该列被定义为NULL值允许NULL值。

指定自动属性，使用`DEFAULT CURRENT_TIMESTAMP` 和 `ON UPDATE CURRENT_TIMESTAMP`在列定义语句。语句的顺序是没问题的。如果都出现在列定义语句中，哪个都可以出现在第一。任何`CURRENT_TIMESTAMP`的同意词与`CURRENT_TIMESTAMP`的意思是一样的。这些同意词是`CURRENT_TIMESTAMP()`，`NOW()`, `LOCALTIME`, `LOCALTIME()`, `LOCALTIMESTAMP`和`LOCALTIMESTAMP()`,


使用`DEFAULT CURRENT_TIMESTAMP`和`ON UPDATE CURRENT_TIMESTAMP`会使用具体的`TIMESTAMP`和`DATETIME`。`DEFAULT`语句也可以指定一个常量(不是自动变化)的默认值；例如，`DEFAULT 0` 或 `DEFAULT '2000-01-01 00:00:00'`


>注意： 示例中使用`DEFAULT 0`，默认情况下能可能会产生警告或错误取决于是否是严格的SQL格式或`NO_ZERO_DATE`SQL模式是否被启用了。要注意`TRADITIONAL` SQL模式包括了严格模式和`NO_ZERO_DATE`。查看Section 5.1.8, "Server SQL MODES"


* 当同时有`DEFAULT CURRENT_TIMESTAMP`和`ON UPDATE CURRENT_TIMESTAMP`,此列使用当前的时间戳作为其默认值，当更新时自动更新成更新时间的时间戳。


```
mysql>  CREATE TABLE t1 (
    ->   ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    ->    dt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    -> );
```



* 当具有`DEFAULT`语句没有`ON UPDATE CURRENT_TIMESTAMP`语句时，该列被赋予默认值，更新时不会赋予当前的时间戳的值。
 默认值取决于`DEFAULT`语句是否指定了`CURRENT_TIMESTAMP`或一个常量。`CURRENT_TIMESTAMP`，默认是当前的时间戳。
 


```
mysql> CREATE TABLE t1 (
    ->   ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ->   dt DATETIME DEFAULT CURRENT_TIMESTAMP
    -> );
```

当设置默认值为一个常量时，默认是指定的值。在这种情况下，此列不再有自动变化的值



```
CREATE TABLE t1 (
  ts TIMESTAMP DEFAULT 0,
  dt DATETIME DEFAULT 0
);
```

当具有`ON UPDATE CURRENT_TIMESTAMP`语句和一个常量`DEFAULT`语句时，该更新时会设置




