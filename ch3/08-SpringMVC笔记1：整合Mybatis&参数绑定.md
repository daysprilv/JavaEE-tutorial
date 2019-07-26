## 前言

SpringMVC 第一天学习大纲：

``` xml
1. SpringMVC 介绍
2. 入门程序
3. SpringMVC 架构讲解
   1. 框架结构
   2. 组件说明
4. SpringMVC 整合 Mybatis
5. 参数绑定
   1. SpringMVC 默认支持的的类型
   2. 简单数据类型
   3. POJO 类型
   4. POJO 包装类型
   5. 自定义参数绑定
6. SpringMVC 和 Struts2 的区别
```



## 一、SpringMVC介绍

1、SpringMVC 是什么？

Spring Web MVC 和 Struts2 都属于表现层的框架，它是 Spring 框架的一部分，我们可以从 Spring 的整体结构中看得出来：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-77529065.jpg)

其作用是：从请求中接收传入的参数，将处理后的结果数据返回给页面展示。

**PS：** 补充点内容，我们聊聊 MVC 架构思想。MVC 是软件工程中的一种软件架构模式，是三个单词的缩写，它们是 Model（模型）、View（视图）和 Controller（控制）。

> 这个模式认为，程序不论简单或复杂，从结构上看，都可以分成三层。
>
> 1）最上面的一层，是直接面向最终用户的"视图层"（View）。它是提供给用户的操作界面，是程序的外壳。
>
> 2）最底下的一层，是核心的"数据层"（Model），也就是程序需要操作的数据或信息。
>
> 3）中间的一层，就是"控制层"（Controller），它负责根据用户从"视图层"输入的指令，选取"数据层"中的数据，然后对其进行相应的操作，产生最终结果。
>
> 这三层是紧密联系在一起的，但又是互相独立的，每一层内部的变化不影响其他层。每一层都对外提供接口（Interface），供上面一层调用。这样一来，软件就可以实现模块化，修改外观或者变更数据都不用修改其他层，大大方便了维护和升级。（*来源阮一峰老师文章 [谈谈MVC模式](http://www.ruanyifeng.com/blog/2007/11/mvc.html)*）

拿例子来解释，最典型的 MVC 就是 JSP+Servlet+JavaBean 模式，以用户登录网站为案例，在视图层（View）即 JSP 中有个表单，用户填写用户名和密码，点击提交，这时候就会跳转到控制层（Controller），控制层 Servlet 会接收到表单提交的用户名和密码。

> 注意：我们并不会在 Servlet 里面进行业务逻辑和数据库 SQL 编写，这样会显得杂乱不堪。MVC 架构的思想是，控制层接到用户名和密码送给 Service 层，在 Service 层中进行业务逻辑的编写，比如判断当前有没有此用户、密码是否正确。
>
> 判断密码的正确性需要查询数据库表，这个时候需要 Dao 层，Dao 层用来专门和数据库打交道，并把查询的结果返回给 Service 层，Service 层有了 Dao 层的返回结果便可进一步判断密码是否正确、用户名是否存在，从而给控制层一个答复，控制层接受到 Service层答复后进行跳转，比如：若密码错误，返回 json 值到视图层，视图层进行渲染展示给用户，若密码正确就直接跳转到主页面。

这种分层开发有利于项目的扩展和维护。代码实践参考菜鸟教程：[MVC 模式](http://www.runoob.com/design-pattern/mvc-pattern.html)

另外关于 JavaWeb 项目为什么要分层，网上这里有个更细致的解释：[为什么JavaWeb项目要分层？](https://zhidao.baidu.com/question/2206388866156884028.html?qbl=relate_question_2)

2、SpringMVC 的处理流程

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-60146579.jpg)



## 二、入门程序

需求：实现商品列表的展示。

需求分析：

1. 请求的 URL：..../itemList.action
2. 参数：无
3. 数据：静态数据

开发步骤：

1. 创建一个 JavaWeb 工程，导入以下 jar 包

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-18231910.jpg)

2. 在工程目录 WEB-INF 新建 jsp 文件夹，并新建 `itemList.jsp` 页面

3. 创建 ItemsController 

   > ItemController 是一个普通的 java 类，不需要实现任何接口，只需要在类上添加 `@Controller` 注解即可。`@RequestMapping` 注解指定请求的 url，其中 `.action` 可以加也可以不加。在 ModelAndView 对象中，将视图设置为 “/WEB-INF/jsp/itemList.jsp” 。

   cn.itheima.controller 下的 ItemsController.java：

   ``` java
   @Controller
   public class ItemsController {

       //指定url到请求方法的映射
       //url中输入一个地址,找到这个方法.例如:localhost:8081/springmvc0523/list.action
       @RequestMapping("/list")
       public ModelAndView  itemsList() throws Exception{
           List<Items> itemList = new ArrayList<>();

           //商品列表
           Items items_1 = new Items();
           items_1.setName("联想笔记本_3");
           items_1.setPrice(6000f);
           items_1.setDetail("ThinkPad T430 联想笔记本电脑！");

           Items items_2 = new Items();
           items_2.setName("苹果手机");
           items_2.setPrice(5000f);
           items_2.setDetail("iphone6苹果手机！");

           itemList.add(items_1);
           itemList.add(items_2);

           //模型和视图
           //model模型: 模型对象中存放了返回给页面的数据
           //view视图: 视图对象中指定了返回的页面的位置
           ModelAndView modelAndView = new ModelAndView();

           //将返回给页面的数据放入模型和视图对象中
           modelAndView.addObject("itemList", itemList);

           //指定返回的页面位置
           modelAndView.setViewName("itemList");

           return modelAndView;

       }
   }

   ```

   其中商品数据使用 Items 类描述，cn.itheima.pojo 下的 Items.java：

   ``` java
   public class Items {

       private Integer id;
       private String name;
       private Float price;
       private String pic;
       private Date createtime;
       private String detail;
       
       get/set 方法已省略......
   }

   ```

4. 创建 SpringMvc.xml 文件：

   ``` xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xmlns:p="http://www.springframework.org/schema/p"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:dubbo="http://code.alibabatech.com/schema/dubbo" 
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xsi:schemaLocation="http://www.springframework.org/schema/beans 
                              http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
                              http://www.springframework.org/schema/mvc 
                              http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
                              http://code.alibabatech.com/schema/dubbo 
                              http://code.alibabatech.com/schema/dubbo/dubbo.xsd
                              http://www.springframework.org/schema/context 
                              http://www.springframework.org/schema/context/spring-context-4.0.xsd">

       <!-- 配置@Controller注解扫描 -->
       <context:component-scan base-package="cn.itheima.controller"></context:component-scan>

   </beans>
   ```
   注：使用组件扫描器省去在 Spring 容器配置每个 Controller 类的繁琐。使用`<context:component-scan>`自动扫描标记`@Controller`的控制器类。

5. 配置前端控制器。在 WEB-INF/web.xml 中添加 DispatcherServlet 的配置：

   ``` xml
   <!-- spirngMvc前端控制器 -->
   <servlet>
       <servlet-name>spirngMvc0523</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

       <!-- 如果没有指定springMvc核心配置文件那么默认会去找/WEB-INF/+<servlet-name>中的内容 +   -servlet.xml配置文件 -->
       <!-- 指定springMvc核心配置文件位置 -->
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:SpringMvc.xml</param-value>
       </init-param>

       <!-- tomcat启动的时候就加载这个servlet -->
       <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
       <servlet-name>spirngMvc0523</servlet-name>
       <url-pattern>*.action</url-pattern>
   </servlet-mapping>
   </web-app>

   ```

6. 最后把项目添加到 Tomcat 下发布，浏览器访问 `localhost:8080/springmvc0523/list.action` 可以看到商品展示出来了


   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-34447945.jpg)

