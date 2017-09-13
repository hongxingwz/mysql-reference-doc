# 196. Delete Duplicate Emails

Write a SQL query to delete all duplicate email entries in a table named **Person**, keeping only unique emails based on its _smallest_ **Id**

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id is the primary key column for this table.
```

For example, after running your query, the above Person table should have the following rows:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

SQL Schema

    DROP TABLE IF EXISTS `Person`;

    CREATE TABLE `Person`(
      Id INT NOT NULL AUTO_INCREMENT,
      Email VARCHAR(255),
      PRIMARY KEY (Id)
    )ENGINE InnoDB CHARSET UTF8;

    INSERT INTO `Person` VALUES (1, "john@example.com"), (2, "bob@example.com"), (3, "john@example.com");

Approach: Using **DELETE** and **WHERE** clause \[Accepted\]  
Algorithm:  
By joining this table with itself on the Email column, we can get the following code.

```
SELECT 
  p1.*
FROM
  Person p1, Person p2
WHERE 
  p1.Email = p2.Email
;
```

Then we need to find the bigger id having same email address with other records. So we can add a new condition to the **WHERE** clause like this.

```
SELECT
  p1.*
FROM
  Person p1, Person p2
WHERE
  p1.Email = p2.Email AND p1.Id > p2.Id
;
```

As we already get the records to be deleted, we can alter this statement to **DELETE** in the end.

```
DELETE 
  p1
FROM
  Person p1, Person p2
WHERE
  p1.Email = p2.Email AND p1.Id > p2.Id
;
```

Algorithm \(By myself\)

I usually like use** left join** or** inner join**

First:

```
SELECT * FROM Person p INNER JOIN Person p2 ON  p.Email = p2.Email AND  p.Id > p2.Id;
```

Finally:

```
DELETE p FROM Person p INNER JOIN Person p2 ON  p.Email = p2.Email AND  p.Id > p2.Id;
```

Result Set:

    mysql> SELECT * FROM `Person`;
    +----+------------------+
    | Id | Email            |
    +----+------------------+
    |  1 | john@example.com |
    |  2 | bob@example.com  |
    +----+------------------+
    2 rows in set (0.01 sec)




