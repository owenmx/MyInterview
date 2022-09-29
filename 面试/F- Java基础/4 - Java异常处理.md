## 异常

#### 异常概述与异常体系系统

在Java语言种讲执行种发生的不正常情况称为异常，逻辑错误不属于异常。

![img](../../LeetCode刷题/images/3-1Q0231H424V1-1624191202492.jpg)

Throwable 类是所有异常和错误的超类，下面有 Error 和 Exception 两个子类分别表示错误和异常。其中异常类 Exception 又分为运行时异常和非运行时异常，这两种异常有很大的区别，也称为不检查异常（Unchecked Exception）和检查异常（Checked Exception）。

-   Exception 类用于用户程序可能出现的异常情况，它也是用来创建自定义异常类型类的类。
-   Error 定义了在通常环境下不希望被程序捕获的异常。一般指的是 JVM 错误，如堆栈溢出。

表 1 Java中常见运行时异常

|           异常类型            |                         说明                          |
| :---------------------------: | :---------------------------------------------------: |
|      ArithmeticException      |              算术错误异常，如以零做除数               |
| ArraylndexOutOfBoundException |                     数组索引越界                      |
|      ArrayStoreException      |              向类型不兼容的数组元素赋值               |
|      ClassCastException       |                     类型转换异常                      |
|   IllegalArgumentException    |                 使用非法实参调用方法                  |
|     lIIegalStateException     |            环境或应用程序处于不正确的状态             |
|  lIIegalThreadStateException  |           被请求的操作与当前线程状态不兼容            |
|   IndexOutOfBoundsException   |                  某种类型的索引越界                   |
|     NullPointerException      |          尝试访问 null 对象成员，空指针异常           |
|  NegativeArraySizeException   |                再负数范围内创建的数组                 |
|     NumberFormatException     | 数字转化格式异常，比如字符串到 float 型数字的转换无效 |
|    TypeNotPresentException    |                      类型未找到                       |

表 2 Java常见非运行时异常

|           异常类型           |            说明            |
| :--------------------------: | :------------------------: |
|    ClassNotFoundException    |         没有找到类         |
|    IllegalAccessException    |        访问类被拒绝        |
|    InstantiationException    | 试图创建抽象类或接口的对象 |
|     InterruptedException     |    线程被另一个线程中断    |
|     NoSuchFieldException     |       请求的域不存在       |
|    NoSuchMethodException     |      请求的方法不存在      |
| ReflectiveOperationException |   与反射有关的异常的超类   |

#### try-catch-finally块

![img](../../\LeetCode刷题\images\3-1Q024110159364.jpg)

#### java throws和throw 声明与抛出异常

+   throws 声明异常

```java
import java.io.FileInputStream;
import java.io.IOException;

public class Test04 {
    public void readFile() throws IOException {
        // 定义方法时声明异常
        FileInputStream file = new FileInputStream("read.txt"); // 创建 FileInputStream 实例对象
        int f;
        while ((f = file.read()) != -1) {
            System.out.println((char) f);
            f = file.read();
        }
        file.close();
    }

    public static void main(String[] args) {
        Throws t = new Test04();
        try {
            t.readFile(); // 调用 readFHe()方法
        } catch (IOException e) {
            // 捕获异常
            System.out.println(e);
        }
    }
}
```

+   throw自定义异常

```java
public class ExceptionTest {
    public static void main(String[] args) {
        try {
            test(90);
        } catch (IllegalThreadStateException e) {
            e.printStackTrace();
        }
    }
    public static void test(int val) {
        if (val < 10) {
            throw new IllegalThreadStateException("错误");
        } else{
            System.out.println(val);
        }
    }
}

```

+   两者的区别
    +   throw生成一个异常对象，并抛出，使用在方法内部。
    +   throws处理异常的方式，使用在方法声明出的末尾。
    +   两者是前后的关系。

