## 前言

**Spring框架的学习路线：**

``` xml
1. Spring第一天：Spring的IOC容器之XML的方式，Spring框架与Web项目整合
2. Spring第二天：Spring的IOC容器之注解的方式，Spring的AOP技术
3. Spring第三天：Spring的事务管理、Spring框架的JDBC模板
4. Spring第四天：SSH三大框架的整合
```

这是第一天学习大纲：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-98727205.jpg)



## 一、 Spring框架的概述

### 1.1 技术分析之什么是Spring框架

1、Spring框架的概述：（更详细参考百度百科 [spring](https://baike.baidu.com/item/Spring/85061)）

> - Spring是一个开源框架
> - Spring是于2003 年兴起的一个轻量级的Java开发框架，由Rod Johnson在其著作Expert One-On-One J2EE Development and Design中阐述的部分理念和原型衍生而来。
> - 它是为了解决企业应用开发的复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许使用者选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。
> 	 Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以	从Spring中受益。
> - Spring的核心是**控制反转**（IoC）和**面向切面**（AOP）。简单来说，Spring是一个分层的JavaSE/EEfull-stack(一站式) 轻量级开源框架。

引用百度百科上一段介绍：

> Rod Johnson在2002年编著的《Expert one on one J2EE design and development》一书中，对Java EE 系统框架臃肿、低效、脱离现实的种种现状提出了质疑，并积极寻求探索革新之道。以此书为指导思想，他编写了interface21框架，这是一个力图冲破J2EE传统开发的困境，从实际需求出发，着眼于轻便、灵巧，易于开发、测试和部署的轻量级开发框架。Spring框架即以interface21框架为基础，经过重新设计，并不断丰富其内涵，于2004年3月24日，发布了1.0正式版。同年他又推出了一部堪称经典的力作《Expert one-on-one J2EE Development without EJB》，该书在Java世界掀起了轩然大波，不断改变着Java开发者程序设计和开发的思考方式。在该书中，作者根据自己多年丰富的实践经验，对EJB的各种笨重臃肿的结构进行了逐一的分析和否定，并分别以简洁实用的方式替换之。至此一战功成，Rod Johnson成为一个改变Java世界的大师级人物。

解释：最早做 Java 开发采用的是 sun 公司提供的 EJB 规范做开发，最开始没那么多框架。Rod Johnson 对开发过程诸如低效、臃肿、脱离现实种种现状提出质疑，并积极寻求探索革新之道，后来就有了 spring 框架。

2、EE开发分成三层结构

- WEB层		-- Spring MVC
	 业务层	        -- Bean管理：(IOC)
	 持久层	        -- Spring的 JDBC 模板. ORM模板用于整合其他的持久层框架

3、什么叫full-stack（一站式）开发？

首先来说 Java EE 开发一般分三层，WEB 层、业务层、持久层，每一层要干的事不一样。如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-13709643.jpg)

Spring 属于业务层一个框架，那为什么叫一站式开发呢？这样来解释，如果 WEB 层你不想使用 Struts2 框架，Spring 框架提供了一个叫 SpringMVC 模块（属于 Spring 的一部分），它就可以当做 WEB 层小框架，处理 web 层事情；如果持久层不想使用 Hibernate 框架，Spring 框架提供了一个叫 SpringJDBC 模板（它能帮忙操作数据库），这样它就能当做持久层解决方案。

这样看来，如果我不使用其他框架，只用 Spring 框架也是能做 Java EE 开发。并且如果你不想使用 SpringMVC 模块做 WEB 层也是可以的，仍然可以使用 Struts2 框架，因为 Spring 框架可以帮忙集成和整合 Struts2 框架；如果持久层你不想使用 SpringJDBC 模板，使用其他一些持久层框架如 Hibernate、Mybatis 也是可以，Spring 框架也可以去整合它们。

另外，如果没有 Spring 框架，只用 Struts2 开发也是没问题，但可能效率等方面就不是很好了。但有了 Spring 框架，Struts2 会更牛，它的管理可能更方便、开发更简单。简单来说：只用 Struts2 去开发也可以，但是有了 Spring 框架开发会更好。

