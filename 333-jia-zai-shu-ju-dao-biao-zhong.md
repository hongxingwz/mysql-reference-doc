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



