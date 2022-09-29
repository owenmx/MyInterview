## SELECT 语句

```mysql
SELECT
{* | <字段列名>}
[
FROM <表 1>, <表 2>…
[WHERE <表达式>
[GROUP BY <group by definition>
[HAVING <expression> [{<operator> <expression>}…]]
[ORDER BY <order by definition>]
[LIMIT[<offset>,] <row count>]
]
```



#### DISTINCT 过滤重复数据

```mysql
# 对某一个或多个字段进行过滤
SELECT DISTINCT name,age from employee_tbl;

# 对所有字段进行过滤
SELECT DISTINCT * from employee_tbl;
```



#### 设置别名

>   为表设置别名

```mysql
 select DISTINCT name,singin from employee_tbl as DIS_table;
```

>   为字段设置别名

```mysql
SELECT employee_tbl.name as "姓名" from employee_tbl;
```



#### LIMIT 限制查询结果的条数

基本语法格式：

```mysql
LIMIT 初始位置，记录数

# 比如
SELECT employee_tbl.name from employee_tbl LIMIT 3; # 默认从第1个开始，记录数为3
SELECT employee_tbl.name from employee_tbl LIMIT 2,3; # 从第3个开始，记录数为3
SELECT employee_tbl.name from employee_tbl LIMIT 3 OFFSET 2; # 从第3个开始，记录数为3
```



#### ORDER BY 对数据进行排序

```mysql
ORDER BY <字段> [ASC|DESC]

# 单字段排序，比如
select * from student order by name ASC;
select * from student order by name DESC;

# 多字段排序，比如
SELECT * from employee_tbl order by singin DESC,id ASC; # 先按照singin降序排列，然后按照id进行升序排列

```

LIKE对大小写是不敏感的，也就是大小写都会进行匹配，如果想要大小写敏感的话，可以加上BINARY 关键字。

```mysql
SELECT * from students where name like BINARY "c%";
```





#### 模糊查询

```mysql
# 1. 带有%的通配符的查询,它能代表任何长度的字符串，字符串的长度可以为 0
SELECT * from student where name like "%"

# 2. 带有_的通配符，只代表单个字符，字符长度不能为0，比如a_b可以匹配abc,adc
SELECT * from student where name like "a%_c"
```

 



数据库中的两个难点：一个是多表查询（join），一个是聚合分组。

#### Group By

单独使用 GROUP BY 关键字时**，查询结果会只显示每个分组的第一条记录。**

分组是聚合的前提，group就是将数据进行分组，也就是将原来的一整块数据分成几小块，聚合是在每个分组内进行一些统计，比如在分组内求解最大值，最小值，平均数和个数等。

未分组时查询返回的行直接与数据库表中的行对应，分组后分为多少组这个查询就会返回多少条记录，返回的记录中只能包含分组字段和在这个分组上的聚合操作的值（MySQL是例外见后面）

一般形式为：语法为：

```mysql
select t.sex, count(t.id) as cnt group by sex having cnt>2
select 查询字段, 聚合字段 group by 分组字段
```

查询中有三种字段：
查询字段：在select中出现的表中的字段
聚合字段：对表中的字段施加了聚合函数后形成的字段
分组字段：group by后的字段



#### 关于 GROUP_CONCAT() 

GROUP BY 关键字可以和 GROUP_CONCAT() 函数一起使用。GROUP_CONCAT() 函数会把每个分组的字段值都显示出来

```mysql
select sex,GROUP_CONCAT(name) as "成员" from students group by sex;

+------+-----------------------------------------------------+
| sex  | 成员                                                |
+------+-----------------------------------------------------+
| 女   | Jack,White,Alicia,Zoe,Goodman,Eve,David,Carry,Alice |
| 男   | Bob                                                 |
+------+-----------------------------------------------------+
```



#### 多字段进行groupby

上面实例在分组过程中，先按照 sex字段进行分组，当 sex字段值相等时，再把 sex字段值相等的记录按照 age 字段进行分组。

```mysql
select sex,age,GROUP_CONCAT(name) from students group by sex,age;

+------+------+--------------------+
| sex  | age  | GROUP_CONCAT(name) |
+------+------+--------------------+
| 女   |   21 | David              |
| 女   |   22 | Jack,Goodman,Eve   |
| 女   |   23 | Zoe                |
| 女   |   24 | Alice              |
| 女   |   26 | White              |
| 女   |   28 | Carry,Alicia       |
| 男   |   12 | Bob                |
+------+------+--------------------+
```



#### GROUP与聚合函数

