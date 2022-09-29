#### 字符串相关的类

##### String类以及常用方法

+   String是一个final类，代表**不可变的字符序列**。

+   String实现了Serializable接口：表示字符串可以支持序列化。

+   String实现了Comparable接口：表示字符串可以比较大小。

+   char数组和字符串转换

    ```java
    // 字符串转换为char数组
    char charData[] = stringData.toCharArray();
    
    // char数组转换成字符串
    String stringData = String.valueOf(charData);
    ```

+   byte数组和字符串转换

```java
byte [] bytes = str.getBytes("gbk"); // 将字符串转化为二进制编码
String val = new String(bytes,"gbk");
```



+   在Srting操作中，如果出现了s1+"124"操作，只要有一个是变量，那就会在对空间中申请变量

    ```java
    // 常量池
    String s8 = s1.intern();  //返回的元素在常量池中
    ```


#### JVM内存解析

一个JVM实例指存在一个堆内存，堆内存的大小是可以调节的。类加载器读取了类文件后，需要包类、方法、常变量放到堆内存中。



##### StringBuffer、StringBuilder、String

String是不可变字符序列，底层采用char[]存储

StringBuffer可变的字符序列，底层采用char[]存储

StringBuilder可变的字符序列，底层采用char[]存储，线程安全。

+   区别一：线程安全

StringBuffer是线程安全的，StringBuilder不是。

+   区别二：缓冲区



+   区别三：性能

StringBuilder最优，StringBuffer次之，String最后



#### Java比较器

实现Comparable必须实现CompareTo方法，如果当前对象this是大于形参对象obj则返回正整数；如果当前对象小于形参对象obj，则返回负整数；如果两者相等，则返回0。一定要理解这是在比较this对象和obj对象。如下所示：

```java
Arrays.sort(data, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return 0;
    }
});
```



#### System类





#### Math类





#### BigInteger和BigDecimal