> **捋一捋：** Tomcat 启动后首先加载 web.xml 文件，加载该文件后把配置的 servlet 加载进来了，servlet 又配置了 SpringMVC 的核心配置文件，如上即 SpringMvc.xml，在 SpringMvc.xml 中配置了包扫描，即会把包下面所有有加有 `@Controller` 注解的类加载到内存变成对象了，那么我们在浏览器输入`localhost:8080/springmvc0523/list.action` 进行访问，它会把所有加有 `@Controller`注解的类的对象都扫描一遍，找到加有注解 `@RequestMapping` 并且值为 list 方法走一遍，最后把数据返回给页面。




## 三、SpringMVC架构

### 3.1 架构结构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-23409898.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-51527658.jpg)

​			（*图片来源  https://blog.csdn.net/Handsome2013/article/details/81563365*）

**架构流程：**

1. 用户发送请求至前端控制器 DispatcherServlet
2. DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器
3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器（如果有则生成）一并返回给DispatcherServlet
4. DispatcherServlet 通过 HandlerAdapter 处理器适配器调用处理器
5. 执行处理器（Controller，也叫后端控制器）
6. Controller 执行完成返回 ModelAndView
7. HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet
8. DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器
9. ViewReslover 解析后返回具体 View
10. DispatcherServlet 对 View 进行渲染视图（即将模型数据填充至视图中）。
11. DispatcherServlet 响应用户

