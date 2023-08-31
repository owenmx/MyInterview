## 关键字

#### static 关键字

static关键字的提出在于满足这样一个需求：

```latex
有时候我们希望无论是否产生对象，或者，无论产生了多少对象的情况下，某些特定的数据在内存空间中值有一份。
```

static可以用来修饰：属性、方法、代码块、内部类。

#### static修饰属性

实例变量和静态变量，实例变量随着对象的创建而加载，静态变量随着类的加载而加载。静态变量存放在`方法区`中。

#### static修饰方法

实例方法和静态方法。静态方法无法调用实例变量。从`对象的声明周期的角度`去思考问题。

#### final 关键字

final应用于类、方法和变量时表示的含义是不同的，但本质都是表示不可改变，不可改变并不代表不可赋值，再获得值之后，数据就不可变化了。

#### final修饰类

表示当前类不可再被继承。

```java
final class Dog { // 
    
}
```

#### final修饰方法

表示当前方法不可以被重写。

```java
class Animal {
    public final void bark() {}
}
class Dog extends Animal{
    @Override // 报错，无法重写
    public void bark() {
        
    }
}
```

#### final修饰变量

用final修饰变量，表明该变量转化为`常量`。对属性可以赋值的位置：

+   默认初始化
+   显式初始化/在代码块种赋值
+   构造器种初始化
+   有了对象之后，通过“对象.属性”进行操作

对于这种变量初始化的问题，最好的方法是从**生命周期**的角度来考虑这种问题。

static final用来修饰属性，全局常量。

```java
    final int age;
    {
        age = 12;
    }

    final static int nation;
    static { // 静态代码块
        nation = 12;
    }
```

## 设计模式

#### 单例（Singleton）设计模式

设计模式是在实践和理论化之后优选的代码结构、编程风格以及解决问题的思考方式。

单例设计模式就是保证在整个软件系统种对某个类只能存在一个对象实例。

```java
// 饿汉式，y
class Bank {
    private static Bank instance = new Bank(); // 2. 内部类创建类的对象, 4. 要求对象也必须为静态的
    private Bank() { // 1. 私有化类的构造器，避免外部调用构造器
    }
    public static Bank getInstance() { // 3. 提供公共的静态的方法，返回类的对象
        return instance;
    }
}

// 懒汉式，用到的时候才创建
class Bank {
    private static Bank instance = null; // 2. 内部类创建类的对象, 4. 要求对象也必须为静态的
    private Bank() { // 1. 私有化类的构造器，避免外部调用构造器
    }
    public static Bank getInstance() { // 3. 提供公共的静态的方法，返回类的对象
        if (instance == null) {
            instance = new Bank();
        }
        return instance;
    }
}

// 懒汉式，线程是安全的
class Bank {
    // 懒汉式
    private static Bank instance = null; // 2. 内部类创建类的对象, 4. 要求对象也必须为静态的
    private Bank() { // 1. 私有化类的构造器，避免外部调用构造器
    }
    public static Bank getInstance() { // 3. 提供公共的静态的方法，返回类的对象 DCL
        if (instance == null) {
            synchronized (Bank.class) {
                if (instance == null) {
                    instance = new Bank();
                }
            }
        }
        return instance;
    }
}
```

#### 懒汉式和饿汉式的区别

饿汉式：

+   好处：饿汉式是线程安全的
+   坏处：对象声明周期过长

懒汉式：

+   好处：延迟对象的创建
+   坏处：懒汉式是线程不安全的



## 理解main方法

#### java-main [🔗](https://juejin.cn/post/6976678093024919560)

英文解释：

```text
The java tool launches a Java application. It does this by starting a Java runtime environment, loading a specified class, and invoking that class's main method.
```



main是程序的入口，在程序运行的时候，第一个执行的方法就是main方法。

```java
import java.util.Arrays;

public class MainTest {
    public static void main(String[] args) {
        Main.main(new String[10]);
    }
}

class Main {
    public static void main(String[] args) {
        System.out.println(Arrays.toString(args));
    }
}

```

![Java Main方法都写过，JVM如何调用它的你知道吗？都在这张图里了](../../LeetCode刷题/images/aHR0cDovL3A5LnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlL2FhYzkxMGM5ZGUzMTRmZDM4MDcwNTA2NGFlMjlmM2Q1)

Java运行的时候，通常通过`java hello.class` 进行运行，而对应的`java`代表了一个可执行程序，这个可执行程序会执行`src/share/tools/launcher/java.c` 文件中的`main`方法，从而创建一个JVM线程来执行java程序。

而创建完线程之后，通过JNI的反向调用，获取java程序中的`main`方法：

```c
mainID = (*env)->GetStaticMethodID(env, mainClass, "main",
                                       "([Ljava/lang/String;)V");
```

 接下来的逻辑就是我们日常了解到的JVM加载、链接和初始化类和接口的流程。

## 代码块

#### 作用

```java
静态代码块（类加载时调用） ---> 构造代码块（创建方法时调用）--->构造函数 ---> 普通代码块（类中方法的方法体）
```



用{}括起来的称为代码块，四种代码块：

-   普通代码块：类中方法的方法体
-   **构造代码块**：类中**{}**直接括起来的语句，**每次创建对象都会被调用**，**先于构造函数执行**
-   **静态代码块**：类中`static{}`括起来的语句，**只执行一次**，**先于构造代码块块执行**
-   同步代码块：类中`synchronized(){}`括起来的语句，多线程环境下互斥执行

用来初始化类或者对象

#### 静态代码块 v.s. 代码块

静态代码块随着类的加载而执行，而且只会执行一次。

