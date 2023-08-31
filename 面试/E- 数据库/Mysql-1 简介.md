#### 发展历史

人工管理阶段-->电脑文件阶段-->数据库阶段



#### 基础概念

+   数据库就是一些关联表的集合。
+   数据表：表是数据的矩阵，一个数据库中的表就像是一个简单的电子表格。
+   列和行
+   冗余：存储两倍数据，冗余可以使得系统速度更快。
+   主键：主键是唯一的，一个数据表只能包含一个主键，比如ID。
+   **外键**：用于关联两个数据表。
+   复合键：将多个列作为一个索引键，用于复合索引。
+   索引：使用索引可以进行快速访问。
+   **参照完整性**：参照完整性要求关系不允许引用不存在的实体。



#### 数据库管理系统与数据库的区别

数据库是存储数据的地方。

数据库管理系统适用于管理数据库的软件，比如MySql。

对于 MySQL 数据库管理系统，服务器为MySQL DBMS。你可以在本地安装的副本上运行，也可以连接到运行在你具有访问权的远程服务器上的一个副本。



#### 数据库有哪些种类

+   层次性数据库
+   关系型数据库，比如Oracle和MySql
+   面向文档数据库，比如MongDB和CouchDB
+   列存储数据库，比如HBase
+   XML数据库，开发人员可以对数据库中的XML文档进行查询，导出和指定格式序列化。
+   键值存储数据库，比如Redis数据库。



#### B/S模型和C/S模型的区别 [🔗](https://blog.csdn.net/weixin_41648325/article/details/79654407)

>   B/S模型

B-S模型是Browser-Server结构，比如微信网页版

1.  用户通过浏览器向Web服务器提出HTTP请求。
2.  Web服务器根据浏览器请求调出相应文件，对相应文件不做处理或加以解释执行后，将纯客户端HTML代码结果返回给浏览器。
3.  浏览器接收到Web服务器发回的页面内容（纯HTML代码），显示给用户



>   C/S模型

C-S模型是Client-Server结构，比如微信PC版

1.  打开一个通信通道，告知服务器进程所在主机将在某一端口上接受客户请求。
2.  等待客户的请求到达该端口。
3.  服务器接收到服务请求，处理该请求并发送应答。
4.  返回至第2步，等待并处理另一个客户的请求。
5.  关闭服务器。



#### MySql的配置文件

在win系统中，配置文件是`my.ini`；在unix系统，配置文件是`my.cnf`

```shell
[mysql]
default-character-set=utf8

[mysqld]
basedir=/export/server/mysql-5.7.25-el7-x86_64
datadir=/export/data/mysql
socket=/var/lib/mysql/mysql.sock
port=3306

log-bin=/export/data/mysql/mysql-bin
server-id=1 #主节点和从节点这个配置值不能相同
binlog_format=ROW
binlog-ignore-db=mysql
binlog-ignore-db=performance_schema
binlog-ignore-db=information_schema
binlog-ignore-db=sys

symbolic-links=0

default-storage-engine=INNODB
max_connections=5000
max_user_connections   = 2000
max_allowed_packet= 1073741824
innodb_log_file_size=209715200
innodb_buffer_pool_size = 4G
character-set-server=utf8
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

[mysqld_safe]
log-error=/export/logs/mysql/mysql.log
pid-file=/var/run/mysql/mysql.pid
!includedir /etc/my.cnf.d
```