**组件说明：** 

以下组件通常使用框架提供实现：

☛ DispatcherServlet：前端控制器（不需要程序员开发）

> 用户请求到达前端控制器，它就相当于 MVC 模式中的 C，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在降低了组件之间的耦合性。
>
> ***作用：作为接受请求，响应结果，相当于转发器、中央处理器，减少其他组件之间的耦合度。***

☛ HandlerMapping：处理器映射器

> HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。
>
> ***作用：根据请求的 URL 查找 Handler。*** 

☛ Handler：处理器**（需要程序员开发）**

> Handler 是继 DispatcherServlet 前端控制器的后端控制器，在 DispatcherServlet 的控制下 Handler 对具体的用户请求进行处理。
>
> 由于 Handler 涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发 Handler。
>
> ***注意：编写 Handler 时按照 HandlerAdpter 的要求去做，这样才可以去正确执行 Handler。***

☛ HandlAdapter：处理器适配器

> 通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-94577921.jpg)
>
> ***作用：按照特定的规则（HandlerAdapter 要求的规则）去执行 Handler。***

☛ ViewResolver：视图解析器

> ViewResolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。
>
> ***作用：进行视图解析，根据逻辑视图名解析成真正的视图（view）。***

☛ View：视图**（需要程序员开发）**

> SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView 等。我们最常用的视图就是 jsp。
>
> 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。

说明：在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件。需要用户开发的组件有 handler、view。

就以上说点个人解释：

> PS：首先先理解 Handler 是什么，就简单理解下，就是请求之后要执行的那个方法。如上代码即为 list 方法。
>
> 大概过程：用户发送请求给前端控制器，前端控制器请求处理器映射器（HandlerMapping）查询 Handler，HandlerMapping 的作用是什么呢？它记录了 URL 到请求方法的映射，如上在`ItemsController.java`代码中写的 `@RequestMapping("/list")`，然后浏览器输入请求地址能找到该方法，可以推测 HandlerMapping 内部其实是一个 Map  结构形式。
>
> 从图中可以看到，处理器映射器（HandlerMapping）返回的是处理器执行链，里面包含了很多拦截器，拦住了就踢出去，拦不住就返回 Handler 给前端控制器，但前端控制器不会自己去执行该 Handler，它会把该 Handler 发送给 HandlerAdapter（处理器适配器）请求执行。
>
> 但是对于 HandlerAdapter（处理器适配器）有不同种的，为什么有不同种的呢？因为你写的 Handler 可能有注解形式的，有实现接口的形式。从下面代码来看：
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-6568293.jpg)
>
> 除了可以使用 `@Controller` 注解形式，也可以使用实现接口的形式。
>
> 不同形式的 Handler 使用不同的处理器适配器（HandlerAdapter）去执行 Handler，执行完返回来一个 ModelAndView 给前端控制器，前端控制器会根据不同的视图找到不同的视图解析器，然后视图解析器返回 View 对象给前端控制器，前端控制器载通过视图渲染，最后返回给客户。

补充：打开`spring-webmvc-4.1.3.RELEASE.jar`包，找到 `DispatcherServlet.properties`：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-30955673.jpg)

可以看到 HandlerMapping（处理器映射器）、HandlerAdapter（处理器适配器）都有不同形式的，比如注解形式，也有非注解形式的。但我们在 SpringMVC 核心配置`SpringMVC.xml`文件未配置处理器映射器、处理器适配器，程序照样能运行，为什么呢？因为它会去默认的`DispatcherServlet.properties`中查找，但其实这样效率不高，因为每次请求都要扫描一次配置文件默认配置文件。所以为了效率，我们需要自己配置。SpringMVC.xml 中添加：