代码块每创建一个对象就执行一次非静态代码块。

```java
static {
    
}

{
    
}
```

```java
public class finalizeTest {
    public static void main(String[] args) {
        Person p1 = new Person();
        Person p2 = new Person();
    }
}

class Person {
    static int count=0;
    static {
        System.out.println("静态代码块");
    }
    {
        System.out.println("非静态代码块");
    }
}

```

#### 执行顺序

+   1.  默认初始化
+   2.  显示初始化/在代码块种赋值
+   3.  构造器种初始化
+   4.  有了对象后，再进行赋值

## 抽象类和抽象方法

#### 抽象类的具体语法

```java
<abstract> class <className> {
    <abstract> <type> <methodName> (param-list);
}
```

其中，abstract 表示该类或该方法是抽象的；class_name 表示抽象类的名称；method_name 表示抽象方法名称，parameter-list 表示方法参数列表。

#### 抽象类的3个特征

+   抽象方法没有方法体
+   抽象方法必须存在于抽象类中，抽象类也可以包含非抽象方法
+   子类重写父类时，必须重写父类所有的抽象方法

#### 抽象类的定义和使用规则

1.  抽象类和抽象方法都要使用 abstract 关键字声明。
2.  如果一个方法被声明为抽象的，那么这个类也必须声明为抽象的。而一个抽象类中，可以有 0~n 个抽象方法，以及 0~n 个具体方法。
3.  抽象类不能实例化，也就是不能使用 new 关键字创建对象。

#### 匿名类

```java
// Person 抽象父类
Person p =  new Person() {
    @Override
    public void eat() {
        System.out.println("吃");
    }
    
    @Override
    public void walk() {}
}
```

#### 模板方法设计模式

**抽象类体现的就是一种模板模式的设计**，抽象类作为多个子类的通用模板，子类再抽象类的基础上进行扩展、改造，但子类总体上会保留抽象类的行为方式。

```java
public class TemplateTest {
    public static void main(String[] args) {
        Template t = new Template() {
            @Override
            public void code() {
                int sum = 0;
                for (int i = 0; i < 1000000; i++) {
                    sum += i;
                }
                System.out.println(sum);
            }
        };

        t.spendTime();
    }
}

abstract class Template {
    public void spendTime() {
        long start = System.currentTimeMillis();
        this.code();
        long end = System.currentTimeMillis();
        System.out.println("Time: " + (end-start));
    }

    abstract public void code();
}
```

也成为钩子函数或者回调方法。

## 接口

#### 定义

Java不支持多重继承，但是可以通过接口实现多继承的效果。

继承是一种**是不是**的关系，而接口是一种**能不能**的关系，在Java种，接口和类是并列的两个结构。

抽象类是从多个类种抽象出来的模板，如果将这种抽象进行得更加彻底，则可以提炼出：接口。

接口可以认为是特殊的类，不同的是接口的成员**没有执行体**，由**全局常量和公共的抽象方法**组成。

#### 接口的语法规则

jdk7之前：只能定义全局常量和抽象方法

+   全局常量：public static final，如果不指出，则默认为这种格式。
+   抽象方法：public abstract

```java
[public] interface interface_name [extends interface1_name[, interface2_name,…]] {
    // 接口体，其中可以包含定义常量和声明方法
    [public] [static] [final] type constant_name = value;    // 定义常量
    [public] [abstract] returnType method_name(parameter_list);    // 声明方法
}
```

#### 接口与多态性

+   如何体现多态性

```java

public class USBTest {
    public static void main(String[] args) {
        Computer c = new Computer();
        c.read_data(new Flash());
    }
}

class Computer {
    public void read_data(USB usb) {
        usb.start();
        usb.end();
    }
}

interface USB {
    void start();
    void end();
}

class Flash implements USB {

    @Override
    public void start() {
        System.out.println("Flash 开始读取数据");
    }

    @Override
    public void end() {
        System.out.println("Flash 结束读取数据");
    }
}
```

+   匿名对象/非匿名对象匿名类/匿名对象匿名类

#### 代理模式

代理设计就是为其他对象提供一种代理以控制对这个对象的访问。

```java

public class NetworkTest {
    public static void main(String[] args) {
        new ProxyServer(new Server()).browse();
    }
}

interface Network{
    void browse();
}

class Server implements Network {

    @Override
    public void browse() {
        System.out.println("服务器浏览数据");
    }
}

class ProxyServer implements Network{
    private Network nt;
    ProxyServer(Network nt) {
        this.nt = nt;
    }
    public void check() {
        System.out.println("检查信息");
    }

    @Override
    public void browse() {
        check();
        this.nt.browse();
    }
}
```



## 内部类

#### 成员局部类

一方面，作为外部类的成员，

+   调用外部类的结构
+   可以被static修饰

另一方面，作为一个类出现：

+   类内可以定义属性，方法，构造器
+   可以被final修饰，此类不能被继承

```java
class Person {
    class Bird {} // 非静态成员类
    static class Dog{} // 静态成员类
}
```

#### 局部方法类

```java
interface Comparable {
    void calculate();
}
// 局部方法类
class Calculate {
    public Comparable getComparator(){
        class MyCom implements Comparable {
            @Override
            public void calculate() {

            }
        }
        return new MyCom();
    }
}

// 匿名类
class Calculate {
    public Comparable getComparator(){
        return new Comparable() {
            @Override
            public void calculate() {
            }
        };
    }
}
// lambda，一般需要和接口同步使用
class Calculate {
    public Comparable getComparator(){
        return () -> {
            System.out.println("hello");
        };
    }
}

```

