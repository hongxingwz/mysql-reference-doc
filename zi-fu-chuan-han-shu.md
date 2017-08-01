##UPPER和UCASE
返回字符串str，根据当前字符集映射(缺省是ISO-8859-1 Latin1)把所有的字符改变成大写。该函数对多字节是可靠的。

##LOWER和LCASE
返回字符串str，根据当前字符集映射(缺省是ISO-8859-1 Latin1)把所有的字符改变为小写。该函数对多字节是可靠的。
## 重复 
**REPEAT(str, count)**
返回由重复count次的字符串str组成的一个字符串。如果count <= 0，返回一个空字符串。如果str或count是NULL，返回NULL
## 反转
**REVERSE(str)**
返回颠倒字符顺序的字符串str

## 长度
**char\_length\(\)**
字符长度
如果参数为null， 返回null
**length\(\)**
字节长度
如果参数为null，返回null

## 去除空格
LTRIM(str) 去除左边的空格
RTRIM(str) 去除右边的空格
TRIM(str) 去除左右两边的空格


## 全并多个字符串，或者表中的多个字段
CONCAT(str1, str2, ...)
返回来自于参数连接的字符串。如果任何参数是NULL，返回NULL。可以有超过2个参数。一个数字参数被变换为等价的字符串形式。





