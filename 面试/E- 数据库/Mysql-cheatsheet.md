#### 查看数据库存放的位置

```mysql
SHOW GLOBAL VARIABLES LIKE "%Datadir%";
```





#### 基本语法

>   DDL 数据库定义语言

+   CREATE
+   ALERT
+   DROP

在堆数据库进行创建的应该判断当前数据库是否存在，如果不存在才创建，如果存在，要么不创建，要么删除创建一个新的数据库。对于数据表也是这样的逻辑。

```sql
-- 第一种，如果存在，则不创建数据表
CREATE TABLE IF NOT EXISTS `teacher` (
	`id` int,
    `name` varchar(10)
);

-- 第二种，如果数据表存在则先删除数据表
DROP TABLE IF EXISTS `teacher`;
```





#### 设置数据密码

```mysql
# 在sql中改密码
set password for root@localhost = password('123');

# mysqladmin -u 用户名 -p 旧密码 password 新密码
mysqladmin -u root -p 123 password 123

# 进入mysql数据库
use mysql;
update mysql.user set authentication_string=password('123') where user='root' and host="localhost";
```



#### SQL基本书写规则 和 大小写规则

可以使用倒引号**`**将表名和列名括起来。

通俗的说就是，MySQL 在 Windows 系统下不区分大小写，但在 Linux 系统下默认区分大小写。因此，数据库名、表名和字段名，都不允许出现任何大写字母，避免节外生枝。



#### SQL帮助系统

```mysql
$ HELP INT;
Name: 'INT'
Description:
INT[(M)] [UNSIGNED] [ZEROFILL]

A normal-size integer. The signed range is -2147483648 to 2147483647.
The unsigned range is 0 to 4294967295.

MySQL 提供了 4 张数据表来保存服务端的帮助信息，即使用 HELP 语法查看的帮助信息。执行语句就是从这些表中获取数据并返回给客户端的，MySQL 提供的 4 张数据表如下：
help_category：关于帮助主题类别的信息
help_keyword：与帮助主题相关的关键字信息
help_relation：帮助关键字信息和主题信息之间的映射
help_topic：帮助主题的详细内容
```





####  SQL查看数据库

```mysql
$ show DATABASES;
information_schema
mysql
performance_schema
w3cshool

-- information_schema：主要存储了系统中的一些数据库对象信息，比如用户表信息、列信息、权限信息、字符集信息和分区信息等。
-- mysql：MySQL 的核心数据库，类似于 SQL Server 中的 master 表，主要负责存储数据库用户、用户访问权限等 MySQL 自己需要使用的控制和管理信息。常用的比如在 mysql 数据库的 user 表中修改 root 用户密码。
-- performance_schema：主要用于收集数据库服务器性能参数。
```



#### SQL Like 语句

```mysql
show tables like "school";
show tables like "%school";
show tables like "school%";
show tables like "%school%";
```



#### SQL 创建数据库

```mysql
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];

比如
CREATE DATABASE IF NOT EXISTS w1cshool
DEFAULT CHARACTER SET utf8 
DEFAULT COLLATE utf8_general_ci;
```

利用`SHOW CREATE DATABASE w1cshool;`可以查看数据库的定义说明。



#### SQL修改数据库

```mysql
$ show create database w3cshool;
+----------+---------------------------------------------------------------------+
| Database | Create Database                                                     |
+----------+---------------------------------------------------------------------+
| w3cshool | CREATE DATABASE `w3cshool` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+---------------------------------------------------------------------+

$ alter database w3cshool default character set utf8;

```



#### SQL删除数据库

```mysql
DROP DATABASE IF NOT EXISTS w3cshool;
```



#### 数据库设计过程

>   软件开发周期

需求分析，概要设计，逻辑设计/详细设计，代码编写，软件测试，安装部署。





### DDL 语言

>   创建数据库

```mysql
# 数据库操作

-- 创建数据库，如果不存在的话，并设定默认字符集以及校验方法。
CREATE DATABASE IF NOT EXISTS w3cshool set DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

>   创建数据表

```sql

-- 创建数据表，如果不存在的话
CREATE TABLE IF NOT EXISTS `teacher`(
    id INT(11),
    name varchar(30)
)ENGINE=INNODB DEFAULT charset=utf8;

-- 如果数据表存在，则删除
DROP TABLE IF EXISTS `teacher`;
```

>   删除数据表中元素