``` xml
<!-- 如果没有显示的配置处理器映射器和处理器适配那么springMvc会去默认的dispatcherServlet.properties中查找,
        对应的处理器映射器和处理器适配器去使用,这样每个请求都要扫描一次他的默认配置文件,效率非常低,会降低访问速度,所以要显示的配置处理器映射器和
        处理器适配器 -->

<!-- 注解形式的处理器映射器 -->
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"></bean> 
<!-- 注解形式的处理器适配器 -->
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"></bean> 

<!-- 配置最新版的注解的处理器映射器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean> 
<!-- 配置最新版的注解的处理器适配器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean> 

<!-- 注解驱动:
  作用:替我们自动配置最新版的注解的处理器映射器和处理器适配器
  -->
<!-- <mvc:annotation-driven></mvc:annotation-driven> -->


<!-- 配置视图解析器 
 作用:在controller中指定页面路径的时候就不用写页面的完整路径名称了,可以直接写页面去掉扩展名的名称
 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 真正的页面路径 =  前缀 + 去掉后缀名的页面名称 + 后缀 -->
    <!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/"></property>
    <!-- 后缀 -->
    <property name="suffix" value=".jsp"></property>
</bean>
```

注意：这里的配置的注解驱动与入门程序案例配置的注解扫描不要被搞晕了，入门程序案例中配置的注解扫描作用是自动扫描标记`@Controller`的类，而这里的注解驱动是替我们自动配置最新版的注解的处理器映射器和处理器适配器，完全是两个东西。

> 继续补充：
>
> 1、注解式处理器映射器，对类中标记`@ResquestMapping`的方法进行映射，根据ResquestMapping 定义的 URL 匹配 ResquestMapping 标记的方法，匹配成功返回 `HandlerMethod` 对象给前端控制器，HandlerMethod 对象中封装 URL 对应的方法 Method。 
>
> 从 Spring3.1 版本开始，废除了 DefaultAnnotationHandlerMapping 的使用，推荐使用 RequestMappingHandlerMapping 完成注解式处理器映射。
>
> 2、注解式处理器适配器，对标记 `@ResquestMapping` 的方法进行适配。
>
>  从 Spring3.1 版本开始，废除了 AnnotationMethodHandlerAdapter 的使用，推荐使用 RequestMappingHandlerAdapter 完成注解式处理器适配。
>
> 3、SpringMVC 使用`<mvc:annotation-driven>`自动加载 RequestMappingHandlerMapping 和 RequestMappingHandlerAdapter，可用在 SpringMVC.xml 配置文件中使用`<mvc:annotation-driven>`替代处理器映射器和适配器的配置。



## 四、SpringMVC整合Mybatis

#### 4.1 案例为例

为了更好的学习 SpringMVC 和 Mybatis 整合开发的方法，需要将 SpringMVC 和 Mybatis 进行整合。

整合目标：控制层采用 SpringMVC、持久层使用 Mybatis 实现。

导包：包括 Spring（包括 SpringMVC）、mybatis、mybatis-spring 整合包、数据库驱动、第三方连接池。

整合思路：

``` xml
1)Dao层：
    pojo和映射文件以及接口使用逆向工程生成
    SqlMapConfig.xml   mybatis核心配置文件
    ApplicationContext-dao.xml 整合后spring在dao层的配置
        a)	数据库连接池
        b)	SqlSessionFactory对象，需要spring和mybatis整合包下的。
        c)	配置mapper文件扫描器。
2)Service层：
    applicationContext-service.xml 包扫描器，扫描@service注解的类。
    applicationContext-trans.xml 配置事务。
3)controller层：
    SpringMvc.xml 
        注解扫描:扫描@Controller注解
        注解驱动:替我们显示的配置了最新版的处理器映射器和处理器适配器
        视图解析器:显示的配置是为了在controller中不用每个方法都写页面的全路径
4)Web.xml
    springMvc前端控制器配置
    spring监听
```

工程整体目录结构：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-39817266.jpg)