再次小结：如果没有 Spring 框架也能做企业级开发，但有了 Spring，做开发更规范，程序扩展性和维护等方面更好。-----> 它能解决企业级开发的复杂性。

另外关于 Spring 框架，我们再来了解下它与 Spring boot、Spring Clound 的区别：[Spring SpringMVC SpringBoot SpringCloud概念、关系及区别](https://blog.csdn.net/xufei512/article/details/79710606)

### 1.2 技术分析之Spring框架的特点

1、 为什么要学习Spring的框架

> - 方便解耦，简化开发
>   - Spring就是一个大工厂，可以将所有对象创建和依赖关系维护，交给Spring管理
> - AOP编程的支持
>   - Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
> - 声明式事务的支持
>   - 只需要通过配置就可以完成对事务的管理，而无需手动编程
> - 方便程序的测试
>   - Spring对Junit4支持，可以通过注解方便的测试Spring程序
> - 方便集成各种优秀框架
>   - Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts2、Hibernate、MyBatis、Quartz等）的直接支持
> - 降低JavaEE API的使用难度
>   - Spring 对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低

2、Spring框架的版本：

- Sring3.x 和 Spring4.x 的版本



## 二、SpringIOC的快速入门

1、什么是 IOC 的功能？

- IoC（Inverse of Control）：控制反转，将对象的创建权反转给Spring！！
- 使用 IOC 可以解决的程序耦合性高的问题！！

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-66129380.jpg)

如上图，解释一下。

假如要做一个保存客户信息的功能，那么我们在代码中可能要在 service 层 new 一个类，再到 dao 层 new 一个类（类比上图即资源），可以看到，资源的创建权利来自该功能。其实这样不是不可以，只是不好，过度耦合了。有了 Spring 框架，那现在可以交给 Spring 框架了。

IOC 的编写过程：（先要了解工厂模式，参考 [工厂模式——看这一篇就够了](https://juejin.im/entry/58f5e080b123db2fa2b3c4c6)）

- App ---> 工厂 ---> 资源（功能和资源分开了，但是有个问题，资源和工厂还存在耦合）

- App ---> 工厂 ---> XML文件 ---> 资源

  > 在 xml 文件中配置，配置什么，就创建什么。工厂读取 xml 文件，工厂负责生产这些对象。程序中用到对象就到工厂拿就可以。

IOC 的底层实现原理：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-15572215.jpg)

编写步骤：

```
1. 步骤一：下载Spring框架的开发包
    * 官网：http://spring.io/
    * 下载地址：http://repo.springsource.org/libs-release-local/org/springframework/spring 解压出来:(Spring目录结构:)
        * docs      -- API和开发规范
        * libs      -- jar包和源码
        * schema    -- 约束

2. 步骤二：创建JavaWEB项目，引入Spring的开发包
    * 引入Spring框架IOC核心功能需要的具体的jar包
        * Spring框架的IOC的功能，那么根据Spring框架的体系结构图能看到，只需要引入如下的jar包
            * Beans
            * Core
            * Context
            * Expression Language

        * Spring框架也需要引入日志相关的jar包
            * 在spring-framework-3.0.2.RELEASE-dependencies/org.apache.commons/com.springsource.org.apache.commons.logging/1.1.1
                * com.springsource.org.apache.commons.logging-1.1.1.jar

            * 还需要引入log4j的jar包 spring-framework-3.0.2.RELEASE-dependencies\org.apache.log4j\com.springsource.org.apache.log4j\1.2.15
                * com.springsource.org.apache.log4j-1.2.15.jar

3. 步骤三：创建对应的包结构，编写Java的类，要注意：以后使用Spring框架做开发，都需要来编写接口与实现类！！
    * com.itcast.demo1
        * UserService           -- 接口
        * UserServiceImpl       -- 具体的实现类

4. 步骤四：想把UserServiceImpl实现类的创建交给Spring框架来管理，需要创建Spring框架的配置文件，完成配置
    * 在src目录下创建applicationContext.xml的配置文件，名称是可以任意的，但是一般都会使用默认名称！！

    * 引入spring的约束，需要先找到具体的约束头信息！！
        * spring-framework-3.2.0.RELEASE\docs\spring-framework-reference\html\xsd-configuration.html
        * 具体的约束如下：      
            <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="
                    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
            </beans>

    * 完成UserService的配置
        <!-- Spring的快速入门 -->
        <bean id="userService" class="com.itcast.demo1.UserServiceImpl"/>

5. 步骤五：编写测试程序，采用Spring框架的工厂方式来获取到UserService接口的具体实现类！！
    public void demo2(){
        // 使用Spring的工厂:
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 通过工厂获得类:
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.sayHello();
    }
```

