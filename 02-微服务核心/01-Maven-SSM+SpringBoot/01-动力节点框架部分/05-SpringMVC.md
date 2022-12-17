# SpringMVC

### 一、SpringMVC概述

##### 1 SpringMVC 简介

SpringMVC 也叫 Spring web mvc。是 Spring 框架的一部分，是在 Spring3.0 后发布的。

```
SpringMVC：
	是基于spring的一个框架， 实际上就是spring的一个模块， 专门是做web开发的。
	理解是servlet的一个升级

	web开发底层是servlet，框架是在servlet基础上面加入一些功能，让你做web开发方便。

SpringMVC就是一个Spring。
Spring是容器，ioc能够管理对象，使用<bean>, @Component, @Repository, @Service, @Controller
SpringMVC能够创建对象，放入到容器中（SpringMVC容器），springmvc容器中放的是控制器对象，

我们要做的是 使用@Contorller创建控制器对象，把对象放入到springmvc容器中，把创建的对象作为控制器使用
这个控制器对象能接收用户的请求，显示处理结果，就当做是一个servlet使用。

使用@Controller注解创建的是一个普通类的对象，不是Servlet。springmvc赋予了控制器对象一些额外的功能。

web开发底层是servlet，springmvc中有一个对象是Servlet：DispatherServlet(中央调度器)
DispatherServlet: 负责接收用户的所有请求，用户把请求给了DispatherServlet，之后DispatherServlet把请求转发给
                  我们的Controller对象， 最后是Controller对象处理请求。

index.jsp-----DispatherServlet(Servlet)----转发，分配给---Controller对象（@Controller注解创建的对象）
main.jsp                                                   MainController
addUser.jsp                                                UserController
```



##### 2 SpringMVC 优点

1. 基于 MVC 架构

   基于 MVC 架构，功能分工明确；解耦合；

2. 容易理解，上手快，使用简单

   就可以开发一个注解的 SpringMVC 项目，SpringMVC 也是轻量级的，jar 很小。不依赖特定的接口和类。

3. 作为 Spring 框架一部分，能够使用 Spring 的 IOC 和 AOP 。方便整合Strtus，MyBatis，Hiberate，JPA 等其他框架。

4. SpringMVC 强化注解的使用，在控制器，Service，Dao 都可以使用注解。方便灵活。

   使用@Controller 创建处理器对象，@Service 创建业务对象，@Autowired 或者@Resource 在控制器类中注入 Service，Service 类中注入 Dao。



##### 3 第一个注解的 SpringMVC 程序

```
ch01-hello-springmvc: 第一个springmvc项目。
需求：用户在页面发起一个请求，请求交给springmvc的控制器对象，
     并显示请求的处理结果（在结果页面显示一个欢迎语句）。
实现步骤：
1. 新建web maven工程
2. 加入依赖
   spring-webmvc依赖，间接把spring的依赖都加入到项目
   jsp，servlet依赖
3.重点：在web.xml中注册springmvc框架的核心对象DispatcherServlet
     1)DispatcherServlet叫做中央调度器，是一个servlet，它的父类是继承HttpServlet
     2)DispatcherServlet也叫做前端控制器（front controller）
     3)DispatcherServlet负责接收用户提交的请求，调用其它的控制器对象，并把请求的处理结果显示给用户
4.创建一个发起请求的页面 index.jsp
5.创建控制器(处理器)类
  1）在类的上面加入@Controller注解，创建对象，并放入到springmvc容器中
  2）在类中的方法上面加入@RequestMapping注解。
6.创建一个作为结果的jsp，显示请求的处理结果。
7.创建springmvc的配置文件（spring的配置文件一样）
  1）声明组件扫描器，指定@Contorller注解所在的包名
  2）声明视图解析器。帮助处理视图的。
```

###### 3.1 新建 maven web 项目

![image-20220302214201016](05-SpringMVC.assets\image-20220302214201016.png)



###### 3.2 pom.xml

在创建好 web 项目后，加入 Servlet 依赖，SpringMVC 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>ch01-hello-springmvc</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!--springmvc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <!-- 编码和编译和JDK版本 -->
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```



###### 3.3 注册中央调度器

```xml
<!--web.xml-->
<!--
    注册(声明)SpringMVC的核心对象 DispatcherServlet
    需要在tomcat服务器启动后，创建 DispatcherServlet 对象的实例。
    为什么要创建 DispatcherServlet 对象的实例呢？
    因为 DispatcherServlet 在他的创建过程中，会同时创建 springmvc 容器对象，
    读取 springmvc 的配置文件，把这个配置文件中的对象都创建好，当用户发起
    请求时就可以直接使用对象了。

    servlet的初始化会执行 init() 方法。 DispatcherServlet在 init() 中
	{
       //创建容器，读取配置文件
       WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
       //把容器对象放入到 ServletContext 中
       getServletContext().setAttribute(key, ctx);
    }

    启动tomcat报错，读取这个文件 /WEB-INF/springmvc-servlet.xml（/WEB-INF/springmvc-servlet.xml）
    springmvc创建容器对象时，读取的配置文件默认是/WEB-INF/<servlet-name>-servlet.xml .
-->
<servlet>
    <servlet-name>springmvc</servlet-name>
    <!--全限定类名-->
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    
    <!--自定义springmvc读取的配置文件的位置-->
    <init-param>
        <!--springmvc的配置文件的位置的属性-->
        <param-name>contextConfigLocation</param-name>
        <!--指定自定义文件的位置-->
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>

    <!--在tomcat启动后，创建Servlet对象
        load-on-startup:表示tomcat启动后创建对象的顺序。它的值是大于等于0的整数，数值越小，
                        tomcat创建对象的时间越早。
    -->
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!--
        使用框架的时候，url-pattern可以使用两种值
        1. 使用扩展名方式，语法 *.xxxx，xxxx是自定义的扩展名。常用的方式 *.do, *.action, *.mvc等等
           不能使用 *.jsp
           http://localhost:8080/myweb/some.do
           http://localhost:8080/myweb/other.do
        2.使用斜杠 "/"
    -->
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

1. 全限定类名

   该中央调度器为一个 Servlet，名称为 DispatcherServlet。中央调度器的全限定性类名在导入的 Jar 文件 spring-webmvc-5.2.5.RELEASE.jar 的第一个包 org.springframework.web.servlet 下可找到。

2. load-on-startup

   ```
   在<servlet/>中添加<load-on-startup/>的作用是，标记是否在Web服务器（这里是Tomcat）
   启动时会创建这个 Servlet 实例，即是否在 Web 服务器启动时调用执行该Servlet的init()方
   法，而不是在真正访问时才创建。
   它的值必须是一个整数。
   ➢ 当值大于等于 0 时，表示容器在启动时就加载并初始化这个 servlet，数值越小，该 Servlet
   的优先级就越高，其被创建的也就越早；
   ➢ 当值小于 0 或者没有指定时，则表示该 Servlet 在真正被使用时才会去创建。
   ➢ 当值相同时，容器会自己选择创建顺序。
   ```

3. url-pattern

   ```
   对于<url-pattern/>，可以写为 / ，建议写为*.do 的形式。
   ```

4. 配置文件位置与名称

   注册完毕后，可直接在服务器上发布运行。此时，访问浏览器页面，控制台均会抛出 FileNotFoundException 异常。即默认要从项目根下的 WEB-INF 目录下找名称为 【Servlet名称-servlet.xml】的配置文件。这里的“Servlet 名称”指的是注册中央调度器标签中指定的 Servlet 的 name 值。本例配置文件名为 springmvc-servlet.xml。

   ![image-20220302221800071](05-SpringMVC.assets\image-20220302221800071.png)

   而一般情况下，配置文件是放在类路径下，即 resources 目录下。所以，在注册中央调度器时，还需要为中央调度器设置查找 SpringMVC 配置文件路径，及文件名。

   ![image-20220302221913339](05-SpringMVC.assets\image-20220302221913339.png)

   打开 DispatcherServlet 的源码，其继承自 FrameworkServlet，而FrameworkServlet类中有一个属性 contextConfigLocation，用于设置 SpringMVC 配置文件的路径及文件名。该初始化参数的属性就来自于这里。

   ![image-20220302222232122](05-SpringMVC.assets\image-20220302222232122.png)

   ![image-20220302222236350](05-SpringMVC.assets\image-20220302222236350.png)



###### 3.4 创建 SpringMVC 配置文件

在工程的类路径即 src 目录下创建 SpringMVC 的配置文件 springmvc.xml。该文件名可以任意命名。

![image-20220302222659407](05-SpringMVC.assets\image-20220302222659407.png)



###### 3.5 创建处理(控制)器

在类上与方法上添加相应注解即可。 

@Controller：表示当前类为处理器 

@RequestMapping：表示当前方法为处理器方法。该方法要对 value 属性所指定的 URI 进行处理与响应。被注解的方法的方法名可以随意

若有多个请求路径均可匹配该处理器方法的执行，则@RequestMapping 的 value 属性中可以写上一个数组。

ModelAndView 类中的 addObject()方法用于向其 Model 中添加数据。Model 的底层为一个 HashMap。

Model 中的数据存储在 request 作用域中，SringMVC 默认采用请求转发的方式跳转到视图，本次请求结束，模型中的数据被销毁。

```java
package com.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
/*
 *  @Controller:创建处理器对象，对象放在springmvc容器中。
 *  位置：在类的上面
 *  和Spring中讲的 @Service, @Component
 *
 *  能处理请求的都是控制器（处理器）： MyController能处理请求，
 *                       叫做后端控制器（back controller）
 *  没有注解之前，需要实现各种不同的接口才能做控制器使用
 */
@Controller
public class MyController {
    /*
       处理用户提交的请求，springmvc中是使用方法来处理的。
       方法是自定义的， 可以有多种返回值， 多种参数，方法名称自定义
     */

    /*
     * 准备使用doSome方法处理some.do请求。
     * @RequestMapping: 请求映射，作用是把一个请求地址和一个方法绑定在一起。
     *                  一个请求指定一个方法处理。
     *       属性：1.value 是一个String，表示请求的uri地址的（some.do）。
     *              value的值必须是唯一的，不能重复。 在使用时，推荐地址以“/”为开头
     *       位置：1.在方法的上面，常用的。
     *            2.在类的上面
     *  说明：使用RequestMapping修饰的方法叫做处理器方法或者控制器方法。
     *  使用@RequestMapping修饰的方法可以处理请求的，类似Servlet中的doGet, doPost
     *
     *  返回值：ModelAndView 表示本次请求的处理结果
     *   Model: 数据，请求处理完成后，要显示给用户的数据
     *   View: 视图，比如jsp等等。
     */
    @RequestMapping(value = {"/some.do", "/first.do"})
    public ModelAndView doSome(){  // doGet()--service请求处理
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv  = new ModelAndView();
        //添加数据，框架在请求的最后把数据放入到request作用域。
        //request.setAttribute("msg","欢迎使用springmvc做web开发");
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");

        //指定视图, 指定视图的完整路径
        //框架对视图执行的forward操作， request.getRequestDispather("/show.jsp).forward(...)
        mv.setViewName("/show.jsp");// src/main/webapp/show.jsp
        //mv.setViewName("/WEB-INF/view/show.jsp");

        //返回mv
        return mv;
    }
}
```



###### 3.6 声明组件扫描器

在 springmvc.xml 中注册组件扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context 
       https://www.springframework.org/schema/context/spring-context.xsd">
    <!--声明组件扫描器-->
    <context:component-scan base-package="com.springmvc.controller" />
</beans>
```



###### 3.7 页面

index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <P>第一个SpringMVC项目</P>
    <p><a href="some.do">发起some.do的请求</a></p>
</body>
</html>
```

