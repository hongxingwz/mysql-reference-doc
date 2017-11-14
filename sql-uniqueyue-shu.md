# SQL UNIQUE约束

UNIQUE约束唯一标识数据库表中的每条记录。

UNIQUE和PRIMARY KEY约束均为列或列集合提供了唯一性的保证

PRIMARY KEY拥有自动定义的UNIQUE约束

请注意，第一个表可以有多个UNIQUE约束，但是每个表只能有一个PRIMARY KEY约束


## SQL UNIQUE Constraint on CREATE TABLE
下面的SQL在“Persons”表创建时在“Id_P”列创建UNIQUE约束：

MYSQL：


```
CREATE TABLE Persons(
Id_P int not null,
LastName varchar(255) not null,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
unique (id_p)
)
```


SQL Server / Oracle / MS Access:

```
CREATE TABLE Persons(
Id_P int not null unique key,
LastName varchar(255) not null,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```

如果需要命名UNIQUE约束，以及为多个列定义UNIQUE约束，请使用下面的SQL语法：


```
CREATE TABLE Persons(
Id_P int not null,
LastName varchar(255) not null,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
 constraint uc_PersonID unique (id_p)
);
```

## SQL UNIQUE Constraint on ALTER TABLE
当表已经被创建时，如需要在“Id_P”创建UNIQUE约束，请使用下列SQL：

MySQL / SQL Server / Oracle / MS ACCESS:

```
alter table Persons add unique key(`Id_P`);
```
如需命名UNIQUE约束，并定义多个列的UNIQUE约束，请使用下面的SQL语法：


```
alter table Persons add constraint `uc_PersonID` unique key (`Id_P`);
```

## 撤销UNIQUE约束
如需要撤销UNIQUE约束，请使用下面的SQL:

MySQL:


```
ALTER TABLE Persons
DROP INDEX uc_PersonID
```

SQL Server / Oracle / MS Access:


```
ALTER TABLE Persons
DROP CONSTRAINT uc_PersonID
```














