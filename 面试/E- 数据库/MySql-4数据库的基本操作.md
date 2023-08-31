#### 创建数据表

```mysql
CREATE TABLE [IF NOT EXITS]  tb_empl (
    id INT(11),
    name VARCHAR(11),
    deptId INT(11),
    salary FLOAT
);
```

 #### 修改数据表

```mysql
# 在最后面增加列
ALTER TABLE tb_empl ADD COLUMN name int; 
# 在最后面增加列
ALTER TABLE tb_empl ADD COLUMN name int FIRST; 
# 在特定列后面添加列面增加列
ALTER TABLE tb_empl ADD COLUMN name int AFTER b; 

ALTER TABLE t1 CHANGE b f float(10);  # 修改列，将b修改为f，类型为float
ALTER TABLE t1 MODIFY b float(10);  # 修改列b为float类型
ALTER TABLE t1 DROP b;
```





#### MySQL 查看表结构

```mysql
# 第一种
DESC students;
+------------+--------------------+------+-----+---------+-------+
| Field      | Type               | Null | Key | Default | Extra |
+------------+--------------------+------+-----+---------+-------+
| student_id | int(10) unsigned   | NO   | PRI | 0       |       |
| name       | varchar(30)        | YES  |     | NULL    |       |
| sex        | char(1)            | YES  |     | NULL    |       |
| birth      | date               | YES  |     | NULL    |       |
| age        | smallint(6)        | YES  |     | NULL    |       |
| death_time | time               | YES  |     | NULL    |       |
| friends    | year(4)            | YES  |     | NULL    |       |
| hobby      | set('play','swim') | YES  |     | NULL    |       |
+------------+--------------------+------+-----+---------+-------+

# SHOW [FULL] COLUMNS FROM students；
```



#### MySql 查看表的创建过程

```mysql
SHOW CREATE TABLE students;
```