**关于第①步：** 相关 Spring 文件有如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-51842600.jpg)

但我们用到的核心的是 `spring-framework-4.2.4.RELEASE-dist.zip` 该文件，把其解压出来可以看到存在 docs文件夹(文档) 和 scheme文件夹(约束)；另外一个文件 `spring-framework-3.0.2.RELEASE-dependencies.zip`是有关依赖包的文件，如文件上传、连接池、日志相关包。

**关于第②步：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-5727767.jpg)

看上图上半部分 Spring Framework Runtime，可以看到核心容器下有四个模块，上面其他功能都依赖该四个模块。所以只要导入四个包就可以。（在这里，另外导入了`spring-framework-3.0.2.RELEASE-dependencies.zip`中两个依赖包，第一个为日志规范，第二个为日志实现）

关于日志，其实还少了一个配置文件 `log4j.properties`，内容：

``` xml
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=c\:mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=info, stdout
```

把日志配置文件粘贴到项目 src 目录下。

注：`log4j.rootLogger=info, stdout` 中 info 表示级别，stdout 表示向控制台输出。如 info 可改为 off 则不会在输出日志信息，还可设置为 debug。

代码演示：

``` java
public class Demo1 {
    //创建日志对象
    private Logger log = Logger.getLogger(Demo1.class);

    @Test
    public void run1() {
        //		System.out.println("执行了...");
        log.info("执行了...");
    }
}
```

结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-41538827.jpg)

**关于第③步：**

UserService.java：

``` java
public interface UserService {
	public void sayHello();
}
```

UserServiceImpl.java：

``` java
public class UserServiceImpl implements UserService {
	public void sayHello() {
		System.out.println("Hello Spring!");
	}
}
```

使用原来的方式：

``` java
//原来的方式
@Test
public void run1() {
    //创建实现类
    //UserServiceImpl us = new UserServiceImpl(); 
    UserService us = new UserServiceImpl();
    us.sayHello();
}
```

可以看到打印输出：Hello Spring!

**关于第④⑤步：** 在 src 目录下创建`applicationContext.xml`的配置文件，写上相关配置信息

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
   	<!-- 使用bean标签 -->
   	<bean id="userService" class="com.strivebo.demo2.UserServiceImpl">
   		<property name="name" value="小凤"/>
   	</bean>
</beans>
```

使用 Spring 框架的方式：

``` java
//使用 Spring 框架的方式
@Test
public void run2() {
    // 创建工厂，加载核心配置文件
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    // 从工厂中获取到对象
    UserServiceImpl usi = (UserServiceImpl) ac.getBean("userService");
    //一般使用如下接口的方式
    //UserService usi = (UserService) ac.getBean("userService");
    // 调用对象的方法执行
    usi.sayHello();
}
```

同样也是能输出：Hello Spring!

### 1.3 入门总结之Spring框架中的工厂（了解）

```
1. ApplicationContext接口
    * 使用ApplicationContext工厂的接口，使用该接口可以获取到具体的Bean对象
    * 该接口下有两个具体的实现类
        * ClassPathXmlApplicationContext            -- 加载类路径下的Spring配置文件
        * FileSystemXmlApplicationContext           -- 加载本地磁盘下的Spring配置文件

