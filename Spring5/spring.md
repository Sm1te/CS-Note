## 1 Spring

### **1.1 Basic**

- 官网 : http://spring.io/ 
- 官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/ 
- GitHub : https://github.com/spring-projects



- SSM: SpringMvc + Spring + Mybatis



```java
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.16</version>
</dependency>
    
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.16</version>
</dependency>
```



### 1.2 优点

- Spring是一个开源免费的框架 , 容器 . 
- Spring是一个轻量级的框架 , 非侵入式的 . 
- **控制反转 IoC , 面向切面 Aop**
- 对事物的支持 , 对框架的支持 
- **Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

*Aspect-Oriented Programming* (*AOP*)， *Inversion of Control(IOC)*



### 1.3 Component

![img](https://upload-images.jianshu.io/upload_images/9654063-3914fb6623b2aac7.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/555/format/webp)

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器 定义了创建、配置和管理 bean 的方式 .

![2. Introduction to the Spring Framework](https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/images/spring-overview.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模 块的功能如下： 

**核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 **BeanFactory** ，它 是工厂模式的实现。 **BeanFactory** 使用控制反转（IOC） 模式将应用程序的配置和依赖性规范 与实际的应用程序代码分开。 

**Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文 包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。 

**Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以 将声明性事务管理集成到应用程序中。

**Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不 同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异 常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次 结构。 

**Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包 括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结 构。 

**Spring Web** 模块：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提 供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请 求以及将请求参数绑定到域对象的工作。 

**Spring MVC** 框架：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口， MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。



### 1.4 拓展

**Spring Boot与Spring Cloud**

- Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务;
- Spring Cloud是基于Spring Boot实现的； 
- Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架； 
- Spring Boot使用了约束优于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置 , 
- Spring Cloud很大的一部分是基于Spring Boot来实现，Spring Boot可以离开Spring Cloud独立使 用开发项目，但是Spring Cloud离不开Spring Boot，属于依赖的关系。 
- SpringBoot在SpringClound中起到了承上启下的作用，如果你要学习SpringCloud必须要学习 SpringBoot。

![image-20220318124842688](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220318124842688.png)



## 2 IOC基础

新建一个空白的maven项目

### 2.1 分析实现

1. 先写一个UserDao接口

```java
public interface UserDao {
	public void getUser();
}
```

2. 再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
	@Override
	public void getUser() {
		System.out.println("获取用户数据");
	}
}
```

3. 然后去写UserService的接口

```java
public interface UserService {
	public void getUser();
}
```

4. 最后写Service的实现类

```JAVA
public class UserServiceImpl implements UserService {
	private UserDao userDao = new UserDaoImpl();
	@Override
	public void getUser() {
	userDao.getUser();
	}
}
```

5. 测试一下

```JAVA
@Test
public void test(){
	UserService service = new UserServiceImpl();
	service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 . 把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
	@Override
	public void getUser() {
	System.out.println("MySql获取用户数据");
	}	
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现 .

```java
public class UserServiceImpl implements UserService {
	private UserDao userDao = new UserDaoMySqlImpl();
    
	@Override
	public void getUser() {
		userDao.getUser();
	}
}
```

在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
	@Override
	public void getUser() {
		System.out.println("Oracle获取用户数据");
	}
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身 .

<span style="color:red">用户变了->改源代码 NO</span>.

**如何解决：**

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserService {
	private UserDao userDao;
	// 利用set实现
	public void setUserDao(UserDao userDao) {
		this.userDao = userDao;
	}
    
	@Override
	public void getUser() {
		userDao.getUser();
	}
}
```

现在去我们的测试类里 , 进行测试

```java
@Test
public void test(){
	UserServiceImpl service = new UserServiceImpl();
	service.setUserDao( new UserDaoMySqlImpl() );
	service.getUser();
	//那我们现在又想用Oracle去实现呢
	service.setUserDao( new UserDaoOracleImpl() );
	service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别 . 但是同学们 , 他们已经发生了根本性的变化 , 很多地方都不一样了 . 仔细去思考一下 , 以前所有东西都是由程序去进行控制创建 , 而现在是由我们自行控制创建对 象 , 把主动权交给了调用者 . 程序不用去管怎么创建,怎么实现了 . 它只负责提供一个接口 . 这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是IOC的原型 !



### IOC

**![image-20220319214525934](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220319214525934.png)**

**控制反转IoC(Inversion of Control)，是一种设计思想**，DI(依赖注入)是实现IoC的一种方法，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系 完全硬编码在程序中，对象的创建由程序自己控制，**控制反转后将对象的创建转移给第三方，个人认为 所谓控制反转就是：获得依赖对象的方式反转了**。

![image-20220319215017113](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220319215017113.png)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解， 新版本的Spring也可以零配置实现IoC。 

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![image-20220319220415214](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220319220415214.png)

![image-20220319220525773](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220319220525773.png)采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为 一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。 **控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI)。**



### 3 HelloSpring

### 3.1 jar

已经导入成功

### 3.2 代码部分

1. Hello class

```java
public class Hello {
    private String str;

    public String getStr(){
        return str;
    }

    public void setStr(String str){
        this.str = str;
    }

    @Override
    public String toString() {
        return "Hello " + str;
    }
}
```

2. spring 文件，beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
  https://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!--使用spring来创建对象，在spring中这些都称为Bean
    Hello hello = new Hello()

    id = 变量名   class = new 对象
    property 相当于给对象中的属性设置一个值
    bean = 对象   new Hello()
    -->
    <bean id="hello" class="com.robin.pojo.Hello">
            <property name ="str" value="Spring"/>
    </bean>
</beans>
```

3. Test

```java
public class MyTest {
    public static void main(String[] args) {
        //获取spring的上下文对象！
        //getBean : 参数即为spring配置文件中bean的id.
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //对象目前都在spring中管理，如果要使用，直接去取出来就行
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello.toString());;
    }
}
```

### 3.3实现原理

- Hello 对象是谁创建的 ? 【 hello 对象是由Spring创建的 】
- Hello 对象的属性是怎么设置的 ? 【hello 对象的属性是由Spring容器设置的 】

这个过程就叫**控制反转** : 

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是 由Spring来创建的 

- 反转 : 程序本身不创建对象 , 而变成被动的接收对象 . 
- 依赖注入 : 就是利用set方法来进行注入的. 

<span style="color:red">IOC是一种编程思想，由主动的编程变成被动的接收 </span>.

可以通过newClassPathXmlApplicationContext去浏览一下底层源码

**本质是通过set来实现控制反转（ioc）**

### 3.4 Refine Case 1

新增一个beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="MysqlImpl" class="com.robin.Li.UserLiMysqlImpl"/>
    <bean id="LiImpl" class="com.robin.Li.UserLiImpl"/>

    <bean id="UserServiceImpl" class="com.robin.Service.UserServiceImpl">
        <!--
        这里起到了set的作用
        ref: 引用spring容器中创建好的对象
        value： 具体的值： 基本数据类型！
        -->
        <property name="userLi" ref = "MysqlImpl"/>
    </bean>
</beans>
```

test

```java
public static void main(String[] args) {
        //获取ApplicationContext, 拿到容器
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        UserServiceImpl userServiceImpl = (UserServiceImpl) context.getBean("UserServiceImpl");
        userServiceImpl.getUser();
    }
```

现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 , 所谓的IoC,一句话搞定 : **对象由Spring 来创建 , 管理 , 装配** !



## 4 IOC 创建对象



1. 默认使用无参构造对象

```xml
<bean id="user" class="com.robin.pojo.User">
        <property name="name" value="Yijian Li"></property>
</bean>
```

2. Constructor argument index下标赋值

```xml
<bean id="user" class="com.robin.pojo.User">
        <constructor-arg index="0" value="Robin"/>
</bean>
```

3. Constructor argument type matching(Not recommend)

```xml
<bean id="user" class="com.robin.pojo.User">
        <constructor-arg type = "java.lang.String" value="Yijian"/>
</bean>
```

4. Constructor Argument Resolution直接通过参数赋值

```xml
<bean id="user" class="com.robin.pojo.User">
        <constructor-arg name = "name" value="Robin"/>
</bean>
```

Test

```java
@Test
public void test(){
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //在执行getBean的时候, user已经创建好了 , 通过无参构造
    User user = (User) context.getBean("user");
    //调用对象的方法 .
    user.show();
}

```

**在配置文件加载的时候, 其中所有 管理的对象都已经初始化了！**

![image-20220320151456721](C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220320151456721.png)

True



## 5 配置Spring

#### Alias

name 和 alias的参数都可以用，多个名字

```xml
<alias name = "user" alias = "user1"/>
```

#### Bean

id 是bean的**标识符,要唯一,**如果没有配置id,name就是默认标识符
如果配置id,又配置了name,那么name是别名
**name可以设置多个别名**,可以用逗号,分号,空格隔开
如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;
**class是bean的全限定名=包名+类名, bean对象所对应的类名**

```xml
<!--bean就是java对象,由Spring创建和管理-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.kuang.pojo.Hello">
	<property name="name" value="Spring"/>
</bean>
```

#### Import

```xml
<import resource="{path}/beans.xml"/>
```

将多个配置文件导入合并为一个, 比如不同的类需要注册在不同的bean中，我们可以利用import将所有人的beans.xml合并为一个总的



## 6 依赖注入(DI)

依赖注入(Dependency Injection,DI)。 

依赖 : 指Bean对象的创建依赖于容器 . Bean对象的依赖资源. 

注入 : 指Bean对象所依赖的资源（属性） , 由容器来设置和装配.

### 6.1 构造器输入

### 6.2 ※set注入

要求被注入的属性 , 必须有**set**方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is.

```xml
<bean id = "address" class="com.robin.pojo.Address"/>

<bean id="student" class="com.robin.pojo.Student">
    <!--常量值注入，value-->
    <property name = "name" value="Robin"/>

    <!--Bean 注入， ref-->
    <property name="address" ref="address"/>

    <!--数组注入-->
    <property name="books">
        <array>
            <value>三国演义</value>
            <value>红楼梦</value>
            <value>西游记</value>
            <value>水浒传</value>
        </array>
    </property>

    <!--List-->
    <property name="hobbies">
        <list>
            <value>Basketball</value>
            <value>Music</value>
            <value>Tennis</value>
        </list>
    </property>

    <!--Map-->
    <property name="card">
        <map>
            <entry key = "李伊健" value="15073039072"/>
            <entry key = "RobinLi"  value="2244206315"></entry>
        </map>
    </property>

    <!--Set-->
    <property name="games">
        <set>
            <value>LoL</value>
            <value>CSGO</value>
        </set>
    </property>

    <!--NULL-->
    <property name="wife">
        <null></null>
    </property>

    <!--Properties-->
    <property name="info">
        <props>
            <prop key="index">55</prop>
            <prop key="sex">男</prop>
        </props>
    </property>
</bean>
```

==

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220321082513252.png" alt="image-20220321082513252" style="zoom:50%;" />





### 6.3 扩展方式注入

1. P命名空间注入

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--P(属性: properties)命名空间, 可以直接注入属性的值：property-->
    <bean name="user" class="com.robin.pojo.User" p:name="Robin" />
    
</beans>
```

2. C命名空间注入

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--C 命名空间注入，可以通过构造器注入：construct-org-->
    <bean name="user2" class="com.robin.pojo.User" c:name="Robin" />
</beans>
```

两个都需要有set方法



### 6.4 Bean 的作用域(Scope)

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲， bean就是由IoC容器初始化、装配及管理的对象.

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220321092405245.png" alt="image-20220321092405245" style="zoom:80%;" />

#### 6.4.1 The Singleton Scope

<img src="https://docs.spring.io/spring-framework/docs/5.3.16/reference/html/images/singleton.png" alt="singleton" style="zoom:74%;" />

当一个bean的作用域为Singleton，那么**Spring IoC容器中只会存在一个共享的bean实例**，并且所有对 bean的请求，只要id与该bean定义相匹配，**则只会返回bean的同一实例**。Singleton是单例类型，**就是在创建起容器时就同时自动创建了一个bean的对象**，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。

```xml
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

#### 6.4.2 The Prototype Scope

You should use the prototype scope for all stateful beans and the singleton scope for stateless beans.

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会**创建一个新的bean实例**。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们**获取bean的时候才会去创建一个对象**，**而且我们每次获取到的对象都不是同一个对象**。根据经验，**对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。**

<img src="https://docs.spring.io/spring-framework/docs/5.3.16/reference/html/images/prototype.png" alt="prototype" style="zoom:67%;" />

```xml
<bean id="account" class="com.foo.DefaultAccount" scope="prototype"/>
或者
<bean id="account" class="com.foo.DefaultAccount" singleton="false"/>
```

#### 6.4.3 **Request**

当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
<bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
```

针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

### 6.4.4 Session

当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的 userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据 userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session 最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。



### 7 Bean的自动装配

Spring会在上下文中自动寻找并自动给bean装配.



三种装配机制：

1. 在xml中显式配置
2. 在java中显式配置
3. 隐式的bean发现机制和自动装配



Spring的自动装配需要从两个角度来实现，或者说是两个操作： 

1. 组件扫描(component scanning)：spring会自动发现应用上下文中所创建的bean 
2. 自动装配(autowiring)：spring自动满足bean之间的依赖，也就是我们说的IoC/DI

组件扫描和自动装配组合发挥巨大威力，使的显示的配置降低到最少。 

推荐不使用自动装配xml配置 , 而使用注解

正常：

```xml
<bean id = "cat" class="com.robin.pojo.Cat"/>
<bean id = "dog" class="com.robin.pojo.Dog"/>

<bean id = "human" class="com.robin.pojo.Human">
    <property name="name" value = "Robin"/>
    <property name="cat" ref= "cat"/>
    <property name="dog" ref = "dog"/>
</bean>
```



#### 7.1 byName

由于在手动配置xml过程中，常常发生字母缺漏和大小写等错误，而无法对其进行检查，使得开发效率降低。

```xml
<bean id = "cat" class="com.robin.pojo.Cat"/>
<bean id = "dog" class="com.robin.pojo.Dog"/>
<!--byName会自动在容器上下文中查找，和自己对象set方法后面的值对应的bean id-->
<bean id = "human" class="com.robin.pojo.Human" autowire="byName">
    <property name="name" value = "Robin"/>
</bean>
```

当一个bean节点带有 autowire byName的属性时:

1. 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。 
2. 去spring容器中寻找是否有此字符串名称id的对象。
3. 如果有，就取出注入；如果没有，就报空指针异常。

#### 7.2 byType

```xml
    <bean id = "cat" class="com.robin.pojo.Cat"/>
    <bean id = "dog" class="com.robin.pojo.Dog"/>
    <!--byName会自动在容器上下文中查找，和自己对象属性类型相同的bean
        这个类型的全局数量必须为1
		可以省略id
    -->
    <bean id = "human" class="com.robin.pojo.Human" autowire="byType">
        <property name="name" value = "Robin"/>
    </bean>
```

使用autowire byType首先需要保证：同一类型的对象，**在spring容器中唯一**。如果不唯一，会报不唯一的异常。



#### 7.3 使用注解实现自动装配

在spring配置文件中引入context文件头

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
	<!--开启属性注解支持！-->
    <context:annotation-config/>

</beans>

```

##### 7.3.1 @Autowired

- @Autowired是按类型自动转配的，不支持id匹配。 
- 需要导入 spring-aop的包
- 可以放在属性或者set部分使用
- 可以忽略set方法使用, 自动转配的属性在IOC（Spring）容器中存在，且符合名字byName

```java
public class Human {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
    
    .......
}
```

@Autowired(required=false) 说明： false，对象可以为null；true，对象必须存对象，不能为null

```java
//如果允许对象为null，设置required = false,默认为true
@Autowired(required = false)
private Cat cat;
```



##### 7.3.2 @Qualifier

@Autowired是根据类型自动装配的，加上@Qualifier则可以根据byName的方式自动装配 @Qualifier不能单独使用。

如果@Autowired自动装配的环节比较复杂， 自动装配无法通过一个注解完成的是时候， 我们可以可以使用@Qualifier(value  = "") 来配合autowired使用，指定一个唯一的bean对象注入

```xml
<bean id="dog1" class="com.kuang.pojo.Dog"/>
<bean id="dog2" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>
```

```java
@Autowired
@Qualifier(value = "cat2")
private Cat cat;
@Autowired
@Qualifier(value = "dog2")
private Dog dog;
```



##### 7.3.3

- @Resource如有指定的name属性，**先按该属性进行byName方式查找装配**； 
- **其次再进行默认的byName方式进行装配**；
- 如果以上都不成功，**则按byType的方式自动装配**。 
- 都不成功，则报异常。

```java
//Case 1
public class User {
    //如果允许对象为null，设置required = false,默认为true
    @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
    private String str;
}
//Case 2
@Resource
private Cat cat;
@Resource
private Dog dog;
```

```xml
<!--Case 1-->
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
<bean id="cat2" class="com.kuang.pojo.Cat"/>
<bean id="user" class="com.kuang.pojo.User"/>
<!--Case 2-->
<bean id="dog" class="com.kuang.pojo.Dog"/>
<bean id="cat1" class="com.kuang.pojo.Cat"/>
```

先进行byName查找，失败；再进行byType查找。



##### 7.3.4 Summary

**@Autowired与@Resource异同**： 

1. @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。 
2. @Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用 
3. @Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在 setter方法上默认取属性名进行装配。 当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。 

它们的作用相同都是用注解方式注入对象，但执行顺序不同。**@Autowired先byType，@Resource先 byName。**



### 8 使用注解开发

#### 8.1 Bean

要使用注解，必须要导入aop的包

<img src="C:\Users\liyij\AppData\Roaming\Typora\typora-user-images\image-20220322215947663.png" alt="image-20220322215947663" style="zoom:80%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

#### 8.2 Bean的实现

1. 扫描哪些包下的注解

```xml
<context:component-scan base-package="com.robin"/>
```

2. 在指定包下编写类，增加注解

就是说这个类以及被Spring接管了，注册到了容器中

```java
// 等价于<bean id = "user" class = "com.robin.pojo.User">
@Component
public class User {
public String name = "robin";
}
```

#### 8.3 属性注入

使用注解注入属性

1. 无set方法

```java
@Component("user")
// 相当于配置文件中 <bean id="user" class="当前注解的类"/>
public class User {
    @Value("Yijian Li")
    // 相当于配置文件中 <property name="name" value="Yijian Li"/>
    public String name;
}
```

2. 有set方法

```java
@Component("user")
public class User {
    public String name;
        @Value("Yijian Li")
        public void setName(String name) {
        this.name = name;
    }
}
```

#### 8.4 衍生注解

我们这些注解，就是替代了在配置文件当中配置步骤，更加的方便快捷！ 

**@Component三个衍生注解** 

为了更好的进行分层，Spring可以使用其它三个注解，功能一样，目前使用哪一个功能都一样。 

- **@Controller：web层** 
- **@Service：service层** 
- **@Repository：dao层** 

写上这些注解，就相当于将这个类交给Spring管理装配了！



#### 8.5 自动装配注解

- @Autowired：自动装配通过类型。名字

​	如果Autowired 不能唯一自动装配上属性，则需要通过@Qualifier(value = "xxx")

- @Nullable：字段标记了这个注解，说明这个字段可以为Null
- @Resource：自动装配通过名字，类型
- @Component: 组件，放在类上，说明这个类被Spring管理了



#### 8.6 作用域

@scope 

- singleton：默认的，Spring会采用单例模式创建这个对象。关闭工厂 ，所有的对象都会销毁。 
- prototype：多例模式。关闭工厂 ，所有的对象不会销毁。内部的垃圾回收机制会回收

```java
@Controller("user")
@Scope("prototype")
public class User {
    @Value("秦疆")
    public String name;
}

```



#### 8.7 XML vs. Annotation

**XML与注解比较**

- XML可以适用任何场景 ，结构清晰，维护方便 
- 注解不是自己提供的类使用不了，开发简单方便 

**xml与注解整合开发** ：推荐最佳实践 

- xml管理Bean 
- 注解完成属性注入 
- 使用过程中， 可以不用扫描，扫描是为了类上的注解 

```xml
<context:annotation-config/>
```

作用： 

- 进行注解驱动注册，从而使注解生效 
- 用于激活那些已经在spring容器里注册过的bean上面的注解，也就是显示的向Spring注册 
- 如果不扫描包，就需要手动配置bean 
- 如果不加注解驱动，则注入的值为null！

### 9 Java-based Container Configuration

JavaConfig 原来是 Spring 的一个子项目，它通过 Java 类的方式提供 Bean 的定义信息.

完全不使用xml配置

1. 编写实例

```java
@Component //将这个类标注为Spring的一个组件，放到容器中！
public class Dog {
	public String name = "dog";
}
```

2. 新建config配置包

```java
@Configuration //他本来也是Component类， 代表这个是一个配置类 == beans.xml
public class MyConfig {
    @Bean 
    //注册一个bean, 等于之前写的bean标签
    //这个方法的名字等价于 bean 以前的id
    //返回值就是 bean中的class属性
    public Dog dog(){
    	return new Dog();//返回要注入的对象
    }
}

```

3. Test

```java
@Test
public void test2(){
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(MyConfig.class);
    Dog dog = (Dog) applicationContext.getBean("dog");
    System.out.println(dog.name);
}
```

#### 导入其他配置

```java
@Configuration //代表这是一个配置类
public class MyConfig2 {
}

@Configuration
@Import(MyConfig2.class) //导入合并其他配置类，类似于配置文件中的 inculde 标签
public class MyConfig {
    @Bean
    public Dog dog(){
    	return new Dog();
    }
}

```



### 10 代理模式

#### 10.1 静态代理

