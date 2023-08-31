## Volatile关键字

#### 1. Volatile可见性的验证

```java
public class JMMTest {
    private static int tickets = 0;
    public static void main(String[] args) throws InterruptedException {
        new Thread() {
            @Override
            public void run() {
                System.out.println("Thread 1 prepare ...");
                while (tickets == 0) {
                }
            }
        }.start();
		
        // 保证线程1先运行
        Thread.currentThread().sleep(2000);

        new Thread() {
            @Override
            public void run() {
                System.out.println("Thread 2 prepare ...");
                tickets = 20;
                System.out.println("Thread 2 modified ...");
            }
        }.start();
    }
}
```

#### 2. Volatile不保证原子性

```java
public class JMMTest {
    private static volatile int count = 0;
    public static void main(String[] args) throws InterruptedException {
        // 建立20个线程，每个线程求和，整体应该是20,000
        for (int i = 0; i < 20; i++) {
            new Thread() {
                @Override
                public void run() {
                    for (int i = 0; i < 1000; i++) {
                        count += 1;
                    }
                }
            }.start();
        }
        // 保证前面的线程全部结束
        while (Thread.activeCount() > 2) {
            Thread.yield();
        }
		
        // 输出的结果小于等于20000
        System.out.println(count);
    }
}
```

对于这样一个操作`count++;`中，可以被转变为：

```shell
2: getfiled # 获取值
5: iconst_1 
6: iadd # 进行加一操作
7: putfiled # 获取值
```

**解决方法：**

+   Synchronized方法
+   利用AtomicInteger下面的方法





#### 3. Volatile 禁止指令重排

volatile通过在指令间插入一条Memory Barrier则会高数编译器和CPU，不管什么指令都不能和这条Memory Barrier指令重排序，也就是说通过插入内存屏障禁止在内存屏障前后的指令执行重排序优化（包括load屏障和store屏障）。

单线程环境中，处理器在进行指令重排时，需要考虑**数据依赖**问题。

多线程环境中，由于编译器优化重排的存在，两个线程中使用后的变量能否保证一致性是无法确认的。

```java
class Resort {
    int a = 0;
    boolean flag = false;
    
    public void method1() {
        a = 1;
        flag = true; // 多线程环境下如果发生指令重排，并且另外一个线程强占了，会发生a=5的情况。
    }
    
    public void method2() {
        if (flag) {
            a = a + 1;
            System.out.println("Return value" + a);
        }
    }
}
```





#### 4. Volatile 单例模式

由于存在指令重排，双检锁的模式可能存在问题。

```java
public class SingletonTest {
    private static SingletonTest instance; // 在这里引入volatile -->
    // private static volatile SingletonTest instance; // 在这里引入volatile,禁止指令重排
    private SingletonTest() {
        System.out.println("构建了一个对象");
    }

    public static SingletonTest getInstance() {
        if (instance == null) {
            synchronized (SingletonTest.class) {
                if (instance == null) {
                    instance = new SingletonTest();
                }
            }
        }
        return instance;
    }

    public static void main(String[] args) {
        for (int i = 0; i < 1000; i++) {
            new Thread() {
                @Override
                public void run() {
                    SingletonTest.getInstance();
                }
            }.start();
        }
    }
}
```