在 classpath 下创建 config/sqlMapConfig.xml：（*关于什么是 classpath 参考《Mybatis学习笔记1》*）

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

配置数据源、配置 SqlSessionFactory、mapper 扫描器，ApplicationContext-dao.xml：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

    <!-- 加载配置文件 -->
    <context:property-placeholder location="classpath:db.properties" />
    <!-- 数据库连接池 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
          destroy-method="close">
        <property name="driverClassName" value="${jdbc.driver}" />
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <property name="maxActive" value="10" />
        <property name="maxIdle" value="5" />
    </bean>

    <!-- mapper配置 -->
    <!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 加载mybatis的全局配置文件 -->
        <property name="configLocation" value="classpath:SqlMapConfig.xml" />
    </bean>

    <!-- 配置Mapper扫描器 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.itheima.dao"/>
    </bean>

</beans>
```

db.properties：

``` xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/springmvc?characterEncoding=utf-8
jdbc.username=root
jdbc.password=root
```

ApplicationContext-service.xml 中添加：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
                           http://www.springframework.org/schema/tx 
                           http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

    <!-- @Service扫描 -->
    <context:component-scan base-package="cn.itheima.service"></context:component-scan>
</beans>
```

ApplicationContext-trans.xml 中添加（已省略了 Schema 约束）：

``` xml
<!-- 事务管理器 -->
<bean id="transactionManager"
      class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <!-- 数据源 -->
    <property name="dataSource" ref="dataSource" />
</bean>

<!-- 通知 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- 传播行为 -->
        <tx:method name="save*" propagation="REQUIRED" />
        <tx:method name="insert*" propagation="REQUIRED" />
        <tx:method name="delete*" propagation="REQUIRED" />
        <tx:method name="update*" propagation="REQUIRED" />
        <tx:method name="find*" propagation="SUPPORTS" read-only="true" />
        <tx:method name="get*" propagation="SUPPORTS" read-only="true" />
    </tx:attributes>
</tx:advice>

<!-- 切面 -->
<aop:config>
    <aop:advisor advice-ref="txAdvice"
                 pointcut="execution(* cn.itheima.service.*.*(..))" />
</aop:config>
```

SpringMvc.xml 中添加（已省略了 Schema 约束）：

``` xml
<!-- @Controller注解扫描 -->
<context:component-scan base-package="cn.itheima.controller"></context:component-scan>

<!-- 注解驱动:
      替我们显示的配置了最新版的注解的处理器映射器和处理器适配器 -->
<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>

<!-- 配置视图解析器 
 作用:在controller中指定页面路径的时候就不用写页面的完整路径名称了,可以直接写页面去掉扩展名的名称
 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <!-- 真正的页面路径 =  前缀 + 去掉后缀名的页面名称 + 后缀 -->
    <!-- 前缀 -->
    <property name="prefix" value="/WEB-INF/jsp/"></property>
    <!-- 后缀 -->
    <property name="suffix" value=".jsp"></property>
</bean>
```

WEB-INF/web.xml：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
    <display-name>ssm0523</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>default.html</welcome-file>
        <welcome-file>default.htm</welcome-file>
        <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>

    <!-- 加载spring容器 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:ApplicationContext-*.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- springmvc前端控制器 -->
    <servlet>
        <servlet-name>springMvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:SpringMvc.xml</param-value>
        </init-param>
        <!-- 在tomcat启动的时候就加载这个servlet -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMvc</servlet-name>
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>

</web-app>
```

Dao：使用 Mybatis 逆向工程生成：（用户表和商品表）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-4675780.jpg)

Service：① Service 由 Spring 管理；② Spring 对 Service 进行事务控制。

ItemsService.java 接口：

``` java
public interface ItemsService {
    public List<Items> list() throws Exception;
    public Items findItemsById(Integer id) throws Exception;
    public void updateItems(Items items) throws Exception;
}
```

ItemsServiceImpl.java 实现类：

``` java
@Service
public class ItemsServiceImpl implements ItemsService {

    @Autowired
    private ItemsMapper itemsMapper;

    @Override
    public List<Items> list() throws Exception {
        //如果不需要任何查询条件,直接将example对象new出来即可
        ItemsExample example = new ItemsExample();
        List<Items> list = itemsMapper.selectByExampleWithBLOBs(example);
        return list;
    }

