## Lambda

#### Lambda 方法

lambda本质就是接口的实例，必须依赖于接口。

```java
public class LambdaTest {
    @Test
    public void test1() {
        MyCompare mc = (a,b) -> {
            return a + b;
        };
        System.out.println(mc.sum(123, 31));
    }
}
interface MyCompare {
    int sum(int a,int b);
}
```

#### 函数式接口

只包含一个抽象方法的接口，称为函数式接口。

```java
@FunctionalInterface
public interface MyInter {
    void method();
}
```



#### 方法引用

有三种形式：

+   类 : 静态方法
+   类 : 非静态方法
+   对象 : 非静态方法，实例方法

```java
MyCompare mc =  Interger: sum;
```



#### 构造器引用

```java
Supplier<Employee> sup1 = () -> new Employee();
Supplier<Employee> sup2 = Employee : new;
```



## StreamAPI

#### 介绍

利用StreamAPI对集合数据进行操作，类似于使用SQL执行的数据库查询。

+   Stream关注的式对数据的运算，与CPU打交道；集合关注二点数据的存储，与内存打交道
+   Stream自己不存储元素，相反他们会返回一个新的Stream
+   Stream操作是延迟的，只有得到结果时才会执行
+   执行流程
    +   1 实例化
    +   2 执行操作
    +   3 终止

#### 创建Stream的四种方式

+   通过Arrays创建Stream ✔

```java
int data[] = {1,23,3,432};
IntStream stream = Arrays.stream(data);

// IntStream data = (IntStream) Arrays.stream(new Integer[]{1,2,3}); 
```

+   通过collections的stream创建 ✔

```java
ArrayList<Integer> data = new ArrayList();
data.add(13);
data.add(14);
data.stream();
```

+   通过Stream.of建造

```java
IntStream data = (IntStream) Stream.of(1,2,3);
data.filter(e -> !(e <= 13)).forEach(System.out::println);
```

+   创建无限流

```java
Stream.generate(Math::random).limit(10).forEach(System.out::println);
```

#### Java 8 与 Java 7 的区别

+   语言层面，引入stream API。
+   架构层面，对JVM做了改进。



#### Stream 支持的中间操作

Stream 关注的是数据的计算与CPU打交道

一个中间操作链条，`Stream --> filter --> map --> sort --> forEach(System.std::println)`

+   **filter 操作**

    ```java
    // 过滤特定条件的数据
    int []s1 = {1,23,3,432};
    IntStream stream = Arrays.stream(s1);
    stream.filter(e -> e > 20).forEach(System.out::println);
    ```

+   **limit操作**

    ```java
    // 限制查询数据的条数
    int []s1 = {1,23,3,432};
    IntStream stream = Arrays.stream(s1);
    stream.limit(2).forEach(System.out::println);
    ```

+   **map操作**

    ```python
    public class App {
        public static void main(String[] args) {
            /* 数组 & Arrays集合类操作 */
            int []s1 = {1,23,23,432,23};
            IntStream stream = Arrays.stream(s1);
            stream.mapToObj(Util::addNumber).forEach(System.out::println);
        }
    }
    
    class Util {
        public static String addNumber(int age) {
            if(age > 10) {
                return "young";
            } else {
                return "baby";
            }
        }
    }
    ```

+   **sorted操作**

    ```python
    public class App {
        public static void main(String[] args) {
            ArrayList<User> users = new ArrayList<>();
            users.add(new User(10));
            users.add(new User(27));
            users.add(new User(9));
            users.stream().sorted((e1,e2) -> Integer.compare(e1.getAge(),e2.getAge())).forEach(System.out::println);
            
            ArrayList<Integer> ints = new ArrayList<>();
            ints.add(2);
            ints.add(0);
            ints.add(23);
            ints.add(21);
            ints.stream().sorted((e1,e2) -> -Integer.compare(e1,e2)).forEach(System.out::println);
        }
    }
    ```



#### Stream的规约操作

+   anyMatch allMatch

    ```java
    ArrayList<User> users = new ArrayList<>();
    users.add(new User(10));
    users.add(new User(27));
    users.add(new User(9));
    
    boolean flag1 = users.stream().allMatch(e -> e.getAge() > 18);
    System.out.println(flag1);
    
    boolean flag2 = users.stream().anyMatch(e -> e.getAge() > 18);
    System.out.println(flag2);
    
    boolean flag3 = users.stream().noneMatch(e -> e.getAge() == 18);
    System.out.println(flag2);
    
    // 什么是 Optional
    Optional<User> first = users.stream().findFirst();
    System.out.println(first);
    ```

+   reduce操作，`reduce(0,(e1,e2)->e1+e2)` 表示从0开始进行累加。

    ```java
     Integer reduce = 
         users.stream().filter(e -> e.getAge() <= 20).map(e -> e.getAge()).reduce(0, (e1, e2) -> e1 + e2);
            System.out.println(reduce);
    ```



#### Stream的收集操作

+   Collectors 收集工具类，操作的对象必须是类，而不能是基本数据类型。

```java
List<User> collect = users.stream().filter(e -> e.getAge() < 10).collect(Collectors.toList());
System.out.println(collect);
```





## 新增的方向

+   垃圾回收算法
+   针对流式计算新增的接口
+   Optional方法（避免空指针）
+   更好地适应鸭子类型
