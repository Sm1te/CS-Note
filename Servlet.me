# Servlet

## 1. 概念

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。



![Servlet 架构](https://www.runoob.com/wp-content/uploads/2014/07/servlet-arch.jpg)

**Servelet Job:**

Servlet 执行以下主要任务：

- 读取客户端（浏览器）发送的显式的数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。
- 读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。
- 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应。
- 发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
- 发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。





**结构：**

![image-20220302104400478](C:\Java_Study\General Node\img\servlet_structure.png)

###### ![image-20220302104723627](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220302104723627.png)



## 2. 如何编写

![image-20220302104602388](C:\Java_Study\General Node\img\Hellow_servelet.png)

## 3. Mapping

![image-20220302104650786](C:\Java_Study\General Node\img\servlet_mapping_1.png)

```xml
<!--一个Servlet可以指定多个或者一个映射（name 一样, pattern不同）-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello2</url-pattern>
</servlet-mapping>

<!--通用映射路径-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello/*</url-pattern>
</servlet-mapping>

<!--默认请求路径-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/*</url-pattern>
</servlet-mapping>

<!--可以自定义后缀实现请求映射
注意点，*前面不能加项目映射的路径
hello/sajdlkajda.qinjiang
-->
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>*.robin</url-pattern>
</servlet-mapping>

指定了固有的映射路径优先级最高，如果找不到就会走默认的处理请求

```





**实例：**

```java
// 导入必需的 java 库
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

// 扩展 HttpServlet 类
public class HelloWorld extends HttpServlet {
 
  private String message;

  public void init() throws ServletException
  {
      // 执行必需的初始化
      message = "Hello World";
  }

  public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
  {
      // 设置响应内容类型
      response.setContentType("text/html");

      // 实际的逻辑是在这里
      PrintWriter out = response.getWriter();
      out.println("<h1>" + message + "</h1>");
  }
  
  public void destroy()
  {
      // 什么也不做
  }
}
```



## 4. ServletContext 

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的 web应用；



![image-20220302201129653](C:\Java_Study\General Node\img\Servlet_context_1.png)

设置两个class并且在web.xml中都加上

```xml
<!--同时获取-->
ServletContext context = this.getServletContext();
<!--Set-->
ServletContext context = this.getServletContext();
String username = "Robin"; //数据
context.setAttribute("username",username); //将一个数据保存在了ServletContext中，名字为：username 。值 username


```

```xml
<servlet>
<servlet-name>hello</servlet-name>
<servlet-class>com.robin.servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>hello</servlet-name>
<url-pattern>/hello</url-pattern>
</servlet-mapping>
<servlet>
<servlet-name>getc</servlet-name>
<servlet-class>com.robin.servlet.GetServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>getc</servlet-name>
<url-pattern>/getc</url-pattern>
</servlet-mapping>

```



**请求转发**

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ServletContext context = this.getServletContext();
    System.out.println("进入了ServletDemo04");
    //RequestDispatcher requestDispatcher =context.getRequestDispatcher("/gp"); //转发的请求路径
    //requestDispatcher.forward(req,resp); //调用forward实现请求转发；
    context.getRequestDispatcher("/gp").forward(req,resp);
}

```



**读取资源文件**

Properties 

在java目录下新建properties 

在resources目录下新建properties 

发现：都被打包到了同一个路径下：classes，我们俗称这个路径为classpath: 

思路：需要一个文件流；

```java
public class ServletDemo05 extends HttpServlet {
	@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        InputStream is = this.getServletContext().getResourceAsStream("/WEBINF/classes/com/kuang/servlet/aa.properties");
        
        Properties prop = new Properties();
        prop.load(is);
        
        String user = prop.getProperty("username");
        String pwd = prop.getProperty("password");
        
        resp.getWriter().print(user+":"+pwd);
   }

```

