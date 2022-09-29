#### 什么是是索引

索引是一种数据结构，可以帮助我们检索到数据库中的数据。



#### 索引采用什么数据结构

比较常见的数据结构是：Hash索引和B+树索引。



#### Hash索引和B+树索引的区别有优缺

Hash查找速度很快，但是多个数据的存储是没有**顺序**的，对于**区间查询**往往需要全表扫描，hash索引往往只适合等值查询。

B+树是一种多路查找平衡树，他的叶子结点是通过链表串联在一起的，在区间查询中找到边界后就可以沿着链表进行查找，避免了全表扫描问题。

主要的区别都可以围绕着顺序关系展开，比如hash无法利用索引完成排序，不支持多列索引的最左匹配规则，除此之外，hash索引还面临hash碰撞问题



#### 什么是回表查询 [🔗](https://www.jianshu.com/p/8991cbca3854)

![img](E:\git\typora\LeetCode刷题\images\webp)

注意：聚簇索引叶子结点直接**存储行记录**，而普通索引存放**主键值**。

所以所通过主键值可以非常快的地找到对应的记录，而普通索引想要找到对应的数据，首先需要在普通索引中找到对应的主键值，然后再在聚簇索引中寻找行记录，这就是**回表查询**。先定位主键值，再定位行记录，它的性能较扫一遍索引树更低。



#### 什么是覆盖索引 [🔗](https://www.jianshu.com/p/8991cbca3854)

解释一：就是select的数据只用从索引中就可以获取，不必从数据表中获取，换句话说查询列要被所使用的索引覆盖。

解释二：如果一个索引包含(或覆盖)所有需要查询的字段的值，称为‘覆盖索引’。即只需扫描索引而无须回表。

解释三：简单的说，覆盖索引覆盖所有需要查询的字段（即，大于或等于所查询的字段）。MySQL可以通过索引获取查询数据，因而不需要读取数据行。

举个简单例子，一个数据库，有两个索引，一个聚集索引是：`id`，一个普通索引是`name`。

那么，第一种情况：

```mysql
select id,name from students where name='Bob'; # 这种情况可以命中覆盖索引

select id,name,sex from students where name='Bob'; # 这种情况无法命中覆盖索引
```

可以应用的场景：

+   全表count查询优化

    ```mysql
    select count(name) from user;
    ```

+   列查询回表优化

    ```mysql
    select id,name,sex ... where name='shenjian';
    ```

+   分页查询

    ```mysql
    select id,name,sex ... order by name limit 500,100;
    ```

    

#### 什么是全文索引 FULLETEXT

全文索引目前只有MyISAM引擎支持，不过目前只有CHAR，VARCHAR，TEX列上可以创建全文索引，值得注意的是，在数据量较大的时候，将数据法如一个没有全局索引的表中，再利用CREATE创建索引比讲礼仪一个索引再插入数据要快很多。

主要是为了解决`WHERE name LIKE "%word%"`这样的

InnoDB也可以利用插件实现对应的全文索引。



#### 什么是联合索引

包含两列或更多列以上的索引，成为联合索引，也被称为符合索引。

![737276-20200809192058664-1038936465](E:\git\typora\LeetCode刷题\images\737276-20200809192058664-1038936465.png)

匹配规则：

+   联合索引结果上的叶子结点包含了多个索引列，比如上面包括：age，firstname，lastname
+   在匹配的过程中，首先会将联合索引中的第一个索引条件和节点中的第一个索引列进行匹配，如果匹配成功，那么接着匹配第二个索引条件和第二个索引列；依次类推，直到所有的索引条件都完成匹配。

注意：

+   超过三个列的联合索引不合适，否则虽然介绍了回表操作，但索引块过多，查询时就要遍历更多索引块
+   建立索引应该谨慎，建立索引的过程会产生锁，不是行级锁，而是锁住整个表，在生产环境中建立索引是非常危险的事情

好处：

**减少开销。**建一个联合索引`(Gid,Cid,SId)`，实际相当于建了`(Gid)、(Gid,Cid)、(Gid,Cid,SId)`三个索引。每多一个索引，都会增加写操作的开销和磁盘空间的开销。对于大量数据的表，使用联合索引会大大的减少开销！

**覆盖索引**。对联合索引(Gid,Cid,SId)，如果有如下的sql: select Gid,Cid,SId from student where Gid=1 and Cid=2。那么MySQL可以直接通过遍历索引取得数据，而无需回表，这减少了很多的随机io操作。减少io操作，特别的随机io其实是dba主要的优化策略。所以，在真正的实际应用中，覆盖索引是主要的提升性能的优化手段之一。

**建议。**单表尽可能不要超过一个联合索引，单个联合索引不超过3个字段。



#### 关于区间查询

区间查询就是查询指定范围内的数据，对于B+树很容易办到，但是对于Hash索引则需要全文匹配查找。



#### 关于最左匹配规则 [🔗](https://blog.csdn.net/u013164931/article/details/82386555)

**把最常用的，筛选数据最多的字段放在左侧。**

(A,B,C) 这样3列，mysql会首先匹配A，然后再B，C.