    @Override
    public Items findItemsById(Integer id) throws Exception {
        Items items = itemsMapper.selectByPrimaryKey(id);
        return items;
    }

    @Override
    public void updateItems(Items items) throws Exception {
        itemsMapper.updateByPrimaryKeyWithBLOBs(items);
    }
}
```

ItemsController.java：

``` java
@Controller
public class ItemsController {
    /**
	 * springMvc中默认支持的参数类型:也就是说在controller方法中可以加入这些也可以不加,  加不加看自己需不需要,都行.
	 *HttpServletRequest
	 *HttpServletResponse
	 *HttpSession
	 *Model
	 */
    @RequestMapping("/itemEdit")
    public String itemEdit(HttpServletRequest reuqest, 
                           Model model) throws Exception{

        String idStr = reuqest.getParameter("id");
        Items items = itmesService.findItemsById(Integer.parseInt(idStr));

        //Model模型:模型中放入了返回给页面的数据
        //model底层其实就是用的request域来传递数据,但是对request域进行了扩展.
        model.addAttribute("item", items);

        //如果springMvc方法返回一个简单的string字符串,那么springMvc就会认为这个字符串就是页面的名称
        return "editItem";
    }
}
```

> 几点解释的：① 对于代码中最后一行 `return “editItem”;`可以这样写，是因为 SpringMvc.xml  中我们配置了如下视图解析器：
>
> ``` xml
> <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
>     <!-- 真正的页面路径 =  前缀 + 去掉后缀名的页面名称 + 后缀 -->
>     <!-- 前缀 -->
>     <property name="prefix" value="/WEB-INF/jsp/"></property>
>     <!-- 后缀 -->
>     <property name="suffix" value=".jsp"></property>
> </bean>
> ```
>
> 否则没配置的情况下，则改为`"return WEB-INF/jsp/editItem.jsp";`。
>
> ② QueryVo.java：
>
> ``` java
> public class QueryVo {
>     //商品对象
>     private Items items;
>     //订单对象...
>     //用户对象....
>
>     public Items getItems() {
>         return items;
>     }
>     public void setItems(Items items) {
>         this.items = items;
>     }
> }
> ```
>
> ③ 更多内容参考代码中注释。

最后测试：如查询所有商品，访问 `http://localhost:8080/ssm0523/itemList.action`

#### 4.1 小结

仔细回顾以上过程，捋一捋。先看 web.xml 配置文件：

``` xml
<!-- 加载spring容器 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:ApplicationContext-*.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<!-- springmvc前端控制器 -->
<servlet>
    <servlet-name>springMvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:SpringMvc.xml</param-value>
    </init-param>
    <!-- 在tomcat启动的时候就加载这个servlet -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springMvc</servlet-name>
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```

我们使用了 Spring 监听器把所有 classpath 下以 `ApplicationContext-`为开头的配置文件返还了进来，即下面三个：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-43193850.jpg)

另外，web.xml 文件中配置的前端控制器把 SpringMvc.xml 返回进来了。

大概这么个过程：当 Tomcat 启动后，首先加载 web.xml 文件，在 web.xml 中通过通配符形式把上面红框三个文件加载进来了，通过前端控制器把 SpringMvc.xml 文件也加载进来了，然后在 ApplicationContext-dao.xml 中把 db.properties 和 SqlMapConfig.xml 给加载进来了，并通过扫描 mapper 把 dao 下全都加载进来了。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-44539486.jpg)



## 五、参数绑定

### 5.1 绑定简单数据类型

需求：根据商品 id 查询商品信息

分析：

- 请求的 URL：.../itemEdit.action 
- 参数：id（商品的 id）
- 响应结果：商品编辑页面，展示商品详细信息。

相关代码如上节所示。

**Controller参数绑定：**

