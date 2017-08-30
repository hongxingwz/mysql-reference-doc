# Choosing the Right Type for a Column
为了最理想的存储，你应该在所有情况下努力尝试使用最精确的类型。例如，如果一列用于存储1-99999的范围, MEDIUMINT UNSIGEND是最好的类型之一。对于所有能代表需要值的类型，此类型使用最少的存储空间。

All basic calculations(+, -, *, /) with DECIMAL columns are done with precision of 65 decimal(base 10) digits. Section 11.1.1, "Numberic Type Overview"。

如果精度不是特别的重要或者速度是最高的优先级，DOUBLE类型就已经足够好了。对于精度很重要，你可以把定点类型转换为BIGINT。这可以使以用64位的整数作运算然后将结果转换为需要的浮点数如果需要的话。