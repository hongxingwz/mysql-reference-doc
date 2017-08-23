# Loading Data into a Table

在创建好表后，你需要直充它。LOAD DATA和INSERT语句在干这件事情上很有用。

假设你的宠物记录可以描述成以下形式\(显而易见的MySQL期望日期以'YYYY-MM-DD'的格式出现；这可能与你使用的不同\)

| name | owner | species | sex | birth | death |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Fluffy | Harold | cat | f | 1993-02-04 |  |
| Claws | Gwen | cat | m | 1994-03-17 |  |
| Buffy | Harold | dog | f | 1989-05-13 |  |
| Fang | Benny | dog | m | 1990-08-27 |  |
| Bowser | Diane | dog | m | 1979-08-31 | 1995-07-29 |
| Chirpy | Gwen | bird | f | 1998-09-11 |  |
| Whistler | Gwen | bird |  | 1997-12-09 |  |
| Slim | Benny | snake | m | 1996-04-29 |  |

因为你以一个空表开妈，一种容易的方法来填充它是创建一个文本文件\(一行代表你的一个动物\)，然后加载你的文件到表中使用一个简单的语句。

你可以创建一个文本文件pet.txt包含每行一条记录，value值用tab分割，以CREATE TABLE语句定义的列顺序。对于缺失的值\(例如未知的性别或仍活的动物的死亡日期\)，你可以使用NULL值。在文本文件里代表这些值，使用\N。例如Whistler的bird的记录看起来像这样\(值之间的空白是一个单一的tab字符\):

```
Whistler    Gwen    bird    \N    1997-12-09    \N
```

加载文本文件pet.txt到pet表中，使用下面的语句：

```
mysql> LOAD DATA LOCAL INFILE '/path/pet/txt' INTO TABLE pet;
```

如果你在Windows上用编译器创建的文件以\r\n作为行的分割符，你应该使用下面的语句作为替代

```
mysql> LOAD DATA LOCAL INFILE '/path/pet.txt' INTO TABLE pet 
-> LINES TERMINATED BY '\r\n';
```

\(在一个运行OS X的台苹果机器，你将会想使用LINES TERMINATED BY '\r'\)

你可以显式的指定列分割符和换行符在LOAD DATA语句中如果你希望的话，但默认的是tab和换行符。这些对于正确的读取pet.txt信息就已经足够了。

如果语句失败了，可能是你的MySQL安装默认没有读取本地文件的能力。参阅Section 6.1.6 "Security Issues with LOAD DATA LOCAL",获取改变此项的信息。

当你想添加新的记录时，INSERT语句是很有用的。用其最简单的形式，你提供每列的值，以创建表时列的顺序。假设Diane获得一个新的hamster名字叫"Puffball"，你可以使用INSERT语句添加一个新的记录像下面这样:

```
INSERT INTO pet
    -> values ("Puffball", "Diane", "hamster", "f", "1993-03-30", null);
```

字符串和日期值用引号包围的字符串来表示。用INSERT，你可以插入NULL值直接的来代表缺失的值。你不用使用\N像在LOAD DATA中那样。

在此例中，在使用INSERT语句比LOAD DATA更多的录入涉及加载您的记录开始