> 要根据 id 查询商品数据，需要从请求的参数中把请求的 id 取出来。id 应该包含在 Request 对象中。可以从Request 对象中取 id。
>
> ``` java
> /**
> 	 * springMvc中默认支持的参数类型有如下：(也就是说在controller方法中可以加入这些也可以不加,  加不加看自己需不需要,都行.)
> 	 *HttpServletRequest
> 	 *HttpServletResponse
> 	 *HttpSession
> 	 *Model
> 	 */
> @RequestMapping("/itemEdit")
> public String itemEdit(HttpServletRequest reuqest, 
>                        Model model) throws Exception{
>
>     String idStr = reuqest.getParameter("id");
>     Items items = itmesService.findItemsById(Integer.parseInt(idStr));
>
>     //Model模型:模型中放入了返回给页面的数据
>     //model底层其实就是用的request域来传递数据,但是对request域进行了扩展.
>     model.addAttribute("item", items);
>
>     //如果springMvc方法返回一个简单的string字符串,那么springMvc就会认为这个字符串就是页面的名称
>     return "editItem";
> }
> ```

当请求的参数名称和处理器形参**名称一致**时会将请求参数与形参进行绑定。从 Request 取参数的方法可以进一步简化。如下：

``` java
@RequestMapping("/itemEdit")
public String itemEdit(Integer id, Model model) {
    Items items = itemService.getItemById(id);
    //向jsp传递数据
    model.addAttribute("item", items);
    //设置跳转的jsp页面
    return"editItem";
}
```

#### 5.1.1 支持的数据类型：

``` xml
整形：Integer、int
字符串：String
单精度：Float、float
双精度：Double、double
布尔型：Boolean、boolean
    说明：对于布尔类型的参数，请求的参数值为true或false。
    处理器方法：
    public String editItem(Model model,Integer id,Boolean status) throws Exception
    请求url：
    http://localhost:8080/xxx.action?id=2&status=false
```

参数类型推荐使用包装数据类型，因为基础数据类型不可以为 null。

#### 5.1.2 @RequestParam 

使用 @RequestParam 常用于处理简单类型的绑定。

value：参数名字，即入参的请求参数名字，如`value=“item_id”`表示请求的参数区中的名字为 item_id 的参数的值将传入；

required：是否必须，默认是 true，表示请求中一定要有相应的参数，否则将报：

> TTP Status 400 - Required Integer parameter'XXXX' is not present

defaultValue：默认值，表示如果请求中没有同名参数时的默认值。

定义如下：

``` xml
public String editItem(@RequestParam(value="item_id",required=true) String id) {

}
```

形参名称为 id，但是这里使用`value="item_id"`限定请求的参数名为 item_id，所以页面传递参数的名必须为item_id。注意：如果请求参数中没有 item_id 将跑出异常：

> HTTP Status 500 - Required Integer parameter'item_id' is not present

 这里通过`required=true`限定 item_id 参数为必需传递，如果不传递则报400错误，可以使用 defaultvalue 设置默认值，即使`required=true`也可以不传 item_id 参数值。

### 5.2 绑定pojo类型 

需求：将页面修改后的商品信息保存到数据库中。

分析：

- 请求的 URL：.../updateitem.action
- 参数：表单中的数据
- 响应内容：更新成功页面

#### 5.2.1 使用 POJO 接收表单数据

如果提交的参数很多，或者提交的表单中的内容很多的时候可以使用 pojo 接收数据。要求 pojo 对象中的属性名和表单中 input 的 name 属性一致。 

页面定义如下：

``` jsp
<input type="text" name="name"/>
<input type="text" name="price"/>
```

pojo 的定义：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-26-91387643.jpg)

请求的参数名称和 pojo 的属性名称一致，会自动将请求参数赋值给 pojo 的属性。

``` java
//springMvc可以直接接收基本数据类型,包括string.spirngMvc可以帮你自动进行类型转换.
//controller方法接收的参数的变量名称必须要等于页面上input框的name属性值
//public String update(Integer id, String name, Float price, String detail) throws Exception{

//spirngMvc可以直接接收pojo类型:要求页面上input框的name属性名称必须等于pojo的属性名称
@RequestMapping("/updateitem")
public String update(Items items) throws Exception{
    itmesService.updateItems(items);

    return "success";
}
```

> 注意：提交的表单中不要有日期类型的数据，否则会报400错误。如果想提交日期类型的数据需要用到后面的自定义参数绑定的内容。

#### 5.2.2 解决post乱码问题

在 web.xml 中加入：

