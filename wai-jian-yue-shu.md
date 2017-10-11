### 创建表时添加外键

```sql
create table s_user(
    u_id int auto_increment primary key
)

create table s_orderform(
    o_id int auto_increment primary key,
    o_buyer_id int,
    constraint `fk_id` foreign key(o_buyer_id) references s_user(u_id) #外键到s_user表的u_id字段
```

### 已经存在的表中添加外键

```sql
ALTER TABLE ss_accesscode ADD CONSTRAINT FK_SS_ASC_VCC FOREIGN KEY(vccId) REFERENCES ss_vcc(vccId) ON 
DELETE CASCADE;
```

### 删除外键

```sql
ALTER TABLE ss_accesscode drop foreign key 外键约束名称
```

### 外键的语法

```sql
[CONSTRAINT symbol] FOREIGN KEY [id] (index_col_name, ...)  
    REFERENCES tbl_name (index_col_name, ...)  
    [ON DELETE {RESTRICT | CASCADE | SET NULL | NO ACTION}]  
    [ON UPDATE {RESTRICT | CASCADE | SET NULL | NO ACTION}]
```

* **CASCADE: 在父表上update/delete记录时，同步update/delete掉子表的匹配记录**
* **SET NULL: 在父表上update/delete记录时，将子表上匹配记录的列设为null（要注意子表的外键列不能为not null\)**
* **NO ACTION: 如果子表中有匹配的记录，则不允许对交表对应候选键进行update/delete操作**
* **RESTRICT: 同no action，都是立即检查外键约束**



## 查看一个表的外键约束

* select \* from INFORMATION\_SCHEMA.KEY\_COLUMN\_USAGE;
* show create table hibernate4.tableName \G



