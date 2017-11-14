# SQL PRIMARY KEY 约束

PRIMARY KEY约束唯一标识数据库表中的每条记录。  
主键必须包含唯一的值。  
主键列不能包含NULL值。  
每个表都应该有一个主键，并且每个表只能有一个主键。

---

## SQL PRIMARY KEY Constraint on CREATE TABLE

下面的SQL在“Persons”表创建时在“id\_P”列创建PRIMARY KEY约束

MYSQL：

```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (Id_P)
)
```

SQL SERVER / Oracle / MS Access:

```
CREATE TABLE Persons
(
Id_P int NOT NULL PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```

如果需要命名PRIMARY KEY 约束， 以及为多个列定义PRIMARY KEY约束，请使用下面的SQL语法：

```
CREATE TABLE Persons
(
Id_P int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
)
```

> 注：在MYSQL设置额外的CONSTRAINT貌似不起作用，主键的Key的名字还是PRIMARY

---

## SQL PRIMARY KEY Constraint on ALTER TABLE

如果在表已存在的情况下为“Id\_P”列创建PRIMARY KEY约束，请使用下面的SQL：

```
ALTER TABLE Persons
ADD PRIMARY KEY (Id_p)
```

如果需要命名PRIMARY KEY约束，以及为多个列定义PRIMARY KEY约束，请使用下面的SQL语法：

MySQL / SQL Server / Oracle / MS Access:

```
ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (Id_P,LastName)
```

> 注：如果您使用ALTER TABLE语句添加主键，必须把主键列声明为不包含NULL值\(在表首次创建时\)。

---

## 撤销PRIMARY KEY 约束

如需撤销PRIMARY KEY 约束，请使用下面的SQL：

MYSQL：

```
ALTER TABLE Persons
DROP PRIMARY KEY
```



SQL Server / Oracle / MS ACCESS:

```
ALTER TABLE Persons
DROP CONSTRAINT pk_PersonID
```