``` xml
<!-- 配置Post请求乱码 -->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

以上可以解决 post 请求乱码问题。

对于 get 请求中文参数出现乱码解决方法有两个：

1. 修改 tomcat 配置文件添加编码与工程编码一致，如下：

   ``` xml
   <Connector URIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
   ```

2. 另外一种方法对参数进行重新编码：

   ``` java
   String userName new 
   String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")
   ```

   ISO8859-1 是 tomcat 默认编码，需要将 tomcat 编码后的内容按 utf-8 编码。



### 5.3 绑定包装pojo

需求：使用包装的 pojo 接收商品信息的查询条件。

需求分析，包装对象定义如下 QueryVo：

``` java
public class QueryVo {
	private Items items;
    ......略
}
```

页面定义：

``` jsp
<input type="text" name="items.name" />
<input type="text" name="items.price" />
```

接收查询条件：

``` java
//如果Controller中接收的是Vo,那么页面上input框的name属性值要等于vo的属性.属性.属性.....
@RequestMapping("/search")
public String search(QueryVo vo) throws Exception{
    System.out.println(vo);
    return "";
}
```

### 5.4 自定义参数绑定

需求：在商品修改页面可以修改商品的生产日期，并且根据业务需求自定义日期格式。

需求分析：

> 由于日期数据有很多种格式，所以 SpringMVC  没办法把字符串转换成日期类型。所以需要自定义参数绑定。前端控制器接收到请求后，找到注解形式的处理器适配器，对 RequestMapping 标记的方法进行适配，并对方法中的形参进行参数绑定。在 SpringMVC 这可以在处理器适配器上自定义 Converter 进行参数绑定。如果使用`<mvc:annotation-driven/>`可以在此标签上进行扩展。

自定义 Converter， CustomGlobalStrToDateConverter.java：

``` java
/**
 * S - source:源
 * T - target:目标
 * @author zj
 *
 */
public class CustomGlobalStrToDateConverter implements Converter<String, Date> {

    @Override
    public Date convert(String source) {
        try {
            Date date = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss").parse(source);
            return date;
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

配置 Converter，在 SpringMVC 核心配置文件 SpringMvc.xml 中添加：

``` xml
<!-- 注解驱动 -->
<mvc:annotation-drivenconversion-service="conversionService"/> 
<!-- 配置自定义转换器 
	 注意: 一定要将自定义的转换器配置到注解驱动上
 -->
<bean id="conversionService"
      class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <!-- 指定自定义转换器的全路径名称 -->
            <bean class="cn.itheima.controller.converter.CustomGlobalStrToDateConverter"/>
        </set>
    </property>
</bean>
```

### 5.5 小结

参数绑定（从请求中接收参数）

（1）默认类型：

- 在 controller 方法中可以有也可以没有，看自己需求随意添加
- httpservletRqeust、httpServletResponse、httpSession、Model（ModelMap其实就是Mode的一个子类，一般用的不多）

（2）基本类型：String、double、float、integer、long、boolean

（3）pojo类型：页面上 input 框的 name 属性值必须要等于 pojo 的属性名称

（4）vo类型：页面上 input 框的 name 属性值必须要等于 vo 中的属性.属性.属性....

（5）自定义转换器converter：

- 作用：由于 SpringMMV 无法将 String 自动转换成 Date 所以需要自己手动编写类型转换器
- 需要编写一个类实现 Converter 接口
- 在 SpringMvc.xml 核心配置文件中配置自定义转换器
- 在 SpringMvc.xml 中将自定义转换器配置到注解驱动上



## 六、SpringMVC和Struts2的区别

1、SpringMVC 的入口是一个 servlet 即前端控制器，而 Struts2 入口是一个 filter 过虑器。

2、SpringMVC 是基于方法开发（一个 url 对应一个方法），请求参数传递到方法的形参，可以设计为单例或多例（建议单例），Struts2 是基于类开发，传递参数是通过类的属性，只能设计为多例。

3、  Struts 采用值栈存储请求和响应的数据，通过 OGNL 存取数据， SpringMVC 通过参数解析器是将 request 请求内容解析，并给方法形参赋值，将数据和视图封装成 ModelAndView 对象，最后又将 ModelAndView 中的模型数据通过 reques 域传输到页面。Jsp 视图解析器默认使用 jstl。