2. BeanFactory工厂（是Spring框架早期的创建Bean对象的工厂接口）
    * 使用BeanFactory接口也可以获取到Bean对象
        public void run(){
            BeanFactory factory = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));
            UserService us = (UserService) factory.getBean("us");
            us.sayHello();
        }

    * BeanFactory和ApplicationContext的区别
        * BeanFactory               -- BeanFactory采取延迟加载，第一次getBean时才会初始化Bean
        * ApplicationContext        -- 在加载applicationContext.xml时候就会创建具体的Bean对象的实例，还提供了一些其他的功能
            * 事件传递
            * Bean自动装配
            * 各种不同应用层的Context实现
```



## 三、IoC容器XML的方式

### 3.1 入门总结之配置Spring框架编写XML的提示：

```
1. 步骤一：先复制， http://www.springframework.org/schema/beans/spring-beans.xsd    
2. 步骤二：打开 Eclipse的Windows-->Preference，搜索XML Catalog，点击Add按钮
3. 步骤三：先选择Location的schema的约束地址
    * E:\class\2016\JavaEE28\day35_Spring框架第一天\资料\spring-framework-4.2.4.RELEASE-schema\beans\spring-beans-4.2.xsd
4. 步骤四：注意：Key type要选择：Schema location
5. 步骤五：Key把http://www.springframework.org/schema/beans/spring-beans.xsd复制上
```

### 3.2 技术分析之Spring框架的Bean管理的配置文件方式

#### 3.3.1 Spring框架中标签的配置

```xml
1. id属性和name属性的区别
    * id        -- Bean起个名字，在约束中采用ID的约束，唯一
        * 取值要求：必须以字母开始，可以使用字母、数字、连字符、下划线、句话、冒号  id:不能出现特殊字符

    * name      -- Bean起个名字，没有采用ID的约束（了解）
        * 取值要求：name:出现特殊字符.如果<bean>没有id的话 , name可以当做id使用
        * Spring框架在整合Struts1的框架的时候，Struts1的框架的访问路径是以/开头的，例如：/bookAction

2. class属性          -- Bean对象的全路径
3. scope属性          -- scope属性代表Bean的作用范围
    * singleton         -- 单例（默认值）
    * prototype         -- 多例，在Spring框架整合Struts2框架的时候，Action类也需要交给Spring做管理，配置把Action类配置成多例！！
    * request           -- 应用在Web项目中,每次HTTP请求都会创建一个新的Bean
    * session           -- 应用在Web项目中,同一个HTTP Session 共享一个Bean
    * globalsession     -- 应用在Web项目中,多服务器间的session

4. Bean对象的创建和销毁的两个属性配置（了解）
    * 说明：Spring初始化bean或销毁bean时，有时需要作一些处理工作，因此spring可以在创建和拆卸bean的时候调用bean的两个生命周期方法
    * init-method       -- 当bean被载入到容器的时候调用init-method属性指定的方法
    * destroy-method    -- 当bean从容器中删除的时候调用destroy-method属性指定的方法
        * 想查看destroy-method的效果，有如下条件
            * scope= singleton有效
            * web容器中会自动调用，但是main函数或测试用例需要手动调用（需要使用ClassPathXmlApplicationContext的close()方法）
```

关于 scope 属性 为 singleton（单例）和 prototype（多例）的理解：

> 像以前的写程序方式，每次要在 service 得 `new XxxDao()`，可以看出，每次发送请求，都得 new 一个对象，这就是多例。（每个请求都有一个实例对象）
>
> 那什么是单例呢？单例相当于整个运行环境中就这一个实例对象。就比如 `new XxxDao()` 这个 dao 对象已经创建好了，在内存当中就这一个实例对象。 
>
> 关于 Spring的 bean 的单例和多例作用域更多介绍参考网上资料：[【Spring学习17】bean作用域：单例和多例](https://blog.csdn.net/soonfly/article/details/69359772)
>
> > 在 Spring 里，通过容器创建的对象默认是singleton单例（这里要注意的是 singleton 作用域和 GOF 设计模式中的单例是不同的） ，单例就是在整个容器的生命周期，只会存在一个共享的bean实例。 
> >
> > 如果某个 bean 被标记为多例，则每次请求使用该对象时，都会创建一个新的 bean 实例。比如将这个 bean 注入到另一个 bean 中，或者在程序中调用`getBean("beanid")`方法，都会触发生成一个新的 bean 实例，相当于 new 的操作。 

关于 init-method 和 destory-method 属性，见代码演示：

``` xml
<!-- 使用bean标签 -->
<bean id="userService" class="com.strivebo.demo2.UserServiceImpl" init-method="init" destroy-method="destory">
    <property name="name" value="小凤"/>
