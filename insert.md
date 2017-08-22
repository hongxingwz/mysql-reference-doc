Insert是sql中常用语句，Insert INTO table\(field1, field2, ...\) values \(value1, value2, ...\)这种形式在应用程序开发中必不可少。但我们在开发，测试过种中，经常会遇到需要复制的情况，如将一个tabl1的数据的部分字段复制到table2中,或者将整个table1复制到table2中，这时候我们就要使用SELECT INTO 和 INSERT INTO SELECT表复制语句了。

