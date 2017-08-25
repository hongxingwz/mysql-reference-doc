# 11.2.2 Fixed-Point Types\(Exact Value\) - DECIMAL, NUMERIC

精度precision\(field length  ----&gt; 字段长度\)       范围scale\(decimal places ---&gt; 小数位位数\)

DECIMAL和NUMBERIC类型存储精确的数值类型。这些数据类型被使用当保持数据的准确性非常重要时，例如货币数据。在MySQL中，NUMERIC由DECIMAL实现，因此下面关于DECIMAL的讨论也同样适用于NUMERIC。

MySQL以二进制的格式存储DECIMAL。查看12.21节，“Precision Math”

在DECIMAL列中的声明，精度和范围可以被指定；例如:

```
salary DECIMAL(5, 2)
```

在上面的例子中，5是精度2是范围。精度表示存储值的有效位位数，范围是小数点后可以存储的位数。

标准SQL要求DECIMAL\(5,2\)有能力存储5个数字和2个小数位的任何值，因此在salary列可以存储值的范围是-999.99 - 999.99。

在标准的SQL中，DECIMAL\(M\)与DECIMAL\(M, 0\)是等价的。相似的，DECIMAL与DECIMAL\(M, 0\)是等价的，由实现决定M的值。MySQL支持这两种DECIMAL变种的语法。M默认的值是10。

如果范围是0，DECIMAL值不包含小数点或小数部分

DECIMAL数值位的最大数量是65，但指定的DECIMAL列真实的范围受列的精度和范围的限制。当小数位的范围超出指定的范围时，超出的部分通常会被截取\(在通常的操作系统上\)

