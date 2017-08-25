# 11.2 Numeric Types

MySQL支持所有的标准SQL数据类型。这些类型包括准确数值类型\(INTEGER, SMALLINT, DECIMAL和NUMERIC\),也包括大约的数值类型\(FLOAT, REAL, DOUBLE PRECISION\)。关键字INT与INTEGER是同义词。关键字DEC和FIXED是DECIMAL同意词。MySQL把DOUBLE看作DOUBLE PRECISION的同意词\(不标准的扩展\).MySQL也把REAL看作DOUBLE PRECISION的同义词\(不标准的扩展\)，除非REAL\_AS\_FLOAT模式被激活。

BIT数据类型存储bit值由MyISAM, MEMORY, InnoDB,和NDB表。

获取assignment of out-of-range values to columns 和 overflow在表达式执行时，MySQL怎样处理，查看11.2.6章 "Out-of-Range and Overflow Handling"

获取数值类型存储要求，查看11.8章 ”Data Type Storage Requirements“

The data type used for the result of a calculation on numeric operands depends on the types of the operands and the operations performed on them. For more information, see Section 12.6.1, “Arithmetic Operators”.