</bean>
```

UserServiceImpl.java：

``` java
public class UserServiceImpl implements UserService {
	public void sayHello() {
		System.out.println("Hello Spring!");
	}	
	public void init() {
		System.out.println("初始化...");
	}
	public void destory() {
		System.out.println("销毁...");
	}
}
```

#### 3.3.2 依赖注入（DI）

什么是依赖注入？

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-11797687.jpg)

如上图，我们想做一个功能，以前都是 service 层  `new Dao().xxx` ，可以看到 service 层需要依赖 dao，这就叫依赖。现在有了 Spring ，service 和 dao 都可以交给 Spring 管理。在创建 service 的时候，发现需要 dao，则会注入一个 dao，那 service 就这样有了 dao，于是便可以调用 dao 对象的方法。代码来理解：

UserServiceImpl.java：

``` java
public class UserServiceImpl implements UserService {
    private String name;
    public void setName(String name) {
        this.name = name;
    }

    public void sayHello() {
        System.out.println("Hello Spring!" + name);
    }
}
```

**以前方法都是：**

``` java
public void run1() {
    //创建实现类
    UserServiceImpl us = new UserServiceImpl();
    us.setName("小明");
    us.sayHello();
}
```

可以看到输出：Hello Spring!小明

**现在方式——依赖注入方式：**

applicationContext.xml：

``` xml
<bean id="userService" class="com.strivebo.demo2.UserServiceImpl">
    <property name="name" value="小凤"/>
</bean>
```

执行：

``` java
@Test
public void run2() {
    // 创建工厂，加载核心配置文件
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    UserService usi = (UserService) ac.getBean("userService");
    // 调用对象的方法执行
    usi.sayHello();
}
```

输出结果：Hello Spring!小凤

-------------------------------------------代码完整演示过程---------------------------------------------

①以前的方式：

CustomerDaoImpl.java：

``` java
public class CustomerDaoImpl {
    public void save(){
        System.out.println("我是持久层dao....");
    }
}
```

CustomerServiceImpl.java：

``` java
public class CustomerServiceImpl {
    public void save(){
        System.out.println("我是业务层service....");
        new CustomerDaoImpl().save();
    }	
}
```

执行：

``` java
@Test
public void run1(){	//原始方式
    CustomerServiceImpl cs = new CustomerServiceImpl();
    cs.save();
}
```

结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-20-96451229.jpg)



②依赖注入方式：

CustomerServiceImpl.java：

```java
public class CustomerServiceImpl {
    // 提供成员属性，提供set方法
    private CustomerDaoImpl customerDao;
    public void setCustomerDao(CustomerDaoImpl customerDao) {
        this.customerDao = customerDao;
    }	
    public void save(){
        System.out.println("我是业务层service....");
        // 原来编写方式
        // new CustomerDaoImpl().save();
        // Spring的方式
        customerDao.save();
    }
}
```

applicationContext.xml 文件添加如下：（即把 customeDao 注入到 customerService 中）

``` xml
<!-- 演示的依赖注入 -->
<bean id="customerDao" class="com.strivebo.demo3.CustomerDaoImpl"/>
<bean id="customerService" class="com.strivebo.demo3.CustomerServiceImpl">
    <property name="customerDao" ref="customerDao"/>