show.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <h3>show.jsp 从request作用域获取数据</h3><br>
  <h4>msg数据:${msg}</h4><br>
  <h4>fun数据:${fun}</h4>
</body>
</html>
```

```
springmvc请求的处理流程
 1）发起some.do
 2）tomcat(web.xml--url-pattern知道 *.do的请求给DispatcherServlet)
 3）DispatcherServlet（根据springmvc.xml配置知道 some.do---doSome()）
 4）DispatcherServlet把some.do转发给MyController.doSome()方法
 5）框架执行 doSome() 把得到的ModelAndView进行处理，转发到show.jsp

上面的过程简化的方式
  some.do---DispatcherServlet---MyController
```

![image-20220303192620870](05-SpringMVC.assets\image-20220303192620870.png)

![image-20220303192625147](05-SpringMVC.assets\image-20220303192625147.png)

springMVC请求处理过程

![springmvc](05-SpringMVC.assets\springmvc.png)



###### 3.8 修改视图解析器的注册

SpringMVC 框架为了避免对于请求资源路径与扩展名上的冗余，在视图解析器 InternalResouceViewResolver 中引入了请求的前辍与后辍。而 ModelAndView 中只需给出要跳转页面的文件名即可，对于具体的文件路径与文件扩展名，视图解析器会自动完成拼接。

把 show.jsp 文件放到 /WEB-INF/jsp/路径中

```xml
	<!--springmvc.xml-->
	<!--注册视图解析器：帮助我们处理视图的路径和扩展名，生成视图对象-->
	<!--声明 springmvc框架中的视图解析器，帮助开发人员设置视图文件的路径-->
    <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀：视图文件的路径-->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!--后缀：视图文件的扩展名-->
        <property name="suffix" value=".jsp" />
    </bean>
```



###### 3.9 修改处理器

使用逻辑视图名称，show 是逻辑视图名称。

```java
	@RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv  = new ModelAndView();
        
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");
        
        //当配置了视图解析器后，可以使用逻辑名称（文件名），指定视图
        //框架会使用视图解析器的前缀 + 逻辑名称 + 后缀 组成完成路径，这里就是字符连接操作
        // /WEB-INF/view/ + show + .jsp
        mv.setViewName("show");

        return mv;
    }
```



###### 3.10 使用 SpringMVC 框架 web 请求处理顺序

![image-20220303200208062](05-SpringMVC.assets\image-20220303200208062.png)



##### 4 SpringMVC 的 MVC 组件

![image-20220303200244538](05-SpringMVC.assets\image-20220303200244538.png)



##### 5 SpringMVC 执行流程

###### 5.1 流程图

![image-20220303201039414](05-SpringMVC.assets\image-20220303201039414.png)

###### 5.2 执行流程简单分析

1. 浏览器提交请求到中央调度器。 
2. 中央调度器直接将请求转给处理器映射器。 
3. 处理器映射器会根据请求，找到处理该请求的处理器，并将其封装为处理器执行链后返回给中央调度器。 
4. 中央调度器根据处理器执行链中的处理器，找到能够执行该处理器的处理器适配器。 
5. 处理器适配器调用执行处理器。 
6. 处理器将处理结果及要跳转的视图封装到一个对象 ModelAndView 中，并将其返回给处理器适配器。 
7. 处理器适配器直接将结果返回给中央调度器。 
8. 中央调度器调用视图解析器，将 ModelAndView 中的视图名称封装为视图对象。 
9. 视图解析器将封装了的视图对象返回给中央调度器。 
10. 中央调度器调用视图对象，让其自己进行渲染，即进行数据填充，形成响应对象。 
11. 中央调度器响应浏览器。



### 二、SpringMVC 注解式开发

##### 1 @RequestMapping 定义请求规则

###### 1.1 指定模块名称

通过@RequestMapping 注解可以定义处理器对于请求的映射规则。该注解可以注解在方法上，也可以注解在类上，但意义是不同的。value 属性值常以“/”开始。

@RequestMapping 的 value 属性用于定义所匹配请求的 URI。但对于注解在方法上与类上，其 value 属性所指定的 URI，意义是不同的。

一个@Controller 所注解的类中，可以定义多个处理器方法。当然，不同的处理器方法所匹配的 URI 是不同的。这些不同的 URI 被指定在注解于方法之上的@RequestMapping 的 value 属性中。但若这些请求具有相同的 URI 部分，则这些相同的 URI，可以被抽取到注解在类之上的@RequestMapping 的 value 属性中。此时的这个 URI 表示模块的名称。URI 的请求是相对于 Web 的根目录。

换个角度说，要访问处理器的指定方法，必须要在方法指定 URI 之前加上处理器类前定义的模块名称

```java
/**
 * @RequestMapping:
 *    value：所有请求地址的公共部分,叫做模块名称
 *    位置：放在类的上面
 */
@RequestMapping("/test")
@Controller
public class MyController {
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");
        mv.setViewName("show");
        return mv;
    }

    @RequestMapping(value = {"/other.do","/second.do"})
    public ModelAndView doOther(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","====欢迎使用springmvc做web开发====");
        mv.addObject("fun","执行的是doOther方法");
        mv.setViewName("other");
        return mv;
    }
}
```



###### 1.2 对请求提交方式的定义

对于@RequestMapping，其有一个属性 method，用于对被注解方法所处理请求的提交方式进行限制，即只有满足该 method 属性指定的提交方式的请求，才会执行该被注解方法。

Method 属性的取值为 RequestMethod 枚举常量。常用的为 RequestMethod.GET 与 RequestMethod.POST，分别表示提交方式的匹配规则为 GET 与 POST 提交。

客户端浏览器常用的请求方式，及其提交方式有以下几种：

| 序号 | 请求方式        | 提交方式              |
| ---- | --------------- | --------------------- |
| 1    | 表单请求        | 默认GET，可以指定POST |
| 2    | AJAX请求        | 默认GET，可以指定POST |
| 3    | 地址栏请求      | GET请求               |
| 4    | 超链接请求      | GET请求               |
| 5    | src资源路径请求 | GET请求               |

也就是说，只要指定了处理器方法匹配的请求提交方式为 POST，则相当于指定了请求发送的方式：要么使用表单请求，要么使用 AJAX 请求。其它请求方式被禁用。

```java
修改处理器类 MyController
package com.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;
/*
 * @RequestMapping:
 *    value：所有请求地址的公共部分,叫做模块名称
 *    位置： 放在类的上面
 */
@RequestMapping("/test")
@Controller
public class MyController {
    /*
     * @RequestMapping : 请求映射
     *         属性：method， 表示请求的方式。它的值RequestMethod类枚举值。
     *              get请求方式，RequestMethod.GET
     *              post请求方式，RequestMethod.POST
     *
     *  你不用get方式，错误是：
     *  HTTP Status 405 - Request method 'GET' not supported
     */
    //指定some.do使用get请求方式
    @RequestMapping(value = "/some.do", method = RequestMethod.GET)
    public ModelAndView doSome(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");
        mv.setViewName("show");
        return mv;
    }

    //指定other.do使用post请求方式
    @RequestMapping(value = "/other.do", method = RequestMethod.POST)
    public ModelAndView doOther(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","====欢迎使用springmvc做web开发====");
        mv.addObject("fun","执行的是doOther方法");
        mv.setViewName("other");
        return mv;
    }

    //不指定请求方式，没有限制
    @RequestMapping(value = {"/first.do"})
    public ModelAndView doFirst(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","====欢迎使用springmvc做web开发====");
        mv.addObject("fun","执行的是doFirst方法");
        mv.setViewName("other");
        return mv;
    }
}
```

```jsp
修改 index 页面
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <P>第一个SpringMVC项目</P>
    <p><a href="test/some.do">发起some.do的get请求</a></p>
    <br>
    <form action="test/other.do" method="post">
        <input type="submit" value="post请求other.do">
    </form>
    <p><a href="test/first.do">发起first.do的默认(get)请求</a></p>
</body>
</html>
```



##### 2 处理器方法的参数

处理器方法可以包含以下四类参数，这些参数会在系统调用时由系统自动赋值，即程序员可在方法内直接使用。

- HttpServletRequest 
- HttpServletResponse 
- HttpSession 
- 请求中所携带的请求参数（逐个接收、对象接收）

![image-20220303205241346](05-SpringMVC.assets\image-20220303205241346.png)



###### 2.1 逐个参数接收

只要保证请求参数名与该请求处理方法的参数名相同即可。

Step1:修改 index 页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <P>提交参数给Controller</P>
    <form action="receiveProperty.do" method="post">
        姓名：<input type="text" name="name"><br>
        年龄：<input type="text" name="age"><br>
        <input type="submit" value="提交参数">
    </form>
</body>
</html>
```

Step2:修改处理器类 MyController

```java
@Controller
public class MyController {
    /*
     * 逐个接收请求的参数
     *   要求：处理器方法的形参名和请求中参数名必须一样
     *        同名的请求参数赋值给同名的形参
     *   框架接收请求参数
     *   1. 使用request对象接收请求参数
     *      String strName = request.getParameter("name");
     *      String strAge = request.getParameter("age");
     *   2. springmvc框架通过 DispatcherServlet 调用 MyController的doSome()方法
     *      调用方法时，按名称对应，把接收的参数赋值给形参
     *      doSome（strName，Integer.valueOf(strAge)）
     *      框架会提供类型转换的功能，能把String转为 int，long，float，double等类型。
     *
     *  400状态码是客户端错误，表示提交请求参数过程中，发生了问题。
     */
    @RequestMapping(value = "receiveProperty.do", method = RequestMethod.POST)
    public ModelAndView doSome(String name, Integer age){
        System.out.println("name:" + name + "\nage:" + age);
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        mv.setViewName("show");
        return mv;
    }
}
```

Step3:添加 show 页面

在 /WEB-INF/jsp 下添加 show.jsp 页面。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <h3>show.jsp 从request作用域获取数据</h3><br>
  <h4>myName数据:${myName}</h4><br>
  <h4>myAge数据:${myAge}</h4>
</body>
</html>
```



###### 2.2 请求参数中文乱码问题

```
注意：
 在提交请求参数时，get请求方式中文没有乱码。
 使用post方式提交请求，中文有乱码，需要使用过滤器处理乱码的问题。
 
