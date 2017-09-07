# 1.8.2.1 SELECT INTO TABLE Differences
MySQL Server doesn't support the **SELECT ... INTO TABLE** Sybase SQL . Instead, MySQL Server supports the **INSERT INTO ... SELECT** standard SQL syntax, which is basically the same thing. See Section 13.2.5.1, "INSERT ... SELECT Syntax". For example:


```
INSERT INTO tb1_temp2(fld_id)
    SELECT tb1_temp1.fld_order_id
    FROM tb1_temp1 WHERE tb1_temp1.fld_order_id > 100
```
Alternatively, you can use **SELECT ... INTO OUTFILE** or **CREATE TABLE ... SELECT**.
