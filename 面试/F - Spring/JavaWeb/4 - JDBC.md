#### 第一章： 一个JDBC实例

+   1 加载驱动
+   2 获取链接
+   3 创建sql语句
+   4 获得结果

```java
// 读取jdbc属性信息
InputStream resourceAsStream = JDBCTest.class.getClassLoader().getResourceAsStream("jdbc.properties");

Class.forName("com.mysql.jdbc.Driver"); // 1. load the driver
Connection connection = DriverManager.getConnection(URL, name, password); // 2 get the connection
Statement statement = connection.createStatement(); // 3. create sql statement
ResultSet resultSet = statement.executeQuery("select * from users"); // 4. get the result

while (resultSet.next()) {
    String s = "father:" + resultSet.getString("father") + " " + "," +
        "country:" + resultSet.getString("country");
    System.out.println(s);
}
```

这里可以直接用Class.forName的原因在于，Driver类在加载的时候，会编译静态代码块：

```java
static {
    try {
        DriverManager.registerDriver(new Driver()); // 自动帮助我们注册了
    } catch (SQLException var1) {
        throw new RuntimeException("Can't register driver!");
    }
}
```

+   完整代码

```java
package day01;


import org.junit.Test;

import java.io.*;
import java.sql.*;
import java.util.Properties;

public class JDBCTest {
    private static Properties prop = new Properties();

    @Test
    public void test1(){
        try {
            InputStream resourceAsStream = JDBCTest.class.getClassLoader().getResourceAsStream("jdbc.properties");

            prop.load(resourceAsStream);

            Class.forName(prop.getProperty("driverClass")); // 1. load the driver
            String URL = prop.getProperty("url");
            String name = prop.getProperty("user");
            String password = prop.getProperty("password");

            Connection connection = DriverManager.getConnection(URL, name, password);
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery("select * from users");

            while (resultSet.next()) {
                String s = "father:" + resultSet.getString("father") + " " + "," +
                        "country:" + resultSet.getString("country");
                System.out.println(s);
            }
        } catch (Exception e){
            e.printStackTrace();
        }
    }
}

```



#### 第二章：PrepareStatement实现CRUD操作

+   操作和访问数据库，java.sql包种由三个接口分别定义了堆数据库调用的不同方式：
    +   Statement：执行静态SQL语句，并返回它所生成结果的对象
    +   PreparedStatement：SQL语句被预编译并存储在此对象中，可以使用对象多次高效地执行改语句
    +   CallableStatement：执行存储过程
+   Statement
    +   缺点一：sql语句拼接繁杂
    +   缺点二：存在sql注入的危险
+   PreparedStatement
    +   优点一：对sql进行预编译，编译好的数据存放在内存中，效率高。
    +   防止sql注入



#### 第三章：用反射构建数据库数据所对应的对象

利用两项技术：

+   反射机制
+   resultSet 元数据

```java

public class JDBCTest {
    private static Properties prop = new Properties();

    @Test
    public void test1(){
        try {
            InputStream resourceAsStream = JDBCTest.class.getClassLoader().getResourceAsStream("jdbc.properties");

            prop.load(resourceAsStream);

            Class.forName(prop.getProperty("driverClass")); // 1. load the driver
            String URL = prop.getProperty("url");
            String name = prop.getProperty("user");
            String password = prop.getProperty("password");

            Connection connection = DriverManager.getConnection(URL, name, password);
            // 1. prev compile sql
            PreparedStatement preparedStatement = connection.prepareStatement("select * from users;");
            ResultSet resultSet = preparedStatement.executeQuery();
            ResultSetMetaData rsmd = resultSet.getMetaData();
            int columCount = rsmd.getColumnCount();
            ArrayList<User> uList = new ArrayList<User>();

            // 反射
            while (resultSet.next()) {
                User u = new User(); // use reflection to create the User object
                for (int i = 0; i < columCount; i++) {
                    Object object = resultSet.getObject(i + 1);
                    String columnName = rsmd.getColumnName(i + 1);
                    Field declaredField = User.class.getDeclaredField(columnName);
                    declaredField.setAccessible(true);
                    declaredField.set(u,object);
                }
                uList.add(u);
            }
            Object[] objects = uList.toArray();
            for (Object u: objects) {
                System.out.println((User) u);
            }
        } catch (Exception e){
            e.printStackTrace();
        }
    }
}

class User {
    private int u_id;
    private String father;
    private String country;

    User() {}
    User(int id,String father,String country) {
        this.u_id = id;
        this.father = father;
        this.country = country;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + u_id +
                ", father='" + father + '\'' +
                ", country='" + country + '\'' +
                '}';
    }
}

```

#### 第四章

三个重点：

+   使用PrepareStatement，而不是Statement
+   使用addPatch进行批量处理
+   设置setAutoCommit，禁止自动提交