如果用(B,C)这样的数据来检索的话，就会找不到 `A` 使得索引失效。如果使用(A,C)这样的数据来检索的话，就会先找到所有A的值然后匹配C，此时联合索引是失效的，回答这种问题有两个关键点：第一个画**出数据结构**，第二个**二分法**。如果不能使用二分法查找比较大小，则说明无法使用最左匹配规则。



例子：

```mysql
# 创建数据表
create table test(
a int ,
b int,
c int,
d int,
key index_abc(a,b,c)
)engine=InnoDB default charset=utf8;

# 插入 5000 条数据
DROP PROCEDURE IF EXISTS proc_initData;
DELIMITER $
CREATE PROCEDURE proc_initData()
BEGIN
DECLARE i INT DEFAULT 1;
WHILE i<=3000 DO
    INSERT INTO test(a,b,c,d) VALUES(i,i,i,i);
    SET i = i+1;
END WHILE;
END $
CALL proc_initData();
```

**explain select * from test where a<10 ;**

![image-20210415163420706](E:\git\typora\LeetCode刷题\images\image-20210415163420706.png)

**explain select * from test where a<10 and b < 10 ;**

![image-20210415163501510](C:\Users\chenmingxin7.360BUYAD\AppData\Roaming\Typora\typora-user-images\image-20210415163501510.png)

**explain select * from test where a<10 and c < 10;**

![image-20210415163532524](C:\Users\chenmingxin7.360BUYAD\AppData\Roaming\Typora\typora-user-images\image-20210415163532524.png)

但是当我们去查找b和c的时候，是用不了索引的，只能全文匹配，因为我们无法对索引之间的关系进行比较。

![image-20210415163652818](C:\Users\chenmingxin7.360BUYAD\AppData\Roaming\Typora\typora-user-images\image-20210415163652818.png)

b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即索引的最左匹配特性。


#### Hash碰撞问题



#### Mysql的顺序访问和索引访问

顺序访问是在表中实现全表扫描，从头到尾逐行遍历，直到在无序的行数据中找到符合条件的目标数据。

索引访问是通过遍历索引来直接访问表中记录行的方式，索引的**查找**速度较快，但是**修改，插入和删除**速度较慢。

#### 索引的优缺点

索引的优点如下：

-   通过创建唯一索引可以保证数据库表中每一行数据的唯一性。
-   可以给所有的 MySQL 列类型设置索引。
-   可以大大加快数据的查询速度，这是使用索引最主要的原因。
-   在实现数据的参考完整性方面可以加速表与表之间的连接。
-   在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间

增加索引也有许多不利的方面，主要如下：

-   创建和维护索引组要耗费时间，并且随着数据量的增加所耗费的时间也会增加。
-   索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间。如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。
-   当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，这样就降低了数据的维护速度。

**最好的办法是先删除表中的索引，然后插入数据，插入完成后，再创建索引。**



#### Mysql 索引什么时候失效 [🔗](https://blog.csdn.net/mu_wind/article/details/110128016)

**1、条件字段原因**

​	**单字段有索引**，WHERE条件使用多字段（含带索引的字段），例如 SELECT * FROM student WHERE name ='张三' AND addr = '北京市'语句，如果name有索引而addr没索引，那么SQL语句不会使用索引。
​	**多字段索引**，违反最佳左前缀原则。例如，student表如果建立了(name,addr,age)这样的索引，WHERE后的第一个查询条件一定要是name，索引才会生效。

**2、<>、NOT、in、not exists**

当查询条件为**等值或范围查询**时，索引可以根据查询条件去找对应的条目。否则，索引定位困难（结合我们查字典的例子去理解），执行计划此时可能更倾向于全表扫描，这类的查询条件有：`<>、NOT、in、not exists`

**3、查询条件中使用`OR`**

如果条件中有or，即使其中有条件带索引也不会使用(因此`SQL`语句中要尽量避免使用`OR`)。要想使用`OR`，又想让索引生效，只能将`OR`条件中的每个列都加上索引。

**4、查询条件使用LIKE通配符**

SQL语句中，使用后置通配符会走索引，例如查询姓张的学生（SELECT * FROM student WHERE name LIKE '张%'），而前置通配符(SELECT * FROM student WHERE name LIKE '%东')会导致索引失效而进行全表扫描。

**5、索引列上做操作（计算，函数，（自动或者手动）类型装换）**

有以下几种例子：

在索引列上使用函数：例如select * from student where upper(name)='ZHANGFEI';会导致索引失效，而select * from student where name=upper('ZHANGFEI');是会使用索引的。
在索引列上计算：例如select * from student where age-1=17;

**6、在索引列上使用mysql的内置函数，索引失效**

例如，SELECT * FROM student WHERE create_time

**7、索引列数据类型不匹配**

例如，如果age字段有索引且类型为字符串（一般不会这么定义，此处只是举例）但条件值为非字符串，索引失效，例如`SELECT * FROM student WHERE age=18`会导致索引失效。

**8、索引列使用`IS NOT NULL`或者`IS NULL`可能会导致无法使用索引**

B-tree索引`IS NULL`不会使用索引，`IS NOT NULL`会使用，位图索引`IS NULL`、`IS NOT NULL`都会使用索引。