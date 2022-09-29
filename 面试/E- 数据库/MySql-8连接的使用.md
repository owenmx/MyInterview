你可以在 `SELECT`,` UPDATE`和 `DELETE` 语句中使用 Mysql 的 JOIN 来联合多表查询。

JOIN的功能大致分为三类：

+   **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。
+   **LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。
+   **RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

在Mysql中，`cross join`，`inner join `，`join`三者所实现的功能是一样的



![img](E:\git\typora\LeetCode刷题\images\sql4.png)



表链接的语法解析：

![SQL INNER JOIN syntax](E:\git\typora\LeetCode刷题\images\sql-inner-join-syntax.png)



```mysql

# 数据
mysql> SELECT * FROM tcount_tbl;
+---------------+--------------+
| runoob_author | runoob_count |
+---------------+--------------+
| 菜鸟教程       | 10           |
| RUNOOB.COM    | 20           |
| Google        | 22           |
+---------------+--------------+
3 rows in set (0.01 sec)
 
mysql> SELECT * from runoob_tbl;
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 1         | 学习 PHP    | 菜鸟教程          | 2017-04-12      |
| 2         | 学习 MySQL  | 菜鸟教程          | 2017-04-12      |
| 3         | 学习 Java   | RUNOOB.COM    | 2015-05-01      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
| 5         | 学习 C      | FK            | 2017-04-05      |
+-----------+---------------+---------------+-----------------+
5 rows in set (0.01 sec)

# 从左到右
# 1.
select t1.runoob_author,t1.runoob_count from tcount_tbl as t1 left outer join runoob_tbl as t2 on t1.runoob_author=t2.runoob_author;

# 2.
select * from tcount_tbl as t1 left outer join runoob_tbl as t2 on t1.runoob_author=t2.runoob_author where t2.runoob_author is not null;

# 3. full join, mysql默认不支持full，但是可以用left join和right join 进行合并构成full join
select * from tcount_tbl as a left outer join runoob_tbl as b on a.runoob_author=b.runoob_author union select * from tcount_tbl as a right outer join runoob_tbl as b on a.runoob_author=b.runoob_author;

# 4. 这里用到了嵌套查询，select * from (...) xxx; 这里的xxx表示()查出来数据库的别名
select * from (
	select b.runoob_id,b.runoob_author as b_author,a.runoob_author as a_author,a.runoob_count,b.runoob_title,b.submission_date from tcount_tbl as a left outer join runoob_tbl as b on a.runoob_author=b.runoob_author 
	union 
	select b.runoob_id,b.runoob_author as b_author,a.runoob_author as a_author,a.runoob_count,b.runoob_title,b.submission_date from tcount_tbl as a right outer join runoob_tbl as b on a.runoob_author=b.runoob_author
	)new_tb where a_author is null or b_author is null;
	
# 5.
select t2.runoob_id,t2.runoob_title,t2.runoob_author,t2.submission_date from tcount_tbl as t1 left outer join runoob_tbl as t2 on t1.runoob_author=t2.runoob_author;

# 6.
select t2.runoob_id,t2.runoob_title,t2.runoob_author,t2.submission_date from tcount_tbl as t1 left outer join runoob_tbl as t2 on t1.runoob_author=t2.runoob_author where t2.runoob_author is not null;

# 7. Inner Join 
select * from runoob_tbl t1 inner join tcount_tbl t2 on t1.runoob_author=t2.runoob_author;
```





#### 关于笛卡尔积

在`Mysql`中，如果直接写`inner join`而不用`on`则得到的是笛卡尔积的结果。

什么是笛卡尔积，给定`A={1,2},B={3,4,5}`，那么`AxB`的笛卡尔积结果是`A×B={(1,3), (1,4), (1,5), (2,3), (2,4), (2,5) }`，表与表之间的运算应该避免笛卡尔积，因为他会产生大量不合理的数据。



#### INNER JOIN

内连接（INNER JOIN）主要通过设置连接条件的方式，来移除查询结果中某些数据行的交叉连接。简单来说，就是利用条件表达式来消除交叉连接的某些数据行。

**如果没有连接条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。**

```mysql
# 语法格式
SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]

# 例子
select * from runoob_tbl t1 inner join tcount_tbl t2 on t1.runoob_author=t2.runoob_author;
```



#### 左外连接

左外连接又称为左连接，使用 **LEFT OUTER JOIN** 关键字连接两个表，并使用 ON 子句来设置连接条件。

上述语法中，“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）。

```mysql
SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
```

语法说明如下。

-   字段名：需要查询的字段名称。
-   <表1><表2>：需要右连接的表名。
-   RIGHT OUTER JOIN：右连接中可以省略 OUTER 关键字，只使用关键字 RIGHT JOIN。
-   ON 子句：用来设置右连接的连接条件，不能省略。



#### 右外连接

右外连接又称为右连接，右连接是左连接的反向连接。使用 **RIGHT OUTER JOIN** 关键字连接两个表，并使用 ON 子句来设置连接条件。

右连接的语法格式如下：

```mysql
SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
```

语法说明如下。

-   字段名：需要查询的字段名称。
-   <表1><表2>：需要右连接的表名。
-   RIGHT OUTER JOIN：右连接中可以省略 OUTER 关键字，只使用关键字 RIGHT JOIN。
-   ON 子句：用来设置右连接的连接条件，不能省略。


与左连接相反，右连接以“表2”为基表，“表1”为参考表。右连接查询时，可以查询出“表2”中的所有记录和“表1”中匹配连接条件的记录。如果“表2”的某行在“表1”中没有匹配行，那么在返回结果中，“表1”的字段值均为空值（NULL）。

