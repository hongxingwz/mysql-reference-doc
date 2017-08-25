# 11.2.1 Integer Types\(Exact Value\) - INTEGER, INT, SMALLINT, TINYINT, MEDIUMINT, BIGINT

MySQL支持SQL标准整数值类型INTEGER\(或 INT\) 和 SMALLINT。作了对标准的扩展，MySQL支持整数类型TINYINT, MEDIUMINT, 和BIGINT。下表展示了存储要求和每种整形类型的范围。

| Type | Storage | Minimum Value | Maximum Value |
| :--- | :--- | :--- | :--- |
|  | \(Bytes\) | \(Signed/Unsigned\) | \(Signed/Unsigend\) |
| TINYINT | 1 | -128 | 127 |
|  |  | 0 | 255 |
| SMALLINT | 2 | -32768 | 32767 |
|  |  | 0 | 65536 |
| MEDIUMINT | 3 | -8388608 | 8388607 |
|  |  | 0 | 16777215 |
| INT | 4 | -2147483648 | 2147483647 |
|  |  | 0 | 4294967295 |
| BIGINT | 8 | -9223372036854775808 | 9223372036854775807 |
|  |  | 0 | 18446744073709551615 |



