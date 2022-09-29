#### SQL 数据类型

MySQL数据类型的选择应该描述最精确的。

注意：在实际使用中应该慎用这两个类型BLOB与TEXT，尤其是会创建临时表的情况下，因为如果临时表大小超过max_heap_table_size或者tmp_table_size，就会将临时表存储在磁盘上，进而导致整体速度下降

```mysql
1) 数值类型
整数类型包括 TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT，浮点数类型包括 FLOAT 和 DOUBLE，定点数类型为 DECIMAL。

2) 日期/时间类型
包括 YEAR、TIME、DATE、DATETIME 和 TIMESTAMP。

3) 字符串类型
包括 CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM 和 SET 等。


   AUTO_INCREMENT
   BIGINT
   BINARY
   BIT
   BLOB # BLOB类型的字段用于存储二进制数据，比如文件，字符串
   BLOB DATA TYPE
   BOOLEAN
   CHAR
   CHAR BYTE
   DATE
   DATETIME
   DEC
   DECIMAL
   DOUBLE
   DOUBLE PRECISION
   ENUM
   FLOAT
   INT
   INTEGER
   LONGBLOB # LongBlob 最大 4G
   LONGTEXT
   MEDIUMBLOB # MediumBlob 最大 16M
   MEDIUMINT
   MEDIUMTEXT
   SET DATA TYPE
   SMALLINT
   TEXT
   TIME
   TIMESTAMP
   TINYBLOB # 字符串 TinyBlob 最大 255 字节
   TINYINT # -128〜127
   TINYTEXT
   VARBINARY
   VARCHAR
   YEAR DATA TYPE
```





#### MySql 日期类型

这里对于排序很重要

| 类型名称  | 日期格式            | 日期范围                                          | 存储需求 |
| --------- | ------------------- | ------------------------------------------------- | -------- |
| YEAR      | YYYY                | 1901 ~ 2155                                       | 1 个字节 |
| TIME      | HH:MM:SS            | -838:59:59 ~ 838:59:59                            | 3 个字节 |
| DATE      | YYYY-MM-DD          | 1000-01-01 ~ 9999-12-3                            | 3 个字节 |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59         | 8 个字节 |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1980-01-01 00:00:01 UTC ~ 2040-01-19 03:14:07 UTC | 4 个字节 |





#### MySql 变量

变量一般划分为**全局变量**，**会话变量**，**用户变量**和**局部变量**，[🔗](https://blog.csdn.net/albertsh/article/details/103421646)。

![image-20210410150309834](E:\git\typora\LeetCode刷题\images\image-20210410150309834.png)

首先：MySQL维护了许多系统变量来控制运行的行为，

```mysql
SHOW GLOBAL VARIABLES; # 全局变量

SHOW SESSION VARIABLES; # 会话变量

select @var; # 查看用户变量
select 100 into @var; # 设置用户变量

declare count int(4); # 局部变量，只能在函数或者存储过程中设置，除了存储过程，变量无效。
```

可以通过下面三种方式修改系统变量

1.  修改源代码，然后堆MySQL源代码重新编译
2.  修改MySQL配置文件，`my.ini`或者`my.cnf`，然后重启服务器生效
3.  利用SET命令重新设计系统变量的值

```mysql
SET @@session.sort_buffer_size = 50000;

```



#### MySql变量进行运算

```mysql
## 定义用户变量并进行运算
# 1. 定义
set @a=100;
set @b=90;
# or
select count(*) into @count from student;

# 计算
set @c=@a+@b;
# or
select @a+@b into @c;
```

