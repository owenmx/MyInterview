#### JavaWeb的概念

+   基于请求响应的格式

#### Web资源

![tomcat容器模型](../../../LeetCode刷题/images/SouthEastsasas)

+   静态资源：html，jpg等

+   动态资源asp，jsp，servlet等

    

    

#### 动态访问

![image-20210513151413170](../../../LeetCode刷题/images/image-20210513151413170.png)



#### Tomcat有四个容器

![img](../../../LeetCode刷题/images/v2-5115b768bda2b4fb17c0faced93a07ba_720w.jpg)

#### Tomcat架构平视图

+   第一步，通过Connector构建Request。具体表现：
    1.  在Connector类中调用`setProtocol`来设置对应的传输协议，从而加载 `org.apache.coyote.http11.Http11Protocol`。
    2.  在`Http11Protocol`中，初始化构建`EndPoint`，建立socket通信，获取、解析并返回数据。
    3.  Connector类利用解析的数据，抵用`createRequest`创建`request`。
+   第二步骤，通过四个容器类（Engine、Host、Context和Wrapper），对Request进行处理，最后找到指定的Servlet进行执行。

![image-20210808170648677](../../../LeetCode刷题/images/image-20210808170648677.png)

#### Tomcat如何处理一个请求

![img](../../../LeetCode刷题/images/v2-cf795c8322e5c13cfed76b04ac676868_720w.jpg)

![img](../../../LeetCode刷题/images/20190419095143573.png)

#### SpringBoot如何启动Tomcat

主要通过`refreshContext`函数，刷新上下文来创建服务器。

![未命名文件 (1)](../../../LeetCode刷题/images/未命名文件 (1).png)

