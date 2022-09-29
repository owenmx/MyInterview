#### INSERT 插入数据

插入数据有两种基本格式：

##### (1) INSERT  ....  VALUES 语句

```mysql
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];

# 这种语句既可以插入一条数据，也可以插入多条数据

INSERT INTO students ( student_id,name,sex,birth,age,death_time,friends,hobby) VALUES (170,"Gve","女","2022-12-04",22,"12:34:19",1999,"swim"),(171,"Hve","女","2022-12-04",22,"12:34:19",1999,"swim")
```

##### (2) INSERT ...  SET

```mysql
INSERT INTO <表名>
SET <列名1> = <值1>,
	<列名2> = <值2>,
	...
```



##### 关于INSERT INTO ... FROM 语句复制表数据

INSERT INTO…SELECT…FROM 语句用于快速地从一个或多个表中取出数据，并将这些数据作为行数据插入另一个表中。

```mysql
insert into my_students(student_id,name,sex,birth,age,death_time,friends,hobby) select * from students;
```



#### 更新数据

```mysql
UPDATE <表名> SET 字段1=值1 [WHERE 子句][ORDER BY 子句] [LIMIT 子句];

UPDATE tb_courses_new SET course_name='DB',course_grade=3.5 WHERE course_id=2;
```



#### 删除数据

```mysql
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句];

DELETE FROM student WHERE student_id=170;
```



#### 清空表记录，DROP，DELETE，TRUNCATE 三种之间的关系

执行速度：`drop-->delete-->truncate`

##### DROP 会删除内容和定义，释放空间。表结构和数据一同删除。

drop会删除表的结构，被依赖的约束，触发器，索引，而依赖于该表的存储过程/函数将会被保留，但其状态会变化位invalid。

```mysql
drop table students
```



##### TRUNCATE 删除内容，释放空间，但不删除定义

表结构还在，数据被删除。

```mysql
truncate table students;
```



##### DELETE 删除内容，不删除定义，也不释放空间

```mysql
delete from students;
```



##### 关于释放空间

delete删除数据的时候，数据是一行一行删除的，每删除一行数据，就在事务日志中位删除的那行数据做一项记录，因此可以对delete进行回滚操作。

truncate在删除数据时，是通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。

*因此，truncate 的执行速度比 delete 快，且使用的系统和事务日志资源少（在表数据越多的情况下对比更明显）； 不过，在执行回滚命令时，delete 将被撤销，而 truncate 则不会别撤销。*