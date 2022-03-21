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



## 5.HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表**响应**的一个HttpServletResponse； 

如果要获取客户端请求过来的参数：找HttpServletRequest 

如果要给客户端响应一些信息：找HttpServletResponse



### 5.1分类

负责向浏览器发送数据的方法

```java
ServletOutputStream getOutputStream() throws IOException;
PrintWriter getWriter() throws IOException;
```

负责向浏览器发送响应头的方法

```java
void setCharacterEncoding(String var1);
void setContentLength(int var1);
void setContentLengthLong(long var1);
void setContentType(String var1);
void setDateHeader(String var1, long var2);
void addDateHeader(String var1, long var2);
void setHeader(String var1, String var2);
void addHeader(String var1, String var2);
void setIntHeader(String var1, int var2);
void addIntHeader(String var1, int var2);
```

响应的状态码

1. [Informational responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#information_responses) (`100`–`199`)
2. [Successful responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses) (`200`–`299`) 请求成功
3. [Redirection messages](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#redirection_messages) (`300`–`399`) 请求重定向
4. [Client error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses) (`400`–`499`) 找不到资源
5. [Server error responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (`500`–`599`) 服务器代码错误



### 5.2应用

1. 向浏览器输出消息

2. 下载文件

   1. 获取下载文件的路径
   2. 下载的文件名
   3. 设置让浏览器可以下载我们的东西
   4. 获取下载文件的输入流
   5. 输出
      1. 创建缓冲区
      2. 获得OutputSteam对象
      3. 将FileOutputStream流写入buffer缓冲区
      4. 使用OutputStream将缓冲区中的数据输出到客户端

   

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 1. 要获取下载文件的路径
    String realPath = "the real path of your file";
    System.out.println("path："+realPath);
    // 2. 下载的文件名是啥？
    String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
    // 3. 设置想办法让浏览器能够支持(Content-Disposition)下载我们需要的东西,中文文件名URLEncoder.encode编码，否则有可能乱码
    resp.setHeader("ContentDisposition","attachment;filename="+URLEncoder.encode(fileName,"UTF-8"));
    // 4. 获取下载文件的输入流
    FileInputStream in = new FileInputStream(realPath);
    // 5. 创建缓冲区
    int len = 0;
    byte[] buffer = new byte[1024];
    // 6. 获取OutputStream对象
    ServletOutputStream out = resp.getOutputStream();
    // 7. 将FileOutputStream流写入到buffer缓冲区,使用OutputStream将缓冲区中的数据输出到客户端！
    while ((len=in.read(buffer))>0){
    	out.write(buffer,0,len);
    }
    in.close();
    out.close();
}
```

### 5.3验证码

```java
public class ImageServlet extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)throws ServletException, IOException {
        //如何让浏览器3秒自动刷新一次;
        resp.setHeader("refresh","3");
        //在内存中创建一个图片
        BufferedImage image = new
        BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        //得到图片
        Graphics2D g = (Graphics2D) image.getGraphics(); //笔
        //设置图片的背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.BLUE);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);
        //告诉浏览器，这个请求用图片的方式打开
        resp.setContentType("image/jpeg");
        //网站存在缓存，不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        //把图片写给浏览器
        ImageIO.write(image,"jpg", resp.getOutputStream());
        }
        //生成随机数
        private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999) + "";
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < 7-num.length() ; i++) {
            sb.append("0");
            }
            num = sb.toString() + num;
            return num;
        }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
        }
}

```

### 5.4实现重定向

![image-20220304233455565](C:\Java_Study\General Node\img\response_redirect.png)

一个web资源收到客户端A请求后，B他会通知A客户端去访问另外一个web资源C，这个过程叫重定向 

常见场景：

- 用户登录

```java
void sendRedirect(String var1) throws IOException;

@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    /*
    resp.setHeader("Location","/r/img");
    resp.setStatus(302);
    */
    resp.sendRedirect("/r/img");//重定向
}

```

重定向和转发的区别？

相同点: 页面都会实现跳转 

不同点: 

**请求转发的时候，url不会产生变化**; 307

**重定向时候，url地址栏会发生变化；**302

![image-20220304234241175](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220304234241175.png)

## 6 HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封 装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息；



![image-20220306004356679](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220306004356679.png)

### 获取参数，请求转发

![image-20220306004442568](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220306004442568.png)

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp)throws ServletException, IOException {
    req.setCharacterEncoding("utf-8");
    resp.setCharacterEncoding("utf-8");
    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobbys = req.getParameterValues("hobbys");
    System.out.println("=============================");
    //后台接收中文乱码问题
    System.out.println(username);
    System.out.println(password);
    System.out.println(Arrays.toString(hobbys));
    System.out.println("=============================");
    System.out.println(req.getContextPath());
    //通过请求转发
    //这里的 / 代表当前的web应用
    req.getRequestDispatcher("/success.jsp").forward(req,resp);
}
```