</bean> 	
```

运行：

``` java
@Test
public void run2(){	//Spring 方式
    // 创建工厂，加载配置文件，CustomerDaoImpl创建了，CustomerServiceImpl被创建了，
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    CustomerServiceImpl cs = (CustomerServiceImpl) ac.getBean("customerService");
    cs.save();
}
```

#### 3.3.3 Spring框架的属性注入

``` xml
1. 对于类成员变量，常用的注入方式有两种
    * 构造函数注入
    * 属性setter方法注入

2. 在Spring框架中提供了前两种的属性注入的方式
    1. 构造方法的注入方式，两步
        * 编写Java的类，提供构造方法
            public class Car {
                private String name;
                private double money;
                public Car(String name, double money) {
                    this.name = name;
                    this.money = money;
                }
                @Override
                public String toString() {
                    return "Car [name=" + name + ", money=" + money + "]";
                }
            }

        * 编写配置文件
            <bean id="car" class="com.itheima.demo4.Car">
                <constructor-arg name="name" value="大奔"/>
                <constructor-arg name="money" value="100"/>
            </bean>
		
		* 或者
            <bean id="car" class="com.itheima.demo4.Car">
                <constructor-arg index="0" value="长安奔奔"/>
                <constructor-arg index="1" value="45000"/>
            </bean>

    2. 属性的setter方法的注入方式
        * 编写Java的类，提供属性和对应的set方法即可
        * 编写配置文件

    3. 如果Java类的属性是另一个Java的类，那么需要怎么来注入值呢？
        * <property name="name" rel="具体的Bean的ID或者name的值"/>
        * 例如：
            <bean id="person" class="com.itheima.demo4.Person">
                <property name="pname" value="美美"/>
                <property name="car2" ref="car2"/>
            </bean>
```

#### 3.3.4 Spring的2.5版本中提供了一种:p名称空间的注入（了解）

```
1. 步骤一：需要先引入 p 名称空间
    * 在schema的名称空间中加入该行：xmlns:p="http://www.springframework.org/schema/p"

2. 步骤二：使用p名称空间的语法
    * p:属性名 = ""
    * p:属性名-ref = ""

3. 步骤三：测试
    * <bean id="person" class="com.itheima.demo4.Person" p:pname="老王" p:car2-ref="car2"/>
```

#### 3.3.5 Spring的3.0提供了一种:SpEL注入方式（了解）

```
1. SpEL：Spring Expression Language是Spring的表达式语言，有一些自己的语法
2. 语法
    * #{SpEL}

3. 例如如下的代码
    <!-- SpEL的方式 -->
    <bean id="person" class="com.itheima.demo4.Person">
        <property name="pname" value="#{'小风'}"/>
        <property name="car2" value="#{car2}"/>
    </bean>

4. 还支持调用类中的属性或者方法
    * 定义类和方法，例如
        public class CarInfo {
            public String getCarname(){
                return "奇瑞QQ";
            }
        }
```

#### 3.3.6 数组，集合(List,Set,Map)，Properties等的注入

```
1. 如果是数组或者List集合，注入配置文件的方式是一样的
    <bean id="collectionBean" class="com.itheima.demo5.CollectionBean">
        <property name="arrs">
            <list>
                <value>美美</value>
                <value>小风</value>
            </list>
        </property>
    </bean>

2. 如果是Set集合，注入的配置文件方式如下：
    <property name="sets">
        <set>
            <value>哈哈</value>
            <value>呵呵</value>
        </set>
    </property>

3. 如果是Map集合，注入的配置方式如下：
    <property name="map">
        <map>
            <entry key="老王2" value="38"/>
            <entry key="凤姐" value="38"/>
            <entry key="如花" value="29"/>
        </map>
    </property>

4. 如果是properties属性文件的方式，注入的配置如下：
    <property name="pro">
        <props>
            <prop key="uname">root</prop>
            <prop key="pass">123</prop>
        </props>
    </property>
```

#### 3.3.7 Spring框架的配置文件分开管理（了解）

```
1. 例如：在src的目录下又多创建了一个配置文件，现在是两个核心的配置文件，那么加载这两个配置文件的方式有两种！
    * 主配置文件中包含其他的配置文件:
        <import resource="applicationContext2.xml"/>

    * 工厂创建的时候直接加载多个配置文件:
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(
                    "applicationContext.xml","applicationContext2.xml");
