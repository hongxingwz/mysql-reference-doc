
* BINARY([N]) 
产生一个BINARY数据类型。参阅11.4.2节, "The BINARY 和 VARBINARY Types"来描述怎样影响比较。如果可选的长度N被指定，BINARY(N)会造成cast使用不超过N bytes的参数。值少于N bytes的用0x00填充到指定的长度N
* CHAR[(N)]\[charset_info\]
用CHAR产生一个字符串。如果可选的长度N被指定，CHAR(N)造成将不超过N字符的参数转换。少于N字符的值不会发生填充。
没有_charset\_info_语句，CHAR产生一个肯有默认字符集的字符。显示的指定字符集，这些_charset\_info_值将会允许：
    * CHARACTER SET charset\_name：来产生最有指定字符集的字符串
    * ASCII:CHARACTER SET latin1的简写
    * UNICODE:CHARACTER SET ucs2的简写
    
 所有的情况，字符串对于字符集具有默认的检验集
* DATE
 
  产生一个DATE值
* DECIMAL[(M[, D])] 

 产生一个DECIMAL值。如果指定M和D被指定，他们指定了precision, scale
* JSON(added in MySQL 5.7.8)

 产生一个JSON值，在JSON和其他类型的转换，参阅Comparision and Ordering of JSON Values
* NCHAR[(N)]
 像CHAR，但产生一个具有national character set的字符串。参阅10.1.3.7节，"The National Character Set"
 不像CHAR，NCHAR不允许尾部字符设置信息
* SIGNED [INTEGER]    生产一个有符号整形值
* TIME    产生一个TIME值
* UNSIGEND [INTEGER]    产生一个无符号整形