过滤器可以自定义，也可使用框架中提供的过滤器 CharacterEncodingFilter
```

对于前面所接收的请求参数，若含有中文，则会出现中文乱码问题。Spring 对于请求参
数中的中文乱码问题，给出了专门的字符集过滤器：spring-web-5.2.5.RELEASE.jar 的
org.springframework.web.filter 包下的 CharacterEncodingFilter 类。

1. 解决方案

   在 web.xml 中注册字符集过滤器，即可解决 Spring 的请求参数的中文乱码问题。不过，
   最好将该过滤器注册在其它过滤器之前。因为过滤器的执行是按照其注册顺序进行的。

   直接在 web.xml 上直接进行设置字符集过滤器

   ```xml
   	<!--web.xml-->
   	<!--注册声明过滤器，解决Post请求乱码的问题-->
       <filter>
           <filter-name>characterEncodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <!--设置项目中使用的字符编码-->
           <init-param>
               <param-name>encoding</param-name>
               <param-value>utf-8</param-value>
           </init-param>
           <!--强制请求对象(HttpServletRequest)使用encoding编码的值-->
           <init-param>
               <param-name>forceRequestEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
           <!--强制应答对象(HttpServletResponse)使用encoding编码的值-->
           <init-param>
               <param-name>forceResponseEncoding</param-name>
               <param-value>true</param-value>
           </init-param>
       </filter>
   
       <filter-mapping>
           <filter-name>characterEncodingFilter</filter-name>
           <!-- /*:表示强制所有的请求先通过过滤器处理 -->
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   ```

2. 源码分析

   字符集设置核心方法：

   ![image-20220305140159864](05-SpringMVC.assets\image-20220305140159864.png)



###### 2.3 校正请求参数名@RequestParam

所谓校正请求参数名，是指若请求 URL 所携带的参数名称与处理方法中指定的参数名
不相同时，则需在处理方法参数前，添加一个注解 @RequestParam(“请求参数名”)，指定请
求 URL 所携带参数的名称。该注解是对处理器方法参数进行修饰的。value 属性指定请求参
数的名称。

Step1：修改 index 页面

将表单中的参数名称修改的与原来不一样。

```jsp
	<p>请求参数名和处理器方法的形参名不一样</p>
    <form action="receiveparam.do" method="post">
        姓名：<input type="text" name="rname"> <br/>
        年龄：<input type="text" name="rage"> <br/>
        <input type="submit" value="提交参数">
    </form>
```

Step2：修改处理器类 MyController

```java
	/*
     * 请求中参数名和处理器方法的形参名不一样
     * @RequestParam: 逐个接收请求参数中，解决请求中参数名形参名不一样的问题
     *      属性： 1. value 请求中的参数名称
     *            2. required 是一个boolean，默认是true
     *                true：表示请求中必须包含此参数。
     *                false：可以没有此参数
     *      位置： 在处理器方法的形参定义的前面
     */
    @RequestMapping(value = "/receiveparam.do")
    public ModelAndView receiveParam(@RequestParam(value = "rname", required = false) String name,
                                     @RequestParam(value = "rage", required = false) Integer age) {
        System.out.println("doSome, name=" + name + "  age=" + age);
        //可以在方法中直接使用 name ， age
        //处理some.do请求了。 相当于service调用处理完成了。
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        //show是视图文件的逻辑名称（文件名称）
        mv.setViewName("show");
        return mv;
    }
```



###### 2.4 对象参数接收

将处理器方法的参数定义为一个对象，只要保证请求参数名与这个对象的属性同名即可。

Step1：index 页面

```jsp
 	<p>使用Java对象接受请求参数</p>
    <form action="receiveObject.do" method="post">
        姓名：<input type="text" name="name"> <br/>
        年龄：<input type="text" name="age"> <br/>
        <input type="submit" value="提交参数">
    </form>
```

Step2：定义类 Student

```java
package com.springmvc.vo;

//保存请求参数值的一个普通类
public class Student {
    // 属性名和请求中参数名一样
    private String name;
    private Integer age;
	// get set toString
}
```

Step3：修改处理器类 MyController

```java
	/*
     * 处理器方法形参是java对象，这个对象的属性名和请求中参数名一样的
     * 框架会创建形参的java对象，给属性赋值。请求中的参数是name，框架会调用setName()
     */
    @RequestMapping(value = "/receiveObject.do")
    public ModelAndView receiveObject(Student myStudent) {
        System.out.println("myStduent=" + myStudent);
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", myStudent.getName());
        mv.addObject("myAge", myStudent.getAge());
        mv.addObject("myStudent", myStudent);
        mv.setViewName("show");
        return mv;
    }
```

Step4：修改 show 页面

```jsp
<body>
  <h3>show.jsp 从request作用域获取数据</h3>
  <h4>myName数据:${myName}</h4>
  <h4>myAge数据:${myAge}</h4>
  <h4>student数据:${myStudent}</h4>
</body>
```



##### 3 处理器方法的返回值

使用@Controller 注解的处理器的处理器方法，其返回值常用的有四种类型：

- ModelAndView (同时处理数据和视图)
- String
- 无返回值 void
- 返回自定义类型对象



###### 3.1 返回 ModelAndView

若处理器方法处理完后，需要跳转到其它资源，且又要在跳转的资源间传递数据，此时
处理器方法返回 ModelAndView 比较好。当然，若要返回 ModelAndView，则处理器方法中
需要定义 ModelAndView 对象。

在使用时，若该处理器方法只是进行跳转而不传递数据，或只是传递数据而并不向任何
资源跳转（如对页面的 Ajax 异步响应），此时若返回 ModelAndView，则将总是有一部分多
余：要么 Model 多余，要么 View 多余。即此时返回 ModelAndView 将不合适。



###### 3.2 返回 String

处理器方法返回的字符串可以指定逻辑视图名，通过视图解析器解析可以将其转换为物
理视图地址

**返回内部资源逻辑视图名**

若要跳转的资源为内部资源，则视图解析器可以使用 InternalResourceViewResolver 内部
资源视图解析器。此时处理器方法返回的字符串就是要跳转页面的文件名去掉文件扩展名后
的部分。这个字符串与视图解析器中的 prefix、suffix 相结合，即可形成要访问的 URI。

![image-20220305163657330](05-SpringMVC.assets\image-20220305163657330.png)

index 页面

```jsp
<body>
    <P>处理器方法返回String表示视图名称</P>
    <form action="returnString-view.do" method="post">
        姓名：<input type="text" name="name"><br>
        年龄：<input type="text" name="age"><br>
        <input type="submit" value="提交参数">
    </form>
    <br/>
</body>
```

处理器类 MyController

```java
@Controller
public class MyController {
    /*
    * 处理器方法返回String---表示逻辑视图名称，需要配置视图解析器
    * */
    @RequestMapping(value = "/returnString-view.do", method = RequestMethod.POST)
    public String doReturnView(HttpServletRequest request, String name, Integer age) {
        System.out.println("doReturnView name:" + name + "\nage:" + age);
        //可以自己手工添加数据到request作用域
        request.setAttribute("myName",name);
        request.setAttribute("myAge",age);
        // 逻辑视图名称，项目中配置了视图解析器
        // 框架对视图执行 forward 转发操作
        // /WEB-INF/jsp/show.jsp
        return "show";
    }
}
```

show 页面

```jsp
<body>
  <h3>show.jsp 从request作用域获取数据</h3>
  <h4>myName数据:${myName}</h4>
  <h4>myAge数据:${myAge}</h4>
</body>
```

当然，也可以直接**返回资源的物理视图名**。不过，此时就不需要再在视图解析器中再配
置前辍与后辍了。

```java
	//处理器方法返回String，表示完整视图路径，此时不能配置视图解析器
    @RequestMapping(value = "/returnString-view2.do")
    public String doReturnView2(HttpServletRequest request,String name, Integer age){
        System.out.println("===doReturnView2====, name="+name+"   age="+age);
        //可以自己手工添加数据到request作用域
        request.setAttribute("myname",name);
        request.setAttribute("myage",age);
        // 完整视图路径，项目中不能配置视图解析器
        // 框架对视图执行forward转发操作
        return "/WEB-INF/view/show.jsp";
    }
```



###### 3.3 返回 void

对于处理器方法返回 void 的应用场景，AJAX 响应

若处理器对请求处理后，无需跳转到其它任何资源，此时可以让处理器方法返回 void。

Step1：maven 加入 jackson 依赖

由于本项目中服务端向浏览器传回的是 JSON 数据，需要使用一个工具类将字符串包装
为 JSON 格式，所以需要导入 JSON 的依赖。

```xml
	<!--pom.xml-->
	<!--Jackson依赖-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
```

Step2：引入 jQuery 库

由于本项目要使用 jQuery 的 ajax()方法提交 AJAX 请求，所以项目中需要引入 jQuery 的
库。在 webapp下新建一个 Folder（文件夹），命名为 js，并将 jquery-3.6.0.js 文件放入其
中。

![image-20220306142227841](05-SpringMVC.assets\image-20220306142227841.png)

Step3：定义 index 页面

index 页面由两部分内容构成：一个是<button/>，用于提交 AJAX 请求；一个是<script/>，
用于处理 AJAX 请求。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            $("#btn").click(function (){
                $.ajax({
                    url:"returnvoid-ajax.do",
                    data:{
                        name:"zhangsan",
                        age:20
                    },
                    type:"post",
                    dataType:"json",
                    success:function (resp){
                        //resp从服务器端返回的是json格式的字符串 {"name":"zhangsan","age":20}
                        //jquery会把字符串转为json对象， 赋值给resp形参。
                        alert(resp.name + "    "+resp.age);
                    }
                })
            })
        })
    </script>
</head>
    
<body>
    <button id="btn">发起ajax请求</button>
</body>
</html>
```

Step4: 定义对象 Student

```java
public class Student{
	//属性名和请求参数名一样
	private String name;
	private Integer age;
	// set get
}
```

Step5：修改处理器类 MyController

处理器对于 AJAX 请求中所提交的参数，可以使用逐个接收的方式，也可以以对象的方
式整体接收。只要保证 AJAX 请求参数与接收的对象类型属性同名。

以逐个方式接收参数：

```java
package com.springmvc.controller;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.springmvc.vo.Student;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;
import java.io.PrintWriter;

@Controller
public class MyController {
    // 处理器方法返回void，响应ajax请求
    // 手工实现ajax，json数据：代码有重复的 1.java对象转为json；2.通过HttpServletResponse输出json数据
    @RequestMapping(value = "/returnvoid-ajax.do", method = RequestMethod.POST)
    public void doReturnVoid(HttpServletResponse response, String name, Integer age) throws IOException {
        System.out.println("doReturnVoidAjax name:" + name + "\nage:" + age);
        // 处理ajax，使用json做数据的格式
        // service调用完成了，使用Student表示处理结果
        Student student = new Student();
        student.setName(name);
        student.setAge(age);
        //把结果的对象转为json格式的数据
        String json = "";
        if(student != null){
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(student);
            System.out.println(student + " ==> " + json);
        }
        // 输出数据，响应ajax的请求
        response.setContentType("application/json;charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println(json);
        writer.flush();
        writer.close();
    }
}
```



###### 3.4 返回对象Object

处理器方法也可以返回 Object 对象。这个 Object 可以是 Integer，String，自定义对象，
Map，List 等。但返回的对象不是作为逻辑视图出现的，而是作为直接在页面显示的数据出
现的。

返回对象，需要使用@ResponseBody 注解，将转换后的 JSON 数据放入到响应体中。