```mysql
delete from `teacher` where id=12; # Innodb和MySIAN都不会释放此磁盘空间
optimize table `teacher`; # 当对表有大量的增删改操作时,需要用optimize对表进行优化,可以减少空间与提高I/O性能,


1、drop table table_name : 删除表全部数据和表结构，立刻释放磁盘空间，不管是 Innodb 和 MyISAM;

实例，删除学生表：

drop table student;
2、truncate table table_name : 删除表全部数据，保留表结构，立刻释放磁盘空间 ，不管是 Innodb 和 MyISAM;

实例，删除学生表：

truncate table student;
3、delete from table_name : 删除表全部数据，表结构不变，对于 MyISAM 会立刻释放磁盘空间，InnoDB 不会释放磁盘空间;

实例，删除学生表：

delete from student;
4、delete from table_name where xxx : 带条件的删除，表结构不变，不管是 innodb 还是 MyISAM 都不会释放磁盘空间;

实例，删除学生表中姓名为 "张三" 的数据：

delete from student where T_name = "张三";
5、delete 操作以后，使用 optimize table table_name 会立刻释放磁盘空间，不管是 innodb 还是 myisam;

实例，删除学生表中姓名为 "张三" 的数据：

delete from student where T_name = "张三";
实例，释放学生表的表空间：

optimize table student;
6、delete from 表以后虽然未释放磁盘空间，但是下次插入数据的时候，仍然可以使用这部分空间。
```



```mysql
CREATE TABLE Test(
	id INTEGER,
    flag boolean
);
ALTER TABLE Test ADD COLUMN country varchar(20);
```



#### 展示表的细节信息

```mysql
SHOW FULL COLUMNS FROM students;
+------------+------------------+-------------------+------+-----+---------+-------+---------------------------------+---------+
| Field      | Type             | Collation         | Null | Key | Default | Extra | Privileges                      | Comment |
+------------+------------------+-------------------+------+-----+---------+-------+---------------------------------+---------+
| student_id | int(10) unsigned | NULL              | NO   | PRI | 0       |       | select,insert,update,references |         |
| name       | varchar(30)      | latin1_swedish_ci | YES  |     | NULL    |       | select,insert,update,references |         |
| sex        | char(1)          | latin1_swedish_ci | YES  |     | NULL    |       | select,insert,update,references |         |
| birth      | date             | NULL              | YES  |     | NULL    |       | select,insert,update,references |         |
+------------+------------------+-------------------+------+-----+---------+-------+---------------------------------+---------+
4 rows in set (0.03 sec)
```



#### 展示表的状态信息

```mysql
 show table status like "%student%";
 # 展示的是表的状态信息
```

![image-20210410163249142](E:\git\typora\LeetCode刷题\images\image-20210410163249142.png)



#### 展示表的创建信息

```mysql
SHOW CREATE TABLE students;
```



#### SHOW 和 SELECT的区别

SHOW只展示数据表，数据库，字段以及服务器的信息，不会战术数据表内部record的信息。

SELECT一般用来展示record的信息。





#### MySql重命名一个数据库

```shell
#!/bin/bash
set -e # terminate execution on command failure

mysqlconn="mysql -u root -proot"
olddb=$1
newdb=$2
$mysqlconn -e "CREATE DATABASE $newdb"
params=$($mysqlconn -N -e "SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES \
                           WHERE table_schema='$olddb'")
for name in $params; do
      $mysqlconn -e "RENAME TABLE $olddb.$name to $newdb.$name";
done;

$ mysqlconn -e "DROP DATABASE $olddb"
$ sh rename_database.sh oldname newname
```

或者快速生成对应的脚本：

```mysql
SELECT CONCAT('RENAME TABLE my_db.', table_name, ' TO yiibaidb.', table_name, '; ') FROM information_schema.TABLES  WHERE table_schema='my_db';


+------------------------------------------------------------------------------+
| CONCAT('RENAME TABLE my_db.', table_name, ' TO yiibaidb.', table_name, '; ') |
+------------------------------------------------------------------------------+
| RENAME TABLE my_db.customers TO yiibaidb.customers;                          |
| RENAME TABLE my_db.employees TO yiibaidb.employees;                          |
| RENAME TABLE my_db.items TO yiibaidb.items;                                  |
| RENAME TABLE my_db.offices TO yiibaidb.offices;                              |
| RENAME TABLE my_db.orderdetails TO yiibaidb.orderdetails;                    |
| RENAME TABLE my_db.orders TO yiibaidb.orders;                                |
| RENAME TABLE my_db.payments TO yiibaidb.payments;                            |
| RENAME TABLE my_db.productlines TO yiibaidb.productlines;                    |
| RENAME TABLE my_db.products TO yiibaidb.products;                            |
| RENAME TABLE my_db.tokens TO yiibaidb.tokens;                                |
+------------------------------------------------------------------------------+
```







#### MySql 获取一张表的所有字段名称

```mysql
select COLUMN_NAME from information_schema.COLUMNS where table_name = 'students' and table_schema = 'w3cshool';
```





#### Mysql 如何创建一张临时表

```mysql
# 这里不可以使用sql server 的 select into 来创建临时表
CREATE TEMPORARY TABLE IF NOT EXISTS table2 AS (SELECT * FROM table1)
```



#### Mysql 可以利用*进行通配符匹配

```mysql
SELECT students.* from students;
```



#### Mysql 添加自增属性

```mysql
ALTER TABLE students MODIFY student_id INT; # 删除自增属性
ALTER TABLE students MODIFY student_id INT AUTO_INCREMENT; # 添加自增属性
ALTER TABLE students AUTO_INCREMENT = 300; # 设置开始值
```



#### 创建用户

```mysql
create user lizi@localhost identified by '1234';
```

