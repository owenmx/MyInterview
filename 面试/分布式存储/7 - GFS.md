https://www.cnblogs.com/xybaby/p/8967424.html

https://kb.cnblogs.com/page/174130/

#### 0. 介绍

GFS的基本架构：GFS master、GFS Client、GFS chunkserver





### 1. 搜索引擎的基本架构

GFS$\rightarrow$数据系统(Big Table)$\rightarrow$算法(MapReduce)





### 2. 如何保存一个大文件

通过索引加上偏移量用来寻找文件



### 3. 如何保存一个超大文件

通过master对索引信息和偏移信息进行保存，利用这些信息对数据进行索引，将数据存储在client服务器上



### 4.  如何减少Master的数据和流量

将数据之间的耦合性降低，master只保存client机器的索引信息，而偏移量放在client数据上。

优点：减少master存储的信息，减少通信。





### 5. 发现数据损坏

读取一块数据对checksum进行验证



### 6. 如何减少ChunkServer挂断带来的损失

创建副本（3个）；选择server时，选择跨机架跨中心的服务器。



### 7. 如何恢复损坏的chunk

启动修复进程



#### 8. 如何发现ChunkServer挂掉

心跳



#### 9. 应对热点

当副本过度繁忙，复制到更多的chunkserver；基于chunkserver的硬盘和带宽利用率来选择。

![image-20211012111045620](../../LeetCode%E5%88%B7%E9%A2%98/images/image-20211012111045620.png)

#### 10. 如何读取文件

![image-20210227184842670](../../\LeetCode刷题\images\image-20210227184842670.png)



#### 11. 如何写数据

![image-20210227185204795](../../\LeetCode刷题\images\image-20210227185204795.png)

奥卡姆剃刀原则