1. 环境搭建

    maven pom.xml

   由于返回 Object 数据，一般都是将数据转化为了 JSON 对象后传递给浏览器页面的。而
   这个由 Object 转换为 JSON，是由 Jackson 工具完成的。所以需要导入 Jackson 的相关 Jar 包。

   ```xml
   	<!--Jackson依赖-->
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-core</artifactId>
         <version>2.9.0</version>
       </dependency>
       <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.9.0</version>
       </dependency>
   ```

   

   声明注解驱动

   将 Object 数据转化为 JSON 数据，需要由**消息转换器 HttpMessageConverter** 完成。而转
   换器的开启，需要由<mvc:annotation-driven/ >来完成。

   SpringMVC 使用消息转换器实现请求数据和对象，处理器方法返回对象和响应输出之间
   的自动转换

   当 Spring 容器进行初始化过程中，在 <mvc:annotation-driven/ > 处创建注解驱动时，默认
   创建了七个 HttpMessageConverter 对象。也就是说，我们注册 <mvc:annotation-driven/ > ，就
   是为了让容器为我们创建 HttpMessageConverter 对象。

   ![image-20220306145918905](05-SpringMVC.assets\image-20220306145918905.png)

   HttpMessageConverter 接口 : HttpMessageConverter<T>是 Spring3.0 新添加的一个接口，
   负责将请求信息转换为一个对象（类型为 T），将对象（类型为 T）输出为响应信息

   

   HttpMessageConverter<T>接口定义的方法：

   - boolean canRead(Class<?> clazz, MediaType mediaType)：指定转换器可以读取的对象类型，即
     转换器是否可将请求信息转换为 clazz 类型的对象，同时指定支持 MIME 类型
     ( text/html, applaiction/json 等)
   - boolean canWrite(Class<?> clazz, MediaType mediaType)：指定转换器是否可将 clazz 类型的对
     象写到响应流中，响应流支持的媒体类型在 MediaType 中定义。
   - LIst<MediaType> getSupportMediaTypes()：该转换器支持的媒体类
     型。
   - T read(Class<? extends T> clazz,HttpInputMessage inputMessage)：将请求信息流转换为 T 类型
     的对象。
   - void write(T t, MediaType contnetType, HttpOutputMessgae outputMessage):将 T 类型的对象写
     到响应流中，同时指定相应的媒体类型为 contentType

   

   | *HttpMessageConverter 接口实现类*       | *作用*                                                       |
   | --------------------------------------- | ------------------------------------------------------------ |
   | ByteArrayHttpMessageConverter           | 负责读取二进制格式的数据和写出二进制格<br/>式的数据          |
   | **StringHttpMessageConverter**          | **负责读取字符串格式的数据和写出字符串格<br/>式的数据**      |
   | ResourceHttpMessageConverter            | 负责读取资源文件和写出资源文件数据                           |
   | SourceHttpMessageConverter              | 能够读 / 写 来 自 HTTP 的 请 求 与 响 应 的<br/>javax.xml.transform.Source ,支持 DOMSource, <br/>SAXSource, 和 StreamSource 的 XML 格式 |
   | AllEncompassingFormHttpMessageConverter | 负责处理表单(form)数据                                       |
   | Jaxb2RootElementHttpMessageConverter    | 使用 JAXB 负责读取和写入 xml 标签格式的数<br/>据             |
   | **MappingJackson2HttpMessageConverter** | **负责读取和写入 json 格式的数据。利用<br/>Jackson 的 ObjectMapper 读写 json 数据，操作<br/>Object 类型数据，可读取 application/json，响<br/>应媒体类型为 application/json** |

   

   2. 返回自定义类型对象

      返回自定义类型对象时，不能以对象的形式直接返回给客户端浏览器，而是将对象转换
      为 JSON 格式的数据发送给浏览器的。

      由于转换器底层使用了Jackson转换方式将对象转换为JSON数据，所以需要导入Jackson
      的相关 Jar 包或者依赖。

      Step1：定义数据类

      ```java
      public class Student{
      	//属性名和请求参数名一样
      	private String name;
      	private Integer age;
      	// set get toString()
      }
      ```

      Step2：springmvc.xml 中注册mvc的注解驱动

      ```xml
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xmlns:mvc="http://www.springframework.org/schema/mvc"
             xsi:schemaLocation="http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/context
             https://www.springframework.org/schema/context/spring-context.xsd
             http://www.springframework.org/schema/mvc
             https://www.springframework.org/schema/mvc/spring-mvc.xsd">	
      	<!--注册mvc的注解驱动-->
          <mvc:annotation-driven />
      ```

      Step3：修改处理器 MyController

      ```java
      @Controller
      public class MyController {
          /*
          * 处理器方法返回一个Student，通过框架转为json，响应ajax请求
          * @ResponseBody:
          *   作用：把处理器方法返回对象转为json后，通过HttpServletResponse输出给浏览器
          *   位置：方法的定义上面。和其他注解没有顺序的关系
          * 返回对象框架的处理流程
          * 1.框架会把返回Student类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
          *   检查那个HttpMessageConverter接口的实现类能处理Student类型的数据--MappingJackson2HttpMessageConverter
          * 2.框架会调用实现类的write()，MappingJackson2HttpMessageConverter的write()方法
          *   把李四同学的student对象转为json，调用Jackson的ObjectMapper实现转为json
          *   contentType:application/json;charset=utf-8
          * 3.框架会调用 @ResponseBody 把2的结果数据输出到浏览器，ajax请求处理完成
          * */
          @RequestMapping(value = "/retuenStudentJson.do")
          @ResponseBody
          public Student doStudentJsonObject(){
              // 调用Service，获取请求结果数据，Student对象表示结果数据
              Student student = new Student();
              student.setName("李四同学");
              student.setAge(20);
              return student;// 会被框架转为json
          }
      }
      ```

      Step4：修改 index 页面

      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
          <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
          <script type="text/javascript">
              $(function (){
                  $("#btn").click(function (){
                      $.ajax({
                          url:"retuenStudentJson.do",
                          type:"post",
                          dataType:"json",
                          success:function (resp){
                              // {"name":"李四同学","age":20}
                              alert(resp.name + "    "+resp.age);
                          }
                      })
                  })
              })
          </script>
      </head>
      <body>
          <button id="btn">发起ajax请求</button>
      </body>
      </html>
      ```

      

   3. 返回 List 集合

      Step1：处理器 MyController

      ```java
      @Controller
      public class MyController {
          /*
           * 处理器方法返回List<Student>
           * 返回对象框架的处理流程：
           *  1.框架会把返回List<Student>类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
           *    检查那个HttpMessageConverter接口的实现类能处理Student类型的数据--MappingJackson2HttpMessageConverter
           *  2.框架会调用实现类的write（）， MappingJackson2HttpMessageConverter的write()方法
           *    把李四同学的student对象转为json， 调用Jackson的ObjectMapper实现转为json array
           *    contentType: application/json;charset=utf-8
           *  3.框架会调用@ResponseBody把2的结果数据输出到浏览器， ajax请求处理完成
           */
          @RequestMapping(value = "/retuenStudentJsonList.do")
          @ResponseBody
          public List<Student> doStudentJsonObjectArray(){
              List<Student> list = new ArrayList<>();
              // 调用Service，获取请求结果数据，Student对象表示结果数据
              Student student = new Student();
              student.setName("李四同学");
              student.setAge(20);
              list.add(student);
      
              student = new Student();
              student.setName("张三");
              student.setAge(28);
              list.add(student);
              return list;// 会被框架转为json数组
          }
      }
      ```

      Step2：index 页面

      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
          <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
          <script type="text/javascript">
              $(function (){
                  $("#btn").click(function (){
                      $.ajax({
                          url:"retuenStudentJsonList.do",
                          type:"post",
                          dataType:"json",
                          success:function (resp){
                              // resp = "[{"name":"李四同学","age":20},{"name":"张三","age":28}]";
                              $.each(resp,function(i,n){
                                  alert(n.name + "   " + n.age)
                              })
                          }
                      })
                  })
              })
          </script>
      </head>
      <body>
          <button id="btn">发起ajax请求</button>
      </body>
      </html>
      ```

      

   4. 返回字符串对象

      若要返回非中文字符串，将前面返回数值型数据的返回值直接修改为字符串即可。但若
      返回的字符串中带有中文字符，则接收方页面将会出现乱码。此时需要使用 
      @RequestMapping 的 produces 属性指定字符集。

      produces，产品，结果，即该属性用于设置输出结果类型。

      Step1：处理器 MyController

      ```java
      @Controller
      public class MyController {
          /*
          * 处理器方法返回String，String表示数据，不是视图
          * 区分返回值String是数据，还是视图，看有没有@ResponseBody注解
          * 如果有@ResponseBody注解，返回String就是数据，反之就是视图
          *
          * 默认使用“text/plain;charset=ISO-8859-1”作为contentType,导致中文有乱码，
          * 解决方案：给RequestMapping增加一个属性 produces, 使用这个属性指定新的contentType.
          * 返回对象框架的处理流程：
          *  1.框架会把返回String类型，调用框架的中ArrayList<HttpMessageConverter>中每个类的canWrite()方法
          *    检查那个HttpMessageConverter接口的实现类能处理String类型的数据--StringHttpMessageConverter
          *  2.框架会调用实现类的write()，StringHttpMessageConverter的write()方法
          *    把字符按照指定的编码处理 text/plain;charset=ISO-8859-1
          *  3.框架会调用@ResponseBody把2的结果数据输出到浏览器， ajax请求处理完成
          * */
          @RequestMapping(value = "/returnStringData.do", produces = "text/plain;charset=utf-8")
          @ResponseBody
          public String doStringData(){
              return "Hello SpringMVC 返回String对象，表示数据";
          }
      }
      ```

      Step2：index 页面

      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
          <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
          <script type="text/javascript">
              $(function (){
                  $("#btn").click(function (){
                      $.ajax({
                          url:"returnStringData.do",
                          type:"post",
                          dataType:"text",
                          success:function (resp){
                              alert("返回的是文本数据：" + resp);
                          }
                      })
                  })
              })
          </script>
      </head>
      <body>
          <button id="btn">发起ajax请求</button>
      </body>
      </html>
      ```

###### 3.5 小结

```
ch04-return: 处理器方法的返回值表示请求的处理结果
1.ModelAndView: 有数据和视图，对视图执行forward。
2.String:表示视图，可以逻辑名称，也可以是完整视图路径
3.void: 不能表示数据，也不能表示视图。
  在处理ajax的时候，可以使用void返回值。 通过HttpServletResponse输出数据。响应ajax请求。
  ajax请求服务器端返回的就是数据，和视图无关。
4.Object：例如String，Integer，Map，List，Student等等都是对象，
  对象有属性，属性就是数据。所以返回Object表示数据，和视图无关。
  可以使用对象表示的数据，响应ajax请求。

  现在做ajax，主要使用json的数据格式。实现步骤：
   1.加入处理json的工具库的依赖，springmvc默认使用的jackson。
   2.在sprigmvc配置文件之间加入 <mvc:annotation-driven> 注解驱动。
      json = om.writeValueAsString(student);
   3.在处理器方法的上面加入@ResponseBody注解
      response.setContentType("application/json;charset=utf-8");
      PrintWriter pw  = response.getWriter();
      pw.println(json);

  springmvc处理器方法返回Object，可以转为json输出到浏览器，响应ajax的内部原理
  1.<mvc:annotation-driven> 注解驱动。
     注解驱动实现的功能是 完成java对象到json，xml，text，二进制等数据格式的转换。
     <mvc:annotation-driven>在加入到springmvc配置文件后，会自动创建HttpMessageConverter接口
     的7个实现类对象， 包括 MappingJackson2HttpMessageConverter （使用jackson工具库中的ObjectMapper实现java对象转为json字符串）

     HttpMessageConverter接口：消息转换器。
     功能：定义了java转为json，xml等数据格式的方法。 这个接口有很多的实现类。
           这些实现类完成 java对象到json，java对象到xml，java对象到二进制数据的转换

     下面的两个方法是控制器类把结果输出给浏览器时使用的：
     boolean canWrite(Class<?> var1, @Nullable MediaType var2);
     void write(T var1, @Nullable MediaType var2, HttpOutputMessage var3)

     例如处理器方法
     @RequestMapping(value = "/returnString.do")
     public Student doReturnView2(HttpServletRequest request, String name, Integer age){
             Student student = new Student();
             student.setName("lisi");
             student.setAge(20);
             return student;
     }

     1）canWrite作用检查处理器方法的返回值，能不能转为var2表示的数据格式。
       检查student(lisi，20)能不能转为var2表示的数据格式。如果检查能转为json，canWrite返回true
       MediaType：表示数格式的，例如json，xml等等

     2）write：把处理器方法的返回值对象，调用jackson中的ObjectMapper转为json字符串。
       json  = om.writeValueAsString(student);

  2.@ResponseBody注解
     放在处理器方法的上面，通过HttpServletResponse输出数据，响应ajax请求的。
           PrintWriter pw  = response.getWriter();
           pw.println(json);
           pw.flush();
           pw.close();

=====================================================================================
没有加入注解驱动标签<mvc:annotation-driven /> 时的状态
org.springframework.http.converter.ByteArrayHttpMessageConverter
org.springframework.http.converter.StringHttpMessageConverter
org.springframework.http.converter.xml.SourceHttpMessageConverter
org.springframework.http.converter.support.AllEncompassingFormHttpMessageConverter

