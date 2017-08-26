# 11.2.4 Bit-Value Type - BIT

BIT数值用来存储bit值。BIT\(M\)类型可以存储M个bit值，M的范围是\(1~64\)

可以用b'value'来指定bit值，value是用0和1组成的二进制值。例如, b'111'和b'1000000'代表7和128。查看9.1.5节 “Bit Value Literals”

如果你赋值给BIT\(M\)列的值的长度小于M，在值的左侧会以0填充。例如，把值b'101'赋值以BIT\(6\),事实上，b'000101'与其一样。

NDB Cluster：在指定NDB表中所有BIT列结合起来的大写不能超过4096bit