在进行数据统计的时候，经常和聚合函数一起使用

```mysql
select sex,age,GROUP_CONCAT(name),count(*) as "人员数量" from students group by sex,age;
+------+------+--------------------+----------+
| sex  | age  | GROUP_CONCAT(name) | 人员数量 |
+------+------+--------------------+----------+
| 女   |   21 | David              |        1 |
| 女   |   22 | Jack,Goodman,Eve   |        3 |
| 女   |   23 | Zoe                |        1 |
| 女   |   24 | Alice              |        1 |
| 女   |   26 | White              |        1 |
| 女   |   28 | Carry,Alicia       |        2 |
| 男   |   12 | Bob                |        1 |
+------+------+--------------------+----------+
```



#### GROUP BY 与 WITH ROLLUP

WITH POLLUP 关键字用来在所有分段记录的最后加上一行记录，这条记录是上面所有记录的总和，

```mysql
select sex,age,hobby,GROUP_CONCAT(name),count(*) as "人员数量" from students group by sex,age,hobby WITH ROLLUP;
+------+------+-------+---------------------------------------------------------+----------+
| sex  | age  | hobby | GROUP_CONCAT(name)                                      | 人员数量 |
+------+------+-------+---------------------------------------------------------+----------+
| 女   |   21 | play  | David                                                   |        1 |
| 女   |   21 | NULL  | David                                                   |        1 |
| 女   |   22 | play  | Jack                                                    |        1 |
| 女   |   22 | swim  | Goodman,Eve                                             |        2 |
| 女   |   22 | NULL  | Jack,Goodman,Eve                                        |        3 |
| 女   |   23 | swim  | Zoe                                                     |        1 |
| 女   |   23 | NULL  | Zoe                                                     |        1 |
| 女   |   24 | swim  | Alice                                                   |        1 |
| 女   |   24 | NULL  | Alice                                                   |        1 |
| 女   |   26 | play  | White                                                   |        1 |
| 女   |   26 | NULL  | White                                                   |        1 |
| 女   |   28 | play  | Carry                                                   |        1 |
| 女   |   28 | swim  | Alicia                                                  |        1 |
| 女   |   28 | NULL  | Carry,Alicia                                            |        2 |
| 女   | NULL | NULL  | David,Jack,Goodman,Eve,Zoe,Alice,White,Carry,Alicia     |        9 |
| 男   |   12 | swim  | Bob                                                     |        1 |
| 男   |   12 | NULL  | Bob                                                     |        1 |
| 男   | NULL | NULL  | Bob                                                     |        1 |
| NULL | NULL | NULL  | David,Jack,Goodman,Eve,Zoe,Alice,White,Carry,Alicia,Bob |       10 |
+------+------+-------+---------------------------------------------------------+----------+
```





#### 关于having，过滤分组

与`where`不同的是`having`在`group`后进行进行过滤，`where`在`group`前进行过滤其写在`group`关键字前，`having`后过滤的字段可以是分组字段或聚合字段如：

除此之外，where和group也存在以下区别：

-   一般情况下，WHERE 用于过滤数据行，而 HAVING 用于过滤分组。
-   WHERE 查询条件中不可以使用聚合函数，而 HAVING 查询条件中可以使用聚合函数。
-   WHERE 针对数据库文件进行过滤，而 HAVING 针对查询结果进行过滤。也就是说，**WHERE 根据数据表中的字段直接进行过滤，而 HAVING 是根据前面已经查询出的字段进行过滤。**
-   WHERE 查询条件中不可以使用字段别名，而 HAVING 查询条件中可以使用字段别名。

```mysql
select sex,age,GROUP_CONCAT(name),count(*) as "人员数量" from students group by sex,age HAVING age <= 22;

# 1. 在Having条件中使用聚合函数
select sex,age,GROUP_CONCAT(name),count(*) as "人员数量" from students group by sex,age HAVING count(*) >= 2;
```



#### 关于 ONLY_FUll_GROUP_BY

ONLY_FUll_GROUP_BY的意思是：对于GROUP BY聚合操作，如果在SELECT中的列，没有在GROUP BY中出现，那么这个SQL是不合法的，也就是说查出来的列必须是GROUP BY之后的字段。

```mysql
# 如果想要在当前会话开启这个属性：
SET sql_mode = 'ONLY_FULL_GROUP_BY'; # 开启
SET sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY','')); # 关闭

# 如果想要在全局开启这个属性：
SET GLOBAL sql_mode='ONLY_FULL_GROUP_BY';
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