加入注解驱动标签时<mvc:annotation-driven />的状态
org.springframework.http.converter.ByteArrayHttpMessageConverter
org.springframework.http.converter.StringHttpMessageConverter
org.springframework.http.converter.ResourceHttpMessageConverter
org.springframework.http.converter.ResourceRegionHttpMessageConverter
org.springframework.http.converter.xml.SourceHttpMessageConverter
org.springframework.http.converter.support.AllEncompassingFormHttpMessageConverter
org.springframework.http.converter.xml.Jaxb2RootElementHttpMessageConverter
org.springframework.http.converter.json.MappingJackson2HttpMessageConverter
```



##### 4 解读<url-pattern/  >

###### 4.1 配置详解

1. ***.do**

   在没有特殊要求的情况下，SpringMVC 的中央调度器 DispatcherServlet 的 <url-pattern/ >
   常使用后辍匹配方式，如写为*.do 或者 *.action，*.mvc 等。

2. **/**

   可以写为/，因为 DispatcherServlet 会将向静态资源的获取请求，例如 .css、.js、.jpg、.png
   等资源的获取请求，当作是一个普通的 Controller 请求。中央调度器会调用处理器映射器为
   其查找相应的处理器。当然也是找不到的，所以在这种情况下，所有的静态资源获取请求也
   均会报 404 错误。



在 index.jsp 页面中存在一个访问图片的链接。

该项目用于演示将<url-pattern/ >写为 *.do
可以访问到该图片

而写为/，则无法访问(导致静态资源访问失败404，动态资源可以正常访问)。

```xml
发起的请求是由哪些服务器程序处理的。
http://localhost:8080/ch05_url_pattern/index.jsp ：tomcat（jsp会转为servlet）
http://localhost:8080/ch05_url_pattern/js/jquery-3.4.1.js ： tomcat
http://localhost:8080/ch05_url_pattern/images/p1.jpg ： tomcat
http://localhost:8080/ch05_url_pattern/html/test.html： tomcat
http://localhost:8080/ch05_url_pattern/some.do ：  DispatcherServlet（springmvc框架处理的）

tomcat本身能处理静态资源的访问，像html，图片，js文件都是静态资源
tomcat的web.xml文件有一个servlet 名称是 default ， 在服务器启动时创建的。
 	<servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
	<servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>/</url-pattern>  <!--表示静态资源和未映射的请求都这个default处理-->
    </servlet-mapping>

default这个servlet作用： 
1.处理静态资源
2.处理未映射到其它servlet的请求。
```



###### 4.2 静态资源访问

<url-pattern/ >的值并不是说写为/后，静态资源就无法访问了。经过一些配置后，该问题也是可以解决的。

1. 使用<mvc:default-servlet-handler/ >

   声明了 <mvc:default-servlet-handler / > 后，springmvc 框架会在容器中创建
   DefaultServletHttpRequestHandler 处理器对象。它会像一个检查员，对进入 DispatcherServlet
   的 URL 进行筛查，如果发现是静态资源的请求，就将该请求转由 Web 应用服务器默认的
   Servlet 处理。一般的服务器都有默认的 Servlet。

   在 Tomcat 中，有一个专门用于处理静态资源访问的 Servlet 名叫 DefaultServlet。其
   <servlet-name/ >为 default。可以处理各种静态资源访问请求。该 Servlet 注册在 Tomcat 服务
   器的 web.xml 中。在 Tomcat 安装目录/conf/web.xml。

   ![image-20220306165340345](05-SpringMVC.assets\image-20220306165340345.png)

   

   只需要在 springmvc.xml 中添加<mvc:default-servlet-handler/ >标签即可。

   ![image-20220306165704702](05-SpringMVC.assets\image-20220306165704702.png)

   <mvc:default-servlet-handler/ >表示使用 DefaultServletHttpRequestHandler 处理器对象。
   而该处理器调用了 Tomcat 的 DefaultServlet 来处理静态资源的访问请求。

   当然了，要想使用<mvc: …/>标签，需要引入 mvc 约束

   ![image-20220306165818200](05-SpringMVC.assets\image-20220306165818200.png)

   该约束可从 Spring 帮助文档中搜索关键字 spring-mvc.xsd 即可获取：

   docs/spring-framework-reference/htmlsingle/index.html

   

   解决动态资源和静态资源冲突的问题，在 springmvc 配置文件中声明注解驱动

   ![image-20220306170334023](05-SpringMVC.assets\image-20220306170334023.png)

   

2. 使用<mvc:resources/ >

   在 Spring3.0 版本后，Spring 定义了专门用于处理静态资源访问请求的处理器
   ResourceHttpRequestHandler。并且添加了<mvc:resources/ >标签，专门用于解决静态资源无
   法访问的问题。需要在 springmvc 配置文件中添加如下形式的配置：

   ![image-20220306172631758](05-SpringMVC.assets\image-20220306172631758.png)

   

   解决动态资源和静态资源冲突的问题，在 springmvc 配置文件中声明注解驱动

   ![image-20220306172715088](05-SpringMVC.assets\image-20220306172715088.png)

    ![image-20220306172937951](05-SpringMVC.assets\image-20220306172937951.png)

    

##### 5 绝对地址和相对地址

```
在jsp，html中使用的地址，都是在前端页面中的地址，都是相对地址
地址分类：
 1.绝对地址，带有协议名称的是绝对地址，http://www.baidu.com, ftp://202.122.23.1
 2.相对地址，没有协议开头的，例如 user/some.do, /user/some.do
            相对地址不能独立使用，必须有一个参考地址。通过参考地址+相对地址本身才能指定资源。
 3.参考地址
    1） 在你的页面中的，访问地址不加 "/"
	   访问的是：http://localhost:8080/ch06_path/index.jsp
	   路径：http://localhost:8080/ch06_path/
	   资源：index.jsp
    在index.jsp发起 user/some.do 请求，访问地址变为 http://localhost:8080/ch06_path/user/some.do
	当你的地址没有斜杠开头,例如 user/some.do，
	当你点击链接时，访问地址 = 当前页面的地址 + 链接的地址。
	http://localhost:8080/ch06_path/ + user/some.do
    
    -------------------------------------------------------------
	index.jsp 访问 user/some.do，返回后现在的地址：http://localhost:8080/ch06_path/user/some.do
	http://localhost:8080/ch06_path/user/some.do
	路径：http://localhost:8080/ch06_path/user/
	资源：some.do

	在index.jsp 再点击 user/some.do，就变为 http://localhost:8080/ch06_path/user/user/some.do

	解决方案：
	1.加入${pageContext.request.contextPath}
	2.加入一个base标签，是html语言中的标签。 表示当前页面中访问地址的基地址。
	  你的页面中所有没有“/”开头的地址，都是以base标签中的地址为参考地址
	  使用base中的地址 + user/some.do 组成访问地址

   2）在你的页面中的，访问地址加 "/"
      访问的是：http://localhost:8080/ch06_path/index.jsp
	  路径：http://localhost:8080/ch06_path/
	  资源：index.jsp

	  点击 /user/some.do, 访问地址变为 http://localhost:8080/user/some.do
	  参考地址是 你的服务器地址， 也就是 http://localhost:8080

	  如果你的资源不能访问： 加入${pageContext.request.contextPath}
		<a href="${pageContext.request.contextPath}/user/some.do">发起user/some.do的get请求</a>

index.jsp--addStudent.jsp---student/addStudent.do( service的方法，调用dao的方法)--result.jsp
```

![image-20220306175324876](05-SpringMVC.assets\image-20220306175324876.png)



### 三、SSM 整合开发

SSM 编程，即 SpringMVC + Spring + MyBatis 整合，是当前最为流行的 JavaEE 开发技术架
构。其实 SSM 整合的实质，仅仅就是将 MyBatis 整合入 Spring。因为 SpringMVC原本就是 Spring
的一部分，不用专门整合。

SSM 整合的实现方式可分为两种：基于 XML 配置方式，基于注解方式。

SSM整合注解开发

```
SSM整合开发。
SSM：SpringMVC + Spring + MyBatis.

SpringMVC:视图层，界面层，负责接收请求，显示处理结果的。
Spring：业务层，管理service，dao，工具类对象的。
MyBatis：持久层，访问数据库的

用户发起请求-->SpringMVC接收-->Spring中的Service对象-->MyBatis处理数据

SSM整合也叫做SSI (IBatis也就是mybatis的前身)，整合中有容器。
1.第一个容器SpringMVC容器，管理Controller控制器对象的。
2.第二个容器Spring容器，管理Service、Dao、工具类对象的
我们要做的把使用的对象交给合适的容器创建，管理。把Controller还有web开发的相关对象
交给springmvc容器，这些web用的对象写在springmvc配置文件中
service，dao对象定义在spring的配置文件中，让spring管理这些对象。

springmvc容器和spring容器是有关系的，关系已经确定好了
springmvc容器是spring容器的子容器，类似java中的继承。子可以访问父的内容；
在子容器中的Controller可以访问父容器中的Service对象，就可以实现controller使用service对象

实现步骤：
0.使用springdb的mysql库，表使用student（id auto_increment, name, age）
1.新建maven web项目
2.加入依赖
  springmvc，spring，mybatis三个框架的依赖，jackson依赖，mysql驱动，druid连接池
  jsp，servlet依赖
3.写web.xml
    1)注册DispatcherServlet ,目的：
            1.创建springmvc容器对象，才能创建Controller类对象。
            2.创建的是Servlet，才能接受用户的请求。
    2）注册spring的监听器：ContextLoaderListener，目的：创建spring的容器对象，才能创建service，dao等对象。
    3）注册字符集过滤器，解决post请求乱码的问题。
4.创建包，controller，service，dao，实体类(domain)包名创建好
5.写springmvc，spring，mybatis的配置文件
    1）springmvc配置文件
    2）spring配置文件
    3）mybatis主配置文件
    4）数据库的属性配置文件
6.写代码，dao接口和mapper文件，service和实现类，controller，实体类
7.写jsp页面
```

##### 1 建表Student

student 表

![image-20220308190135375](05-SpringMVC.assets\image-20220308190135375.png)



##### 2 新建 web 工程

骨架：maven-archetype-webapp

工程名称：ssm



##### 3 maven 依赖

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.ssm</groupId>
  <artifactId>ch07-SSM</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!-- jsp依赖 -->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2.1-b03</version>
      <scope>provided</scope>
    </dependency>
    <!--springmvc-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <!--事务的-->  
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <!--jackson-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
    </dependency>
     <!--mybatis 和 spring 整合的-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <!--mybatis-->  
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>
    <!--mysql 驱动-->  
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.9</version>
    </dependency>
    <!--druid-->  
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory><!--所在的目录-->
        <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>
    <plugins>
      <plugin>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```



##### 4 定义包，组织程序的结构

![image-20220310192026690](05-SpringMVC.assets\image-20220310192026690.png)



##### 5 编写配置文件

jdbc 属性配置文件 jdbc.properties

```properties
jdbc.url=jdbc:mysql://localhost:3306/bjpowernode
jdbc.username=root
jdbc.password=111
```

Spring 配置文件 applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">
    <!--Spring的配置文件，声明Service、dao和其他工具类相关的对象-->

    <!--声明数据源，连接数据库-->
    <context:property-placeholder location="classpath:conf/jdbc.properties" />
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <property name="maxActive" value="20" />
    </bean>

    <!--SqlSessionFactoryBean 创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" >
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:conf/mybatis.xml" />
    </bean>

    <!--声明myBatis的扫描器，创建dao对象-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer" >
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.ssm.dao" />
    </bean>

    <!--声明service的注解 @Service 所在的包名位置-->
    <context:component-scan base-package="com.ssm.service" />

    <!--事务配置：注解的配置，aspectj的配置-->
