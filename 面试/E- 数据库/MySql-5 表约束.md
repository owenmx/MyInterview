MySql中，越苏值得是堆表中数据的一种约束，能够邦族我们更好地管理数据

#### 主键约束

主键分为但字段主键和多字段主键，这里介绍两种主键地创建，修改和删除。

注意：

+   每个表只能定义一个主键
+   主键必须唯一表示表中的每一行
+   联合主键不能包含不必要的多余字段，也就是或如果将联合主键的某一个字段删除后，构成的主键依然满足唯一性元组，则联合主键是不正确的。

>   创建主键约束

```mysql
# 单字段主键
CREATE TABLE tmp_tb (
    a int PRIMARY KEY,
    b int
);

CREATE TABLE tmp_tb (
    a int,
    b int,
    PRIMARY KEY (a)
);

CREATE TABLE tmp_tb (
    a int,
    b int
);
ALTER TABLE tmp_tb ADD PRIMARY KEY(a);


# 多字段主键
CREATE TABLE tmp_tb (
    a int,
    b int,
    PRIMARY KEY (a,b)
);

CREATE TABLE tmp_tb (
    a int,
    b int
);
ALTER TABLE tmp_tb ADD PRIMARY KEY(a,b);

# 删除主键
ALTER TABLE tmp_tb DROP PRIMARY KEY;
```

#### 自增字段

```mysql
CREATE TABLE tmp_tb (
    a int AUTO_INCREMENT,
    b int,
    PRIMARY KEY (a,b)
);

```





#### 外键约束

外键用来表示主表与从表之间的关联关系，为两个表的数据建立连接，约束两个表中数据的一致，

假设有两张表，一张表是leader表，一张是department表

注意：从表的外键关联的必须是主表的主键，只有主表中有数据，从表才可以插入。

```mysql
"""
创建外键约束
    CONSTRAINT fk_emp_dept1 FOREIGN KEY(deptId) REFERENCES department(id)

或者在修改的时候加入外键
	ALTER TABLE leader ADD CONSTRAINT fk_tb_dept1 FOREIGN KEY(deptId) REFERENCES department(id);
	
删除外键
	ALTER TABLE leader DROP FOREIGN KEY fk_emp_dept1;
"""
DROP TABLE IF EXISTS department ;
CREATE TABLE department (
    id INT(11) PRIMARY KEY,
		name VARCHAR(22) NOT NULL,
		level int
)ENGINE=INNODB;

DROP TABLE IF EXISTS leader;
CREATE TABLE leader (
    id INT(11) PRIMARY KEY,
    name VARCHAR(50),
    deptId INT(11),
    salary FLOAT,
    CONSTRAINT fk_emp_dept1
    FOREIGN KEY(deptId) REFERENCES department(id)
);
```



#### 唯一约束

唯一约束与主键约束相似的是它们都可以确保列的唯一性。不同的是，唯一约束在一个表中可有多个，并且设置唯一约束的列允许有空值，但是只能有一个空值。而主键约束在一个表中只能有一个，且不允许有空值。

```mysql
-- ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
ALTER TABLE leader ADD CONSTRAINT `name` UNIQUE(name);

# 删除唯一约束
ALTER TABLE leader DROP INDEX name;
```





#### 检查约束

检查约束直到`MySQL 8.0.15`才正式起作用，在此之前的版本都不起作用，

>   The `CHECK` clause is parsed but ignored by all storage engines.

如果想实现对应的功能，可以尝试使用触发器。



#### 默认值 和 非空约束

```mysql
# 1.修改表时添加默认值约束
CREATE TABLE leader (
    id INT(11) PRIMARY KEY,
    name VARCHAR(50) UNIQUE,
    deptId INT(11),
    salary FLOAT DEFAULT 20.0,
    CONSTRAINT fk_emp_dept1 FOREIGN KEY(deptId) REFERENCES department(id)
);

# 2. 删除默认值越苏
-- ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
ALTER TABLE leader CHANGE COLUMN salary 
salary float DEFAULT NULL
```