```

-------------------------------------------- 来看一个属性注入比较全的配置文件 ----------------------------------------------

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd">
	
   	<!-- 使用bean标签 -->
   	<bean id="userService" class="com.itheima.demo2.UserServiceImpl">
   		<property name="name" value="小凤"/>
   	</bean>
   	
   	<!-- 演示的依赖注入 -->
   	<bean id="customerDao" class="com.itheima.demo3.CustomerDaoImpl"/>
   	<bean id="customerService" class="com.itheima.demo3.CustomerServiceImpl">
   		<property name="customerDao" ref="customerDao"/>
   	</bean>
   	
   	<!-- 演示的构造方法的注入的方式 -->
   	<bean id="car1" class="com.itheima.demo4.Car1">
   		<!-- <constructor-arg name="cname" value="奇瑞QQ"/>
   		<constructor-arg name="price" value="25000"/> -->
        
   		<constructor-arg index="0" value="囚车"/>
   		<constructor-arg index="1" value="545000"/>
   	</bean>
    
   	<!-- 演示的依赖注入 -->
   	<bean id="person" class="com.itheima.demo4.Person">
   		<constructor-arg name="pname" value="美美"/>
   		<constructor-arg name="car1" ref="car1"/>
   	</bean>
   	
   	<!-- 采用set方法注入
   	<bean id="car2" class="com.itheima.demo4.Car2">
   		<property name="cname" value="二八自行车"/>
   		<property name="price" value="1000"/>
   	</bean> -->
   	
   	<!-- 采用p名称空间注入的方式（了解） 
   		<bean id="car2" class="com.itheima.demo4.Car2" p:cname="保时捷" p:price="1000000"/>
	-->
	
	<!-- 使用SPEL方式注入 -->
	<bean id="car2" class="com.itheima.demo4.Car2">
		<property name="cname" value="#{'福特野马'}"/>
		<property name="price" value="#{450000}"/>
	</bean>
	
	<!-- 注入集合 
	<bean id="user" class="com.itheima.demo4.User">
		<property name="arrs">
			<list>
				<value>哈哈</value>
				<value>呵呵</value>
				<value>嘿嘿</value>
			</list>
		</property>
		
		<property name="list">
			<list>
				<value>美美</value>
				<value>小凤</value>
			</list>
		</property>
		
		<property name="map">
			<map>
				<entry key="aaa" value="小苍"/>
				<entry key="bbb" value="小泽"/>
			</map>
		</property>
		
		<property name="pro">
			<props>
				<prop key="username">root</prop>
				<prop key="password">1234</prop>
			</props>
		</property>
	</bean>
	-->
	
	<!-- 引入其他的配置文件 
		<import resource="applicationContext2.xml"/>
	-->
</beans>
```



## 四、在web项目中集成Spring

CustomerDao.java：

``` java
public interface CustomerDao {
	public void save();
}
```

CustomerDaoImpl.java：

``` java
public class CustomerDaoImpl implements CustomerDao {
	
	public void save() {
		System.out.println("持久层：保存客户...");
	}
}
```

CustomerService.java：

``` java
public interface CustomerService {
	public void save();	
}
```

CustomerServiceImpl.java：

``` java
public class CustomerServiceImpl implements CustomerService {
	private CustomerDao customerDao;
	public void setCustomerDao(CustomerDao customerDao) {
		this.customerDao = customerDao;
	}
    
	public void save() {
		System.out.println("业务层：保存客户...");
		customerDao.save();
	}
}
```

配置文件 applicationContext.xml 添加如下：

``` xml
<!-- 配置客户的业务层 -->
<bean id="customerService" class="com.itheima.service.CustomerServiceImpl">
    <property name="customerDao" ref="customerDao"/>
</bean>

<!-- 配置持久层 -->
<bean id="customerDao" class="com.itheima.dao.CustomerDaoImpl"/>
```



CustomerAction.java：