</beans>
```

Springmvc 配置文件：dispatcherServlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--SpringMVC的配置文件，声明controller和其他web相关的对象-->
    <!--Controller层 组件扫描器-->
    <context:component-scan base-package="com.ssm.controller" />

    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>

    <!--注解驱动-->
    <!-- 1.响应ajax请求，返回json -->
    <!-- 2.解决静态资源访问问题 -->
    <mvc:annotation-driven />
</beans>
```

mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--设置mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>

    <!--设置别名-->
    <typeAliases>
        <!--name：实体类所在的包名-->
        <package name="com.ssm.domain"/>
    </typeAliases>

    <mappers>
        <!-- name：是包名，这个包中的所有 mapper.xml 一次都能加载 -->
        <!--
            使用package的要求：
                1.mapper文件名称和dao接口名必须完全一样，包括大小写
                2.mapper文件和dao接口必须在同一目录
        -->
        <package name="com.ssm.dao"/>
    </mappers>
</configuration>
```



##### 6 定义 web.xml

1. 注册 ContextLoaderListener
2. 注册 DisatcherServlet
3. 注册字符集过滤器
4. 同时创建 Spring 的配置文件和 SpringMVC 的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--中央调度器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:conf/dispatcherServlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
    
    <!--注册spring的监听器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:conf/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--注册字符集过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```



##### 7 实体类Student

```java
package com.ssm.domain;
public class Student {
    private Integer id;
    private String name;
    private Integer age;

    public Integer getId() { return id; }
    public void setId(Integer id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
    @Override
    public String toString() {
        return "Student{" + "id=" + id + ", name='" + name + '\'' + ", age=" + age + '}';
    }
}
```



##### 8 Dao 接口和 sql 映射文件

```java
package com.ssm.dao;
import com.ssm.domain.Student;
import java.util.List;
public interface StudentDao {
    int insertStudent(Student student);
    List<Student> selectStudents();
}
```

StudentDao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ssm.dao.StudentDao">
    <select id="selectStudents" resultType="com.ssm.domain.Student">
        select id, name, age from student order by id asc
    </select>

    <insert id="insertStudent">
        insert into student(name,age) values(#{name},#{age})
    </insert>
</mapper>
```



##### 9 Service 接口和实现类

```java
package com.ssm.service;
import com.ssm.domain.Student;
import java.util.List;
public interface StudentService {
    int addStudent(Student student);
    List<Student> findStudents();
}
```

```java
package com.ssm.service.impl;
import com.ssm.dao.StudentDao;
import com.ssm.domain.Student;
import com.ssm.service.StudentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import java.util.List;
@Service
public class StudentServiceImpl implements StudentService {
    //引用类型自动注入@Autowired, @Resource
    @Autowired
    private StudentDao studentDao;

    @Override
    public int addStudent(Student student) {
        int nums = studentDao.insertStudent(student);
        return nums;
    }

    @Override
    public List<Student> findStudents() {
        return studentDao.selectStudents();
    }
}
```



##### 10 处理器类

```java
package com.ssm.controller;

@Controller
@RequestMapping("/student")
public class StudentController {
    @Resource
    private StudentService service;

    //注册学生
    @RequestMapping("/addStudent.do")
    public ModelAndView addStudent(Student student){
        ModelAndView mv = new ModelAndView();
        String tips = "注册失败";
        //调用service处理student
        int nums = service.addStudent(student);
        if(nums > 0){
            tips = "学生【" + student.getName() + "】注册成功！";
        }
        //添加数据
        mv.addObject("tips", tips);
        //指定结果页面
        mv.setViewName("result");
        return mv;
    }

    //处理查询，响应ajax
    @RequestMapping("/queryStudent.do")
    @ResponseBody
    public List<Student> queryStudents(){
        //参数检查，简单的处理数据
        List<Student> students = service.findStudents();
        return students;
    }
}
```



##### 11 定义视图-首页文件--- index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
  String basePath = request.getScheme() + "://" +
          request.getServerName() + ":" + request.getServerPort() +
          request.getContextPath() + "/";
%>
<html>
<head>
    <title>功能入口</title>
    <base href="<%=basePath%>" />
</head>
<body>
  <div align="center" >
    <p>SSM整合的例子</p>
    <img src="images/ssm.jpg">
    <table>
      <tr>
        <td><a href="addStduent.jsp">注册学生</a></td>
      </tr>
      <tr>
        <td><a href="listStudent.jsp">浏览学生</a></td>
      </tr>
    </table>
  </div>

</body>
</html>
```

![image-20220310193953091](05-SpringMVC.assets\image-20220310193953091.png)



##### 12 注册学生页面 --- addStudent.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    String basePath = request.getScheme() + "://" +
            request.getServerName() + ":" + request.getServerPort() +
            request.getContextPath() + "/";
%>
<html>
<head>
    <title>注册学生</title>
    <base href="<%=basePath%>" />
</head>
<body>
<div align="center">
    <form action="student/addStudent.do" method="post">
        <table>
            <tr>
                <td>姓名：</td>
                <td><input type="text" name="name"></td>
            </tr>
            <tr>
                <td>年龄：</td>
                <td><input type="text" name="age"></td>
            </tr>
            <tr>
                <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                <td><input type="submit" value="注册"></td>
            </tr>
        </table>
    </form>
</div>
</body>
</html>
```

![image-20220310194015907](05-SpringMVC.assets\image-20220310194015907.png)



##### 13 浏览学生页面 --- listStudent.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    String basePath = request.getScheme() + "://" +
            request.getServerName() + ":" + request.getServerPort() +
            request.getContextPath() + "/";
%>
<html>
<head>
    <title>查询学生ajax</title>
    <base href="<%=basePath%>" />
    <script type="text/javascript" src="js/jquery-3.6.0.js"></script>
    <script type="text/javascript">
        $(function (){
            // 在当前页面dom对象加载后，执行loadStudentData()
            loadStudentData();
            $("#btnLoader").click(function (){
                loadStudentData();
            })
        })

        function loadStudentData(){
            $.ajax({
                url:"student/queryStudent.do",
                type:"get",
                dataType:"json",
                success:function (data){
                    // 清除旧的数据
                    $("#info").html("");
                    //增加新的数据
                    $.each(data, function (i, n){
                        $("#info").append("<tr>")
                            .append("<td>"+ n.id + "</td>")
                            .append("<td>"+ n.name + "</td>")
                            .append("<td>"+ n.age + "</td>")
                            .append("</tr>")
                    })
                }
            })
        }
    </script>
</head>
<body>
    <div align="center">
        <table border="1px">
            <thead>
            <tr>
                <td>学号</td>
                <td>姓名</td>
                <td>年龄</td>
            </tr>
            </thead>
            <tbody id="info">

            </tbody>
        </table>
        <input type="button" id="btnLoader" value="查询数据">
    </div>
</body>
</html>
```



##### 14 结果页面--- result.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
result.jsp 结果页面，注册结果：${tips}
</body>
</html>
```

![image-20220310194104655](05-SpringMVC.assets\image-20220310194104655.png)



### 四、SpringMVC 核心技术

##### 1 请求重定向和转发

当处理器对请求处理完毕后，向其它资源进行跳转时，有两种跳转方式：请求转发与重
定向。而根据所要跳转的资源类型，又可分为两类：跳转到页面与跳转到其它处理器。

注意，对于请求转发的页面，可以是WEB-INF中页面；而重定向的页面，是不能为 WEB-INF中页面的。因为重定向相当于用户再次发出一次请求，而用户是不能直接访问 WEB-INF 中资
源的。

![image-20220312200218987](05-SpringMVC.assets\image-20220312200218987.png)

​		

SpringMVC 框架把原来 Servlet 中的请求转发和重定向操作进行了封装。

现在可以使用简
单的方式实现转发和重定向。

forward: 表示转发，实现 request.getRequestDispatcher("xx.jsp").forward()

redirect: 表示重定向，实现 response.sendRedirect("xxx.jsp")



###### 1.1 请求转发

处理器方法返回 ModelAndView 时，需在 setViewName() 指定的视图前添加 forward:，且
此时的视图不再与视图解析器一同工作，这样可以在配置了解析器时指定不同位置的视图。
视图页面必须写出相对于项目根的路径。forward 操作不需要视图解析器。

处理器方法返回 String，在视图路径前面加入 forward: 视图完整路径。

```java
package com.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/*
 * 处理器方法返回ModelAndView，实现转发forward
 * 语法：setViewName("forward:视图文件完整路径")
 * 特点：不和视图解析器一同使用，就当项目中没有视图解析器
 */
@Controller
public class MyController {
    @RequestMapping(value = "/doForward.do", method = RequestMethod.POST)
    public ModelAndView doSome(){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("msg","欢迎使用springmvc做web开发");
        mv.addObject("fun","执行的是doSome方法");
        // 显示转发
        //mv.setViewName("forward:/WEB-INF/jsp/show.jsp");
        mv.setViewName("forward:/other.jsp");// 解除视图解析器的限制
        return mv;
    }
}
```



###### 1.2 请求重定向

在处理器方法返回的视图字符串的前面添加 redirect:，则可实现重定向跳转。

```jsp
index.jsp
	<P>当出来方法返回ModelAndView实现redirect</P>
    <form action="doRedirect.do" method="post" >
        姓名：<input type="text" name="name"><br>
        年龄：<input type="text" name="age" ><br>
        <input type="submit" value="提交请求">
    </form>
```

```java
MyController.java
	/*
     * 处理器方法返回ModelAndView，实现重定向redirect
     * 语法：setViewName("redirect:视图文件完整路径")
     * redirect特点：不和视图解析器一同使用，就当项目中没有视图解析器
     *
     * 框架对重定向的操作：
     * 1.框架会把Model中的简单类型的数据，转为string使用，作为hello.jsp的get请求参数使用。
     *   目的是在 doRedirect.do 和 hello.jsp 两次请求之间传递数据
     * 2.在目标hello.jsp页面可以使用参数集合对象 ${param}获取请求参数值
     *    ${param.myname}
     * 3.重定向不能访问/WEB-INF资源
     */
    @RequestMapping(value = "/doRedirect.do", method = RequestMethod.POST)
    public ModelAndView doOther(String name, Integer age){
        ModelAndView mv  = new ModelAndView();
        mv.addObject("myName",name);
        mv.addObject("myAge",age);
        // 重定向
        mv.setViewName("redirect:/hello.jsp");
        // http://localhost:8080/ch08_forward/hello.jsp?myName=岳云&myAge=22
        //重定向不能访问/WEB-INF资源
        //mv.setViewName("redirect:/WEB-INF/view/show.jsp");
        return mv;
    }
```

```jsp
hello.jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  <h4>myName数据:${param.myName}</h4>
  <h4>myAge数据:${param.myAge}</h4>
  <h4>取参数数据:<%=request.getParameter("myName")%></h4>
</body>
</html>
```

![image-20220312205020762](05-SpringMVC.assets\image-20220312205020762.png)

![image-20220312205043268](05-SpringMVC.assets\image-20220312205043268.png)



##### 2 异常处理

SpringMVC 框架处理异常的常用方式：使用@ExceptionHandler 注解处理异常。



@ExceptionHandler 注解

使用注解@ExceptionHandler 可以将一个方法指定为异常处理方法。该注解只有一个可选属性 value，为一个   Class<?>数组，用于指定该注解的方法所要处理的异常类，即所要匹
配的异常。

而被注解的方法，其返回值可以是 ModelAndView、String，或 void，方法名随意，方法
参数可以是 Exception 及其子类对象、HttpServletRequest、HttpServletResponse 等。系统会
自动为这些方法参数赋值。

对于异常处理注解的用法，也可以直接将异常处理方法注解于 Controller 之中。



```
异常处理：
springmvc 框架采用的是统一，全局的异常处理。
把controller中的所有异常处理都集中到一个地方。
采用的是aop的思想。把业务逻辑和异常处理代码分开。解耦合。

使用两个注解
1.@ExceptionHandler
2.@ControllerAdvice

异常处理步骤：
1.新建maven web项目
2.加入依赖
3.新建一个自定义异常类 MyUserException , 再定义它的子类NameException, AgeException
4.在controller抛出NameException , AgeException
5.创建一个普通类，作用全局异常处理类
    1) 在类的上面加入@ControllerAdvice
    2) 在类中定义方法，方法的上面加入@ExceptionHandler
