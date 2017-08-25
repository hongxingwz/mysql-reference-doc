# 11.2.3 Floating-Point Types\(Approximate Value\) -FLOAT, DOUBLE

FLOAT和DOUBLE类型代表了不精确的数值类型，MySQL用4字节来存储单精度数值类型8字符来存储双精度数值类型。

对于FLOAT，SQL标准允许可选的对精度\(但不是范围\)的说明以FLOAT关键字后接圆括号的形式。MySQL也支持这种可选的精度说明书，但是精度仅用来决定存储的范围，\(0-23\)的精度以4字节单精度的形式存储，（24-53\)的结果以8字节双精度的形式存储。

MySQL允许不标准的语法:FLOAT\(M, D\) 或 REAL\(M, D\) 或 DOUBLE PRECISION\(M, D\)。\(M，D\)表示数值可以存储的最大位数是M，在小数点后的最大存储位数是D。例如，定义为FLOAT\(7,4\)当显示时看起来像-999.9999。MySQL在存储这种类型值时将会执行rounding，因此如果你插入999.00009到FLOAT\(7,4\)列中大约的结果可能是999.0001。

因为浮点数值是大约的并不作为精确值存储，尝试作为精度值来比较他们可能导致一些问题。这有关于平台或实现依赖的主题。获取更多的信息,查看B5.4.8节 “Problems with Floating-Point Values”

