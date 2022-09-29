#### JSP 处理

+   普通网页，浏览器发送一个 HTTP 请求给服务器
+   Web服务器识别JSP网页的请求，并将请求传递给JSP引擎，
+   JSP引擎从磁盘中载入JSP文件，并将转化为Servlet
+   JSP 引擎将 Servlet 编译成可执行类，并且将原始请求传递给 Servlet 引擎
+   Web 服务器的某组件将会调用 Servlet 引擎，然后载入并执行 Servlet 类。在执行过程中，Servlet 产生 HTML 格式的输出并将其内嵌于 HTTP response 中上交给 Web 服务器
+   Web 服务器以静态 HTML 网页的形式将 HTTP response 返回到您的浏览器中。
+   最终，Web 浏览器处理 HTTP response 中动态产生的HTML网页，就好像在处理静态网页一样

![img](../../../LeetCode刷题/images/jsp-processing.jpg)

#### JSP声明周期

+   编译阶段：JSP引擎编译文件。当浏览器请求JSP页面时，JSP引擎会首先去检查是否需要编译这个文件。如果这个文件没有被编译过，或者在上次编译后被更改过，则编译这个JSP文件。
    +   解析JSP文件
    +   将JSP文件转为Servlet
    +   编译Servlet
+   JSP初始化
    +   容器载入JSP文件后，它会在为请求提供任何服务前调用jspInit()方法。如果您需要执行自定义的JSP初始化任务，复写jspInit()方法就行了。
+   JSP执行
    +   这一阶段描述了JSP生命周期中一切与请求相关的交互行为，直到被销毁。当JSP网页完成初始化后，JSP引擎将会调用_jspService()方法。_jspService()方法需要一个HttpServletRequest对象和一个HttpServletResponse对象作为它的参数，就像下面这样：
+   JSP清理
    +   JSP生命周期的销毁阶段描述了当一个JSP网页从容器中被移除时所发生的一切。jspDestroy()方法在JSP中等价于servlet中的销毁方法。当您需要执行任何清理工作时复写jspDestroy()方法，比如释放数据库连接或者关闭文件夹等等。

#### JSP语法

+   JSP声明

    ```jsp
    <%! declaration; [ declaration; ]+ ... %>
    
    <%! int i = 0; %> 
    <%! int a, b, c; %> 
    <%! Circle a = new Circle(2.0); %> 
    ```

#### JSP 本质上就是一个Servlet

```java
public abstract class HttpJspBase extends HttpServlet implements HttpJspPage {
    // ...
}
```