6.创建处理异常的视图页面
7.创建springmvc的配置文件
    1）组件扫描器，扫描 @Controller 注解
    2）组件扫描器，扫描 @ControllerAdvice 所在的包名
    3）声明注解驱动
```

###### 2.1 自定义异常类

定义三个异常类：NameException、AgeException、MyUserException。其中 MyUserException
是另外两个异常的父类。

```java
package com.springmvc.exception;
public class MyUserException extends Exception {
    public MyUserException() {}
    public MyUserException(String message) { super(message); }
}
```

```java
package com.springmvc.exception;

//当用户的姓名有异常，抛出NameException
public class NameException extends MyUserException{
    public NameException() { super(); }
    public NameException(String message) { super(message); }
}
```

```java
package com.springmvc.exception;

//当用户的年龄有异常，抛出AgeException
public class AgeException extends MyUserException{
    public AgeException() { super(); }
    public AgeException(String message) { super(message); }
}
```



###### 2.2 修改 Controller 类抛出异常

```java
package com.springmvc.controller;

import com.springmvc.exception.AgeException;
import com.springmvc.exception.MyUserException;
import com.springmvc.exception.NameException;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@Controller
public class MyController {
    @RequestMapping(value = "/some.do", method = RequestMethod.POST)
    public ModelAndView doSome(String name, Integer age) throws MyUserException {
        ModelAndView mv  = new ModelAndView();
        // 根据请求参数抛出异常
        if(!"zs".equals(name)){
            throw new NameException("姓名不是zs！");
        }
        if(age == null || age > 80){
            throw new AgeException("年龄过大！");
        }
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        mv.setViewName("show");
        return mv;
    }
}
```



###### 2.3 定义异常响应页面

定义三个异常响应页面。

nameError.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  nameError.jsp<br>
  提示信息：${msg}<br>
  系统异常信息：${exception.message}
</body>
</html>
```

ageError.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  ageError.jsp<br>
  提示信息：${msg}<br>
  系统异常信息：${exception.message}
</body>
</html>
```

defaultError.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  defaultError.jsp<br>
  提示信息：${msg}<br>
  系统异常信息：${exception.message}
</body>
</html>
```



###### 2.4 定义全局异常处理类

```java
package com.springmvc.handler;

import com.springmvc.exception.AgeException;
import com.springmvc.exception.NameException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;
/*
 * @ControllerAdvice : 控制器增强（也就是说给控制器类增加功能----异常处理功能）
 *   位置：在类的上面
 *   特点：必须让框架知道这个注解所在的包名，需要在springmvc配置文件声明组件扫描器。
 *   指定 @Controller 所在的包名
 */
@ControllerAdvice
public class GlobalExceptionHandler {
    /*
     * 定义方法，处理发生的异常
     * 处理异常的方法和控制器方法的定义一样，可以有多个参数；
     * 可以有ModelAndView、String、void、对象类型的返回值
     *
     * 形参：Exception，表示Controller中抛出的异常对象
     * 通过形参可以获取发生的异常信息
     *
     * @ExceptionHandler(对应异常类的class):表示异常的类型，当发生此类异常时，
     * 由当前方法处理。
     */
    @ExceptionHandler(value = NameException.class)
    public ModelAndView doNameException(Exception exception){
        /*
         * 处理NameException异常
         * 异常发生处理逻辑：
         * 1.需要把异常记录下来，记录到数据库，日志文件。
         *   记录日志发生的时间，哪个方法发生的，异常错误内容
         * 2.发送通知，把异常的信息通过邮件，短信，微信发送给相关人员
         * 3.给用户友好的提示
         */
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "姓名必须是zs，其他用户不能访问！");
        mv.addObject("exception", exception);
        mv.setViewName("nameError");
        return mv;
    }

    @ExceptionHandler(value = AgeException.class)
    public ModelAndView doAgeException(Exception exception){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "您的年龄不能大于80！");
        mv.addObject("exception", exception);
        mv.setViewName("ageError");
        return mv;
    }

    // 处理其他异常，NameException、AgeException以外，不知类型的异常
    @ExceptionHandler
    public ModelAndView doOtherException(Exception exception){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "其他异常");
        mv.addObject("exception", exception);
        mv.setViewName("defaultError");
        return mv;
    }
}
```



###### 2.5 定义 SpringMVC 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--声明 springmvc框架中的视图解析器，帮助开发人员设置视图文件的路径-->
    <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀：视图文件的路径-->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!--后缀：视图文件的扩展名-->
        <property name="suffix" value=".jsp" />
    </bean>


    <context:component-scan base-package="com.springmvc.controller" />

    <!--处理异常的两步-->
    <!--声明组件扫描器-->
    <context:component-scan base-package="com.springmvc.handler" />
    <!--声明注解驱动-->
    <mvc:annotation-driven />
</beans>
```



##### 3 拦截器

SpringMVC 中的 Interceptor 拦截器是非常重要和相当有用的，它的主要作用是拦截指定的用户请求，并进行相应的预处理与后处理。其拦截的时间点在 “处理器映射器根据用户提
交的请求映射出了所要执行的处理器类，并且也找到了要执行该处理器类的处理器适配器，
在处理器适配器执行处理器之前” 。当然，在处理器映射器映射出所要执行的处理器类时，
已经将拦截器与处理器组合为了一个处理器执行链，并返回给了中央调度器。

```
拦截器：
1）拦截器是springmvc中的一种，需要实现HandlerInterceptor接口。
2）拦截器和过滤器类似，功能方向侧重点不同。 过滤器是用来过滤器请求参数，设置编码字符集等工作。
    拦截器是拦截用户的请求，做请求做判断处理的。
3）拦截器是全局的，可以对多个Controller做拦截。 
   一个项目中可以有0个或多个拦截器， 他们在一起拦截用户的请求。
	拦截器常用在：用户登录处理，权限检查，记录日志。

拦截器的使用步骤：
 1.定义类实现HandlerInterceptor接口
 2.在springmvc配置文件中，声明拦截器，让框架知道拦截器的存在。

拦截器的执行时间：
  1）在请求处理之前，也就是controller类中的方法执行之前先被拦截。
  2）在控制器方法执行之后也会执行拦截器。
  3）在请求处理完成后也会执行拦截器。

拦截器：看做是多个Controller中公用的功能，集中到拦截器统一处理。使用的aop的思想
```



###### 3.1 一个拦截器的执行

```
步骤：
1.新建maven web项目
2.加入依赖
3.创建Controller类
4.创建一个普通类，作为拦截器使用
    1）实现HandlerInterceptor接口
    2）实现接口中的三个方法
5.创建show.jsp
6.创建springmvc的配置文件
    1)组件扫描器，扫描@Controller注解
    2)声明拦截器，并指定拦截的请求uri地址
```

```java
package com.springmvc.handler;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Date;

// 拦截器类：拦截用户的请求
public class MyInterceptor implements HandlerInterceptor {
    private long bigTime = 0l;

