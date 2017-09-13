# 185. Department Top Three Salaries

The **Employee** table holds all employees. Every employee has an Id, and there is also a column for the department Id.

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
```

The **Department** table holds all departments of the company.

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

Write a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, you SQL query should return the following rows.

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```

The DDL statements:

```
Create table If Not Exists Employee (Id int, Name varchar(255), Salary int, DepartmentId int);
Create table If Not Exists Department (Id int, Name varchar(255));
Truncate table Employee;
insert into Employee (Id, Name, Salary, DepartmentId) values ('1', 'Joe', '70000', '1');
insert into Employee (Id, Name, Salary, DepartmentId) values ('1', 'Randy', '85000', '1');
insert into Employee (Id, Name, Salary, DepartmentId) values ('1', 'Randy', '80000', '1');
insert into Employee (Id, Name, Salary, DepartmentId) values ('2', 'Henry', '80000', '2');
insert into Employee (Id, Name, Salary, DepartmentId) values ('3', 'Sam', '60000', '2');
insert into Employee (Id, Name, Salary, DepartmentId) values ('4', 'Max', '90000', '1');
Truncate table Department;
insert into Department (Id, Name) values ('1', 'IT');
insert into Department (Id, Name) values ('2', 'Sales');
```

---

思路：

**Employee** 和 **Department 进行连接**

问题简化

查询IT部门70000薪水的排名

```
select count(distinct Salary) as num  from employee where DepartmentId = 1 AND Salary >= 70000;
```

85000薪水的排名

```
select count(distinct Salary) as num  from employee where DepartmentId = 1 AND Salary >= 85000;
```

查看部门前三的薪水（子查询）

```
select * from Employee as e 
    where 
    (select count(distinct e2.salary) from Employee as e2 
        where e.DepartmentId = e2.DepartmentId and e2.Salary >= e.Salary) <= 3;
```

核心问题解析了那就把连接和子查询结合起来使用

```
SELECT 
    d.Name AS Department, e.Name AS Employee, e.Salary AS Salary 
FROM 
    Employee AS e 
INNER JOIN 
    Department AS  d 
ON 
    e.DepartmentId = d.Id 
WHERE (SELECT COUNT(DISTINCT e2.salary) FROM Employee AS e2 WHERE e.DepartmentId = e2.DepartmentId AND e2.Salary >= e.Salary) <= 3;

```

Perfect!!!

