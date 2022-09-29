子查询是Sql中比较常见的额查询方法，通过子查询可以实现多表查询，子查询将一个查询语句嵌套在另一个查询语句中，子查询可以在SELECT，UPDATE和DELETE中使用。

子查询在WHERE中的语法格式如下：

```mysql
WHERE <表达式> <操作符> (子查询)
```

其中操作符可以是比较运算符和IN，NOT IN，EXISTS，NOT EXISTS等关键字。

#### （1）IN | NOT IN

当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回值正好相反。

#### （2）EXISTS | NOT EXISTS

用于判断子查询的结果集是否为空，若子查询的结果集不为空，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。

```mysql
select * from tcount_tbl t1 where exists (select runoob_author from runoob_tbl);
```

如果EXISTS 表达式返回 TRUE，外层查询语句接收 TRUE 之后对表 tcount_tbl进行查询，返回**所有**的记录。



#### 关于嵌套子查询

在 SELECT 语句中，子查询可以被嵌套在 SELECT 语句的列、表和查询条件中，即 SELECT 子句，FROM 子句、WHERE 子句、GROUP BY 子句和 HAVING 子句。



#### 发生在SELECT中的子查询

```mysql
# 子查询结果为单行单列，但不必指定列别名
SELECT (子查询) FROM 表名;


 # 如果子查询不是单行单列的话，查询会报错
 select t2.*,(select t1.runoob_title from runoob_tbl t1 where t1.runoob_author=t2.runoob_author limit 1) from tcount_tbl t2;
```



#### 发生在FROM中的子查询

嵌套在 SELECT 语句的 FROM 子句中的子查询语法格式如下。

```mysql
SELECT * FROM (子查询) AS 表的别名; # 必须指定别名。一般返回多行多列数据记录，可以当作一张临时表。

# 比如
SELECT * FROM (SELECT * FROM students) new_tb;
```





#### 嵌套子查询

+   **where 子查询**，指将内部查询的结果作为外层查询的比较条件。

    ```mysql
    -- 查出每个栏目最新的商品(以good_id为最大为最新商品)
    SELECT cat_id,good_id,good_name 
      FROM goods 
     WHERE good_id IN ( SELECT max(good_id) 
                         FROM goods 
                     GROUP BY cat_id);
    ```

+   **from子查询**，把内层的查询结果当成临时表，供外层sql再次查询。

    ```mysql
      SELECT * 
        FROM  (SELECT cat_id,good_id,good_name 
                 FROM goods 
             ORDER BY cat_id asc, good_id desc) AS tep 
    GROUP BY cat_id;
    ```

    