    /**
     * preHandle:叫做预处理方法
     *   重要：是整个项目的入口，门户。
     *        当preHandle返回true，请求可以被处理
     *        当preHandle返回false，请求到此被截止
     * @param handler 被拦截的控制器对象MyController
     * @return boolean值
     *              true:请求通过了拦截器的验证，可以执行处理器方法
     *                拦截器MyInterceptor的preHandle()方法
     *                MyController中的doSome方法
     *                拦截器MyInterceptor的postHandle()方法
     *                拦截器MyInterceptor的afterCompletion()方法
     *              false:请求没有通过拦截器的验证，请求到达拦截器就截止了，请求没有被处理
     *                拦截器MyInterceptor的preHandle()方法
     * @throws Exception
     * 特点：1.方法在控制器方法(MyController的doSome方法)之前执行的。
     *        用户的请求首先到达此方法
     *      2.在这个方法中可以获取请求的信息，验证请求是否符合要求；
     *        可以验证用户是否登录，验证用户是否有权限访问某个链接地址；
     *        如果验证失败，可以截断请求，请求不能被处理。
     *        如果验证成功，可以放行请求，此时控制器方法才能执行。
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
                             Object handler) throws Exception {
        bigTime = System.currentTimeMillis();
        System.out.println("拦截器MyInterceptor的preHandle()方法");
        // 计算的业务逻辑，根据计算结果，返回true或者false
        // 给浏览器一个返回结果
        //request.getRequestDispatcher("/tips.jsp").forward(request, response);
        //return fasle;
        return true;
    }

    /**
     * postHandle:后处理方法
     * @param handler 被拦截的处理器对象MyController
     * @param modelAndView mv:处理器方法的返回值
     * 特点：
     *    1.在处理器方法之后执行的（MyController.doSome()）
     *    2.能够获取到处理器方法的返回值ModelAndView，可以修改ModelAndView中的数据和
     *      视图，可以影响到最后的执行结果。
     *    3.主要是对原来的执行结果做二次修正
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response,
                           Object handler, ModelAndView modelAndView) throws Exception {
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
        System.out.println("拦截器MyInterceptor的postHandle()方法");
        // 对原来doSome的执行结果，需要调整
        if(modelAndView != null){
            modelAndView.addObject("myDate", new Date());//修改数据
            modelAndView.setViewName("other");//修改视图
        }
    }

    /**
     * afterCompletion:最后执行的方法
     * @param handler 被拦截的处理器对象
     * @param ex 程序中发生的异常
     * 特点：
     *    1.在请求处理完成后执行的；框架中规定是当你的视图处理完成后，对视图执行了forward，就认为请求处理完成
     *    2.一般做资源回收工作的，程序请求过程中创建了一些对象，在这里可以删除，把占用的内存回收。
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                Object handler, Exception ex) throws Exception {
        System.out.println("拦截器MyInterceptor的afterCompletion()方法");
        long endTime = System.currentTimeMillis();
        System.out.println("计算从preHandle到请求处理完成的时间：" + (endTime - bigTime));
    }
}
```



自定义拦截器，需要实现 HandlerInterceptor 接口。而该接口中含有三个方法：

- preHandle(request, response, Object handler)：该方法在处理器方法执行之前执行。其返回值为 boolean，若为 true，则紧接着会执行处理器方
  法，且会将 afterCompletion()方法放入到一个专门的方法栈中等待执行。
- postHandle(request, response, Object handler, modelAndView)：该方法在处理器方法执行之后执行。处理器方法若最终未被执行，则该方法不会执行。
  由于该方法是在处理器方法执行完后执行，且该方法参数中包含 ModelAndView，所以该方法可以修
  改处理器方法的处理结果数据，且可以修改跳转方向。
- afterCompletion(request, response, Object handler, Exception ex)：当 preHandle() 方法返回 true 时，会将该方法放到专门的方法栈中，等到对请求进行响应的所有
  工作完成之后才执行该方法。即该方法是在中央调度器渲染（数据填充）了响应页面之后执行的，此
  时对 ModelAndView 再操作也对响应无济于事。



**afterCompletion 最后执行的方法，清除资源，例如在 Controller 方法中加入数据**

![image-20220313195423029](05-SpringMVC.assets\image-20220313195423029.png)



拦截器中方法与处理器方法的执行顺序如下图：

![image-20220313195454006](05-SpringMVC.assets\image-20220313195454006.png)



1. 注册拦截器

   在springmvc.xml中注册拦截器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!--声明组件扫描器-->
       <context:component-scan base-package="com.springmvc.controller" />
   
       <!--声明 springmvc框架中的视图解析器，帮助开发人员设置视图文件的路径-->
       <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!--前缀：视图文件的路径-->
           <property name="prefix" value="/WEB-INF/jsp/" />
           <!--后缀：视图文件的扩展名-->
           <property name="suffix" value=".jsp" />
       </bean>
   
       <!--声明拦截器：拦截器可以有0个或多个-->
       <mvc:interceptors>
           <!--声明第一个拦截器-->
           <mvc:interceptor>
               <!--
                   指定拦截的请求uri地址
                   path：就是uri地址，可以使用通配符 **
                       **：表示任意的字符，文件或者多级目录和目录中的文件
                   http://localhost:8080/myweb/user/listUser.do
               -->
               <mvc:mapping path="/user/**" />
               <!--声明拦截器对象-->
               <bean class="com.springmvc.handler.MyInterceptor" />
           </mvc:interceptor>
       </mvc:interceptors>
   </beans>
   ```

2. 修改 index 页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
       <%
           String basePath = request.getScheme() + "://" +
                   request.getServerName() + ":" + request.getServerPort() +
                   request.getContextPath() + "/";
       %>
       <base href="<%=basePath%>" />
   </head>
   <body>
       <P>一个拦截器</P>
       <form action="some.do" method="post" >
           姓名：<input type="text" name="name"><br>
           年龄：<input type="text" name="age" ><br>
           <input type="submit" value="提交请求">
       </form>
       <br>
   </body>
   </html>
   ```

3. 修改处理器

   ```java
   package com.springmvc.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class MyController {
       @RequestMapping(value = "/some.do", method = RequestMethod.POST)
       public ModelAndView doSome(String name, Integer age) {
           System.out.println("MyController中的doSome方法");
           ModelAndView mv  = new ModelAndView();
           mv.addObject("myName", name);
           mv.addObject("myAge", age);
           mv.setViewName("show");
           return mv;
       }
   }
   ```

4. 修改结果页面

   show.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
     <h4>myName数据:${myName}</h4>
     <h4>myAge数据:${myAge}</h4>
   </body>
   </html>
   ```

   other.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
     <h4>myName数据:${myName}</h4>
     <h4>myAge数据:${myAge}</h4>
     <h4>拦截器中增加的数据:${myDate}</h4>
   </body>
   </html>
   ```

   tips.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
     tips.jsp  请求被拦截，不能执行
   </body>
   </html>
   ```



###### 3.2 多个拦截器的执行

1. 建立两个自定义拦截器

   ```java
   package com.bjpowernode.handler;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.util.Date;
   
   //拦截器类：拦截用户的请求。
   public class MyInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("111111-拦截器的MyInterceptor的preHandle()");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request,
                              HttpServletResponse response,
                              Object handler, ModelAndView mv) throws Exception {
           System.out.println("111111-拦截器的MyInterceptor的postHandle()");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                   Object handler, Exception ex) throws Exception {
           System.out.println("111111-拦截器的MyInterceptor的afterCompletion()");
       }
   }
   
   ```

   ```java
   package com.bjpowernode.handler;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   //拦截器类：拦截用户的请求。
   public class MyInterceptor2 implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("22222-拦截器的MyInterceptor的preHandle()");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request,
                              HttpServletResponse response,
                              Object handler, ModelAndView mv) throws Exception {
           System.out.println("22222-拦截器的MyInterceptor的postHandle()");
       }
   
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response,
                                   Object handler, Exception ex) throws Exception {
           System.out.println("22222-拦截器的MyInterceptor的afterCompletion()");
       }
   }
   ```

2. 多个拦截器的注册与执行

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
   
       <!--声明组件扫描器-->
       <context:component-scan base-package="com.bjpowernode.controller" />
   
       <!--视图解析器-->
       <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/view/" />
           <property name="suffix" value=".jsp" />
       </bean>
   
       <!--在框架中保存多个拦截器是ArrayList，按照声明的先后顺序放入到ArrayList-->
       <mvc:interceptors>
           <!--声明第一个拦截器-->
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <bean class="com.bjpowernode.handler.MyInterceptor" />
           </mvc:interceptor>
   
           <!--声明第二个拦截器-->
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <bean class="com.bjpowernode.handler.MyInterceptor2" />
           </mvc:interceptor>
       </mvc:interceptors>
   </beans>
   ```

3. 控制台执行结果

   当有多个拦截器时，形成拦截器链。拦截器链的执行顺序，与其注册顺序一致。需要再
   次强调一点的是，当某一个拦截器的 preHandle()方法返回 true 并被执行到时，会向一个专
   门的方法栈中放入该拦截器的 afterCompletion()方法。

   多个拦截器中方法与处理器方法的执行顺序如下图：

   ![image-20220313202745230](05-SpringMVC.assets\image-20220313202745230.png)

   从图中可以看出，只要有一个 preHandle()方法返回 false，则上部的执行链将被断开，
   其后续的处理器方法与 postHandle()方法将无法执行。但，无论执行链执行情况怎样，只要
   方法栈中有方法，即执行链中只要有 preHandle()方法返回 true，就会执行方法栈中的
   afterCompletion()方法。最终都会给出响应。

   ```
   多个拦截器：
   第一个拦截器preHandle=true , 第二个拦截器preHandle=true 
   
   111111-拦截器的MyInterceptor的preHandle()
   22222-拦截器的MyInterceptor的preHandle()
   =====执行MyController中的doSome方法=====
   22222-拦截器的MyInterceptor的postHandle()
   111111-拦截器的MyInterceptor的postHandle()
   22222-拦截器的MyInterceptor的afterCompletion()
   111111-拦截器的MyInterceptor的afterCompletion()
   
   ---------------------------------------------------
   第一个拦截器preHandle=true , 第二个拦截器preHandle=false
   
   111111-拦截器的MyInterceptor的preHandle()
   22222-拦截器的MyInterceptor的preHandle()
   111111-拦截器的MyInterceptor的afterCompletion()
   
   ----------------------------------------------------------
   第一个拦截器preHandle=false , 第二个拦截器preHandle=true|false
   
   111111-拦截器的MyInterceptor的preHandle()
   ```



###### 3.3 拦截器与过滤器的区别

1. 过滤器是servlet的对象，拦截器是框架中的对象。

2. 过滤器实现Filter接口的对象，拦截器是实现HandlerInterctor

3. 过滤器是用来设置request、response的参数、属性的，侧重是对数据过滤；

   拦截器是用来验证请求的，能截断请求。

4. 过滤器是在拦截器之前执行的。

5. 过滤器是Tomcat服务器创建的对象；

   拦截器是SpringMVC容器中创建的对象。

6. 过滤器只有一个执行时间点；

   拦截器有三个执行时间点。

7. 过滤器可以处理jsp、js、html等等；

   拦截器是侧重拦截对Controller的请求；如果你的请求不能被DispatcherServlet接收，这个请求不会执行拦截器内容。

8. 拦截器拦截普通类方法执行，过滤器过滤servlet请求响应。



###### 3.4 权限拦截器举例

```
使用拦截器检查登录的用户是不是能访问系统
实现步骤：
1.新建maven
2.修改web.xml注册中央调度器
3.新建index.jsp发起请求
4.创建MyController处理请求
5.创建结果show.jsp
6.创建一个login.jsp，模拟登录（把用户的信息放入到session）；
  创建一个jsp，logout.jsp，模拟退出系统（从session中删除数据）
7.创建拦截器，从session中获取用户的登录数据，验证能否访问系统
8.创建一个验证的jsp，如果验证视图，给出提示
9.创建springmvc配置文件
  声明组件扫描器
  声明拦截器
```

只有经过登录的用户方可访问处理器，否则，将返回“无权访问”提示。

本例的登录，由一个 JSP 页面完成。即在该页面里将用户信息放入 session 中。也就是
说，只要访问过该页面，就说明登录了。没访问过，则为未登录用户。

1. index.jsp 页面

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <%
       String basePath = request.getScheme() + "://" +
               request.getServerName() + ":" + request.getServerPort() +
               request.getContextPath() + "/";
   %>
   <html>
   <head>
       <title>Title</title>
       <base href="<%=basePath%>" />
   </head>
   <body>
       <p>一个拦截器</p>
       <form action="some.do" method="post">
           姓名：<input type="text" name="name"> <br/>
           年龄：<input type="text" name="age"> <br/>
           <input type="submit" value="提交请求">
       </form>
   </body>
   </html>
   
   ```

2. Controller 类

   ```java
   package com.bjpowernode.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   public class MyController {
       @RequestMapping(value = "/some.do")
       public ModelAndView doSome(String name,Integer age) {
           System.out.println("=====执行MyController中的doSome方法=====");
           //处理some.do请求了。 相当于service调用处理完成了。
           ModelAndView mv  = new ModelAndView();
           mv.addObject("myname",name);
           mv.addObject("myage",age);
           mv.setViewName("show");
           return mv;
       }
   }
   ```

3. 定义权限拦截器

   当 preHandle()方法返回 false 时，需要使用 request 或 response 对请求进行响应。

   ```java
   package com.bjpowernode.handler;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.util.Date;
   
   //拦截器类：拦截用户的请求。
   public class MyInterceptor implements HandlerInterceptor {
       //验证登录的用户信息， 正确return true，其它return false
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("111111-拦截器的MyInterceptor的preHandle()");
           String loginName = "";
           //从session中获取name的值
           Object attr  = request.getSession().getAttribute("name");
           if( attr != null){
               loginName = (String)attr;
           }
   
           //判断登录的账户，是否符合要求
           if( !"zs".equals(loginName)){
               //不能访问系统
               //给用户提示
               request.getRequestDispatcher("/tips.jsp").forward(request,response);
               return false;
           }
   
           //zs登录
           return true;
       }
   }
   ```

4. 注册权限拦截器

   在springmvc.xml中注册权限拦截器

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
       <!--声明组件扫描器-->
       <context:component-scan base-package="com.bjpowernode.controller" />
   
       <!--声明 springmvc框架中的视图解析器， 帮助开发人员设置视图文件的路径-->
       <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!--前缀：视图文件的路径-->
           <property name="prefix" value="/WEB-INF/view/" />
           <!--后缀：视图文件的扩展名-->
           <property name="suffix" value=".jsp" />
       </bean>
   
       <!--声明拦截器： 拦截器可以有0或多个
           在框架中保存多个拦截器是ArrayList，
           按照声明的先后顺序放入到ArrayList
       -->
       <mvc:interceptors>
           <!--声明第一个拦截器-->
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <!--声明拦截器对象-->
               <bean class="com.bjpowernode.handler.MyInterceptor" />
           </mvc:interceptor>
       </mvc:interceptors>
   </beans>
   ```

5. 定义页面

   login.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
       模拟登录,zs登录系统
       <%
           session.setAttribute("name","zs");
       %>
   </body>
   </html>
   ```

   logout.jsp

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
      退出系统，从session中删除数据
     <%
         session.removeAttribute("name");
     %>
   </body>
   </html>
   ```

   tips.jsp 请求被截止后

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
     tips.jsp  非zs不能访问系统
   </body>
   </html>
   ```

   show.jsp 请求没被截止

   ```jsp
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
       <h3>/WEB-INF/view/show.jsp从request作用域获取数据</h3><br/>
       <h3>myname数据：${myname}</h3><br/>
       <h3>myage数据：${myage}</h3>
   </body>
   </html>
   ```

   