``` java
/**
 * 客户的Action
 * @author Administrator
 */
public class CustomerAction extends ActionSupport{
	
	private static final long serialVersionUID = 113695314694166436L;
	
	/**
	 * 保存客户
	 * @return
	 */
	public String save(){
		System.out.println("WEB层：保存客户...");
		// 使用工厂
		/*ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		CustomerService cs = (CustomerService) ac.getBean("customerService");
		cs.save();*/
		
		ServletContext servletContext = ServletActionContext.getServletContext();
		// 需要使用WEB的工厂的方式
		WebApplicationContext context = WebApplicationContextUtils.getWebApplicationContext(servletContext);
		CustomerService cs = (CustomerService) context.getBean("customerService");
		cs.save();
		
		return NONE;
	}
}
```

页面每次点击保存客户，则会调用 Action 层（WEB层） save() 方法，再调用 Service 层（业务层），最后调用 Dao 层（持久层）。

但这里会出现一个问题，每访问一次都会加载一次配置文件（即每次请求都会创建一个工厂类，服务器端的资源就浪费了，一般情况下一个工程只有一个Spring的工厂类就OK了）。需要解决该问题，该怎么解决呢？整合来了。

第一步：找到`spring-framework-4.2.4.RELEASE\libs\spring-web-4.2.4.RELEASE.jar`复制到 `WEB-INF\lib` 目录（即导入该 jar 包）；

第二步：得先温故 Servlet 监听器有关知识了，如图

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-20-94330440.jpg)

> - 有这么一类监听器，监听 ServletContext 对象的创建和销毁的。
>
>
> - ServletContext 对象什么时候创建？当服务器启动时候创建，当服务器销毁时候关闭。



当服务器启动， ServletContext 对象创建，则监听器对象的方法执行，加载 `applicationContext.xml` 文件，而且只会加载一次，并且里面的那些对象已经创建好了。

第三步：配置监听器（在 `web.xml` 添加如下）

``` xml
<!-- 配置WEB整合的监听器 -->
<listener>
    <!-- 服务器一启动，监听器这个类的方法就会执行，可以关联查看源码 --> 
    <!-- 配置该监听器是固定的，只需要每次粘贴过来就行。-->
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!-- 加载方式：该监听器默认加载WEB-INF目录下的配置文件。-->
<!-- 我们提供配置方式，加载其他目录下配置文件，如src目录下的配置文件 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

第四步：编写 Servlet

``` java
public String save(){
    System.out.println("WEB层：保存客户...");
    ServletContext servletContext = ServletActionContext.getServletContext();
    // 需要使用WEB的工厂的方式
    WebApplicationContext context = WebApplicationContextUtils.getWebApplicationContext(servletContext);
    CustomerService cs = (CustomerService) context.getBean("customerService");
    cs.save();
    return NONE;
}
```



**步骤小结：Spring框架整合WEB（不是最终的方案）**

```
1. 创建JavaWEB项目，引入Spring的开发包。编写具体的类和方法。
    * 环境搭建好后，启动服务器来测试项目，发送每访问一次都会加载一次配置文件，这样效率会非常非常慢！！

2. 解决上面的问题
    * 将工厂创建好了以后放入到ServletContext域中.使用工厂的时候,从ServletContext中获得.
        * ServletContextListener:用来监听ServletContext对象的创建和销毁的监听器.
        * 当ServletContext对象创建的时候:创建工厂 , 将工厂存入到ServletContext

3. Spring整合Web项目
    * 引入spring-web-4.2.4.RELEASE.jar包
    * 配置监听器
         <!-- 配置Spring的核心监听器: -->
         <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
         </listener>
         <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
         </context-param>

4. 修改servlet的代码
    * 从ServletContext中获得工厂
    * 具体代码如下
        ServletContext servletContext = ServletActionContext.getServletContext();
        // 需要使用WEB的工厂的方式
        WebApplicationContext context = WebApplicationContextUtils.getWebApplicationContext(servletContext);
        CustomerService cs = (CustomerService) context.getBean("customerService");
        cs.save();  
```