## 7 Cookie, Session

### 7.1、会话 

**会话**：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之 为会话；

**有状态会话**：一个同学来过教室，下次再来教室，我们会知道这个同学，曾经来过，称之为有状态会话

**一个网站，怎么证明你来过？** 

客户端 						服务端 

1. 服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了； cookie 
2. 服务器登记你来过了，下次你来的时候我来匹配你； **session**



### 7.2 保存会话的两种方式

**cookie** 客户端技术 （响应，请求） 



**session 服务器技术**，利用这个技术，可以保存用户的会话信息, 我们可以把信息或者数据放在Session 中！



常见常见：网站登录之后，你下次不用再登录了，第二次访问直接就上去了！

### 7.3 Cookie

 ![image-20220307125530864](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220307125530864.png)

1. 从请求中拿到cookie信息 

2. 服务器响应给客户端cookie

```java
Cookie[] cookies = req.getCookies(); //获得Cookie
cookie.getName(); //获得cookie中的key
cookie.getValue(); //获得cookie中的value
new Cookie("lastLoginTime", System.currentTimeMillis()+""); //新建一个cookie
cookie.setMaxAge(24*60*60); //设置cookie的有效期
resp.addCookie(cookie); //响应给客户端一个cookie
```

cookie：一般会保存在本地的 用户目录下 appdata；



一个网站cookie**是否存在上限！**聊聊细节问题 

- 一个Cookie只能保存一个信息；
-  一个web站点可以给浏览器发送多个cookie，最多存放20个cookie；

- Cookie大小有限制4kb； 
- 300个cookie浏览器上限



**删除Cookie:**

- 不设置有效期，关闭浏览器，自动失效； 
- 设置有效期时间为 0 ；



**Encoder/Decoder:**

```java
URLEncoder.encode("秦疆","utf-8")
URLDecoder.decode(cookie.getValue(),"UTF-8")
```



### 7.4 Session(*)

什么是Session： 

- 服务器会给每一个用户（浏览器）创建一个Seesion对象； 
- 一个Seesion独占一个浏览器，只要浏览器没有关闭，这个Session就存在； 
- 用户登录之后，整个网站它都可以访问！--> 保存用户的信息；保存购物车的信息…..

![image-20220307132020696](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220307132020696.png)

**Session和cookie的区别：**

- Cookie是把用户的数据写给用户的浏览器，浏览器保存 （可以保存多个） 
- Session把用户的数据写到用户独占Session中，服务器端保存 （保存重要的信息，减少服务器资 源的浪费） 
- Session对象由服务创建； 



使用场景： 

保存一个登录用户的信息； 

购物车信息； 

在整个网站中经常会使用的数据，我们将它保存在Session中；

```JAVA
package com.robin.servlet;
import com.kuang.pojo.Person;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import java.io.IOException;

public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		//解决乱码问题
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=utf-8");
        //得到Session
        HttpSession session = req.getSession();
        //给Session中存东西
        session.setAttribute("name",new Person("秦疆",1));
        //获取Session的ID
        String sessionId = session.getId();
        //判断Session是不是新创建
        if (session.isNew()){
        	resp.getWriter().write("session创建成功,ID:"+sessionId);
        	}else {
        	resp.getWriter().write("session以及在服务器中存在了,ID:"+sessionId);
   			}
        //Session创建的时候做了什么事情；
        // Cookie cookie = new Cookie("JSESSIONID",sessionId);
        // resp.addCookie(cookie);
		}
    
    @Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp)throws ServletException, IOException {
		doGet(req, resp);
		}
	}
}
//得到Session
HttpSession session = req.getSession();
Person person = (Person) session.getAttribute("name");
System.out.println(person.toString());
HttpSession session = req.getSession();
session.removeAttribute("name");
//手动注销Session
session.invalidate();

```



**会话自动过期：web.xml配置**

```xml
<!--设置Session默认的失效时间-->
<session-config>
<!--15分钟后Session自动失效，以分钟为单位-->
<session-timeout>15</session-timeout>
</session-config>

```

![image-20220307134626690](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220307134626690.png)
