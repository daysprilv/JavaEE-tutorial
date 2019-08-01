
## 前言

**Spring框架的学习路线：**

``` xml
1. Spring第一天：Spring的IOC容器之XML的方式，Spring框架与Web项目整合
2. Spring第二天：Spring的IOC容器之注解的方式，Spring的AOP技术
3. Spring第三天：Spring的事务管理、Spring框架的JDBC模板
4. Spring第四天：SSH三大框架的整合
```

这是第二天学习大纲：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-96310633.jpg)



## 一、Spring框架的IOC基于注解的方式

> 传统的 SSH（Structs2、Spring、Hibernate）架构的项目基本都是使用的配置文件方式，很少用注解的方式，但其实 Struts2 有注解，Hibernate 也有注解。如果是 SSM（SpringMVC、Spring、MyBatis）这套架构的项目有可能使用到注解方式，所以重点会在这套架构学习中讲解注解方式，但 SSH 该架构也是用到了 Spring，所以还是会先讲解注解方式。

先想一想注解出现的目的是干嘛呢？其实注解最主要一个目的就是要替代传统 XML 文件方式，用注解会变得更简单，因为传统 XML 文件需要写很多很多配置，如果多了会变得很臃肿。

### 1.1 Spring框架的IOC之注解方式的快速入门

注解方式大概步骤如下：

```
1. 步骤一：导入注解开发所有需要的jar包
    * 引入IOC容器必须的6个jar包
    * 多引入一个：Spring框架的AOP的jar包，spring-aop的jar包

2. 步骤二：创建对应的包结构，编写Java的类
    * UserService           -- 接口
    * UserServiceImpl       -- 具体的实现类

3. 步骤三：在src的目录下，创建applicationContext.xml的配置文件，然后引入约束。注意：因为现在想使用注解的方式，那么引入的约束发生了变化
    * 需要引入context的约束，具体的约束如下
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
                http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> <!-- bean definitions here -->

        </beans>

4. 步骤四：在applicationContext.xml配置文件中开启组件扫描
    * Spring的注解开发:组件扫描
        <context:component-scan base-package="com.itheima.demo1"/>

    * 注意：可以采用如下配置
        <context:component-scan base-package="com.itheima"/> 这样是扫描com.itheima包下所有的内容

5. 步骤五：在UserServiceImpl的实现类上添加注解
    * @Component(value="userService")   -- 相当于在XML的配置方式中 <bean id="userService" class="...">

6. 步骤六：编写测试代码
    public class SpringDemo1 {
        @Test
        public void run1(){
            ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
            UserService us = (UserService) ac.getBean("userService");
            us.save();
        }
    }
```

注：在导入如下 6 个包之外

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-80613557.jpg)

还需导入 Spring 框架的 AOP 的 jar 包 `spring-aop-4.2.4.RELEASE.jar`。

**简单例子：** 这里简单用代码来演示下。

配置文件`applicationContext.xml`：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> <!-- bean definitions here -->	
	<!-- 开启注解的扫描 -->
	<context:component-scan base-package="com.itheima.demo1"/>
</beans>
```

UserService.java：（业务层接口）

``` java
public interface UserService {
	public void sayHell();
}
```

UserServiceImpl.java：（业务层实现类）

``` java
/**
 * 组件注解，标记类
 * <bean id="userService" class="com.itheima.demo1.UserServiceImpl"> 等价于 @Component(value="userService")
 * @author Administrator
 */
@Component(value="userService")
public class UserServiceImpl implements UserService {
    public void sayHell() {
        System.out.println("hello Spring!!");
        userDao.save();
    }
}
```

运行：

``` java
/**
* 注解的方式
 */
@Test
public void run2(){
    // 获取工厂，加载配置文件
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    // 获取对象
    UserService us = (UserService) ac.getBean("userService");
    us.sayHell();
}
```

运行结果：hello Spring!!

可以看出注解方式与之前方式效果没什么区别。

### 1.2 Spring框架中Bean管理的常用注解

以下是主要内容：

```
1. @Component:组件.(作用在类上)

2. Spring中提供@Component的三个衍生注解:(功能目前来讲是一致的)
    * @Controller       -- 作用在WEB层
    * @Service          -- 作用在业务层
    * @Repository       -- 作用在持久层

    * 说明：这三个注解是为了让标注类本身的用途清晰，Spring在后续版本会对其增强

3. 属性注入的注解(说明：使用注解注入的方式,可以不用提供set方法)
    * 如果是注入的普通类型，可以使用value注解
        * @Value            -- 用于注入普通类型

    * 如果注入的是对象类型，使用如下注解
        * @Autowired        -- 默认按类型进行自动装配
            * 如果想按名称注入
            * @Qualifier    -- 强制使用名称注入

    * @Resource             -- 相当于@Autowired和@Qualifier一起使用
        * 强调：Java提供的注解
        * 属性使用name属性
```

继续用代码演示，承上面代码例子，我们修改代码：

UserServiceImpl.java：

``` java
@Component(value="userService")  
// @Scope(value="prototype")
public class UserServiceImpl implements UserService {
    // 给name属性注入美美的字符串，setName方法还可以省略不写
    @Value(value="美美")
    private String name;
    /*public void setName(String name) {
		this.name = name;
	}*/

    // @Autowired 按类型自动装配
    // @Autowired
    // @Qualifier(value="userDao")		// 按名称注入

    // 是Java的注解，Spring框架支持该注解
    @Resource(name="userDao")
    private UserDao userDao;

    public void sayHell() {
        System.out.println("hello Spring!!"+name);
        userDao.save();
    }
}
```

几点强调的：

1. `@Component(value="userService") ` 可更改为 `@Service(value="userService") `

2. 使用 ` @Value(value="美美")` 给 name 属性注入美美的字符串，setName 方法还可以省略不写

3. `@Autowired` 按类型自动装配什么意思呢？用代码解释下：

   UserDao.java： 

   ``` java
   public interface UserDao 
       public void save();
   }
   ```

   UserDaoImpl.java：

   ``` java
   /**
    * UserDaoImpl交给IOC的容器
    * @author Administrator
    */
   @Repository(value="userDao")
   public class UserDaoImpl implements UserDao {
       public void save() {
           System.out.println("保存客户...");
       }
   }
   ```

   若使用  `@Autowired`

   ``` java
   @Autowired
   private UserDao userDao;
   ```

   进行属性注入，它会自动找 UserDao 接口的实现类对象，如下图为 UserDaoImpl 类对象，再解释下：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-44437919.jpg)

   > 首先 IOC 容器解析完，会有很多对象，比如有 UserDaoImpl、UserServiceImpl 对象（可以观察代码，这两个类有进行注入），其中 UserServiceImpl 有成员属性 UserDao，`@Autowired` 属性注入的意思就是它会去找容器的 UserDao 实现类对象，发现有一个实现类 UserDaoImpl 对象，然后默认就把这个注入进来了，这就是按类型自动装配。但这种方式不好，因为如果 UserDao 有多个实现类，就不知道装配哪个了（即注入不能成功，参考：[当一个接口有多个实现类时，@Autowired会出问题吗？](https://www.oschina.net/question/1158633_229054)）。（可以试着把`@Repository(value="userDao")` 的 value 值改为其他值，可以发现还是能运行 UserDaoImpl 对象的 sava 方法，因为 `@Autowired` 按类型自动装配的。）
   >
   > 一般按名称来装配，即通过给 ID 名称注入。怎么做的呢？比如：
   >
   > UserDaoImpl.java 改为：
   >
   > ``` java
   > @Repository(value="ud")
   > public class UserDaoImpl implements UserDao {
   >     public void save() {
   >         System.out.println("保存客户...");
   >     }
   > }
   > ```
   >
   > 然后可以使用 `@Qualifier` 注入
   >
   > ``` java
   > @Qualifie(value="ud") //按名称 ud 注入
   > private UserDao userDao;
   > ```

   另外可以使用 Java 提供的一个注解，Spring 框架支持该注解。

   ``` java
   @Resource(name="userDao")
   private UserDao userDao;
   ```

   如下图，可以看到所在包的不同

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-84596144.jpg)

   网上相关资料：

   - [Spring @Resource、@Autowired、@Qualifier的注解注入及区别](https://blog.csdn.net/Baple/article/details/17891755)
   - [一个接口多个实现类的Spring注入方式](https://blog.csdn.net/niceLiuSir/article/details/80499821)
   - [@resource、@Autowired、@Service在一个接口多个实现类中的应用](https://blog.csdn.net/jisuli1987/article/details/76036744)

### 1.3 Bean的作用范围和生命周期的注解

   ```
   1. Bean的作用范围注解
       * 注解为@Scope(value="prototype")，作用在类上。值如下：
           * singleton     -- 单例，默认值
           * prototype     -- 多例

   2. Bean的生命周期的配置（了解）
       * 注解如下：
           * @PostConstruct    -- 相当于init-method
           * @PreDestroy       -- 相当于destroy-method
   ```

 如 UserServiceImpl.java：（多例）

   ``` java
@Component(value="userService")
@Scope(value="prototype")
public class UserServiceImpl implements UserService {
    ....

        @PostConstruct
        public void init(){
        System.out.println("初始化...");
    }
}
   ```



## 二、Spring框架整合JUnit单元测试

Spring 框架整合 JUnit 单元测试，什么意思呢？我们可以先看下之前都是怎么进行 JUnit 单元测试的，如下某个例子：

``` java
@Test
public void run2(){
    // 获取工厂，加载配置文件
    ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
    // 获取对象
    UserService us = (UserService) ac.getBean("userService");
    us.sayHell();
}
```

可以看到每次测试我们都得手动 new 这么个工厂，加载配置文件，这样很麻烦。为了方便我们测试，Spring 提供了整合 JUnit 单元测试方案。步骤如下：

```
1. 为了简化了JUnit的测试，使用Spring框架也可以整合测试
2. 具体步骤
    * 要求：必须先有JUnit的环境（即已经导入了JUnit4的开发环境）！！

    * 步骤一：在程序中引入:spring-test.jar
    * 步骤二：在具体的测试类上添加注解
        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext.xml")
        public class SpringDemo1 {

            @Resource(name="userService")
            private UserService userService;

            @Test
            public void demo2(){
                userService.save();
            }
        }
```

代码演示：

``` java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Demo2 {
    
    @Resource(name="userService")
    private UserService UserService;

    @Test
    public void run1(){
        // 原来：获取工厂，加载配置文件，getBean()
        UserService.sayHell();	//直接调用就行
    }
}
```



## 三、AOP的概述

在讲什么 AOP 之前，我们先看例子，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-92610858.jpg)

如上图。在企业开发中我们可能需要写很多很多 DAO 层代码，可能有的是保存用户信息，有的是保存书籍信息，每个方法里面都写了很多代码，但假如有一天有新的需求，比如在保存用户信息的时候记录日志，那不得不打开 DAO 层源代码去修改，如果保存书籍信息也得记录日志，那也得去修改 BookDao 代码。这样的方式不是特别理想，万一不小心的修改，但一上线就报错了出问题了。

那有什么其他的方式，在不改变源代码的情况下，记录日志的功能加上？有的，AOP 可以。

另外，像上图，还可以看到每个 DAO 层都有提交事务这个步骤，每次都得写提交事务的代码，这样其实很麻烦，AOP 可以解决该问题。

1、什么是 AOP 的技术？

- 在软件业，AOP为Aspect Oriented Programming的缩写，意为：**面向切面编程** 
- AOP是一种编程范式，隶属于软工范畴，指导开发者如何组织程序结构
- AOP最早由AOP联盟的组织提出的,制定了一套规范.Spring将AOP思想引入到框架中,必须遵守AOP联盟的规范
- 通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术
- AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型
- 利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率

2、AOP：面向切面编程(思想。—— 解决OOP遇到一些问题)

3、AOP采取横向抽取机制，取代了传统纵向继承体系重复性代码（性能监视、事务管理、安全检查、缓存）

4、为什么要学习AOP：可以在不修改源代码的前提下，对程序进行增强！！   

我们举例来理解下 AOP。我们可以先了解下模块化手机，百度百科 [模块化手机](https://baike.baidu.com/item/%E6%A8%A1%E5%9D%97%E5%8C%96%E6%89%8B%E6%9C%BA/18473322)。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-81698818.jpg)

简单讲就是把手机分成好几个不同模块，比如摄像头模块、电池模块、屏幕模块。这样可以明显看到有很多好处，解耦合了，并且可以自定义，比如摄像头换别的更好的摄像头。

其实 AOP 也是这个思想，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-46522431.jpg)

把「获取session」、「开启事务」等等看做一个个模块。其中有的模块可以直接拿来用就行，也可以自定义。我们可以把很多重复模块直接拿出来编写就行，AOP 采取了横向抽取机制。（这就是 AOP 的宏观思想）



## 四、AOP的底层实现原理

Srping 框架的 AOP 技术底层是采用的代理技术。如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-6125481.jpg)

解释下，如上图，代理对象可以控制目标对象的访问，原来没有代理对象，其他的请求可以直接访问目标对象，让目标对象代码执行，现在有了代理对象，其访问先要经过代理对象，代理对象中可以写任何代码，做很多功能，比如在目标对象执行之前执行记录日志。这样的方式可以看到，我们并没改变 save 方法，但仍能加上了想要的功能。AOP 底层就是这种思想。有两种代理方式：

- JDK 动态代理（针对有接口的方式）
- CGLIB 技术，代理模式，采用生成类的子类的方式（针对没有接口）

``` xml
1. Srping框架的AOP技术底层也是采用的代理技术，代理的方式提供了两种
    1. 基于JDK的动态代理
        * 必须是面向接口的，只有实现了具体接口的类才能生成代理对象

    2. 基于CGLIB动态代理
        * 对于没有实现了接口的类，也可以产生代理，产生这个类的子类的方式

2. Spring的传统AOP中根据类是否实现接口，来采用不同的代理方式
    1. 如果实现类接口，使用JDK动态代理完成AOP
    2. 如果没有实现接口，采用CGLIB动态代理完成AOP
```

**① JDK 动态代理：（代码了解，理解原理）** 

UserDao.java：

``` java
public interface UserDao {
	public void save();	
	public void update();
}
```

UserDaoImpl.java：

``` java
public class UserDaoImpl implements UserDao {
	public void save() {
		System.out.println("保存用户...");
	}	
	public void update() {
		System.out.println("修改用户...");
	}
}
```

MyProxyUtils.java：

``` java
/**
 * 使用JDK的方式生成代理对象
 * @author Administrator
 */
public class MyProxyUtils {
	public static UserDao getProxy(final UserDao dao) {
		// 使用Proxy类生成代理对象
		UserDao proxy = (UserDao) Proxy.newProxyInstance(dao.getClass().getClassLoader(),
				dao.getClass().getInterfaces(), new InvocationHandler() {
					
					// 代理对象方法一执行，invoke方法就会执行一次
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						if("save".equals(method.getName())){
							System.out.println("记录日志...");
							// 开启事务
						}
						// 提交事务
						// 让dao类的save或者update方法正常的执行下去
						return method.invoke(dao, args);
					}
				});
		
		// 返回代理对象
		return proxy;
	}
}
```

测试：

``` java
@Test
public void run1(){
    // 目标对象
    UserDao dao = new UserDaoImpl();
    dao.save();
    dao.update();

    System.out.println("=============================");
    // 使用工具类，获取到代理对象
    UserDao proxy = MyProxyUtils.getProxy(dao);
    // 调用代理对象的方法
    proxy.save();
    proxy.update();
}
```

运行结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-51135014.jpg)

**② CGLIB的代理技术：（代码了解）**

> CGLIB 其实本身是一个单独的项目，被 Spring 单独拿过来了作为它内在的一种技术。
>
> 在使用之前，需要引入CGLIB 的开发的 jar 包，在 Spring 框架核心包中已经引入了 CGLIB 的开发包了，所以直接导入 Spring 核心开发包 `spring-core-4.2.4.RELEASE.jar` 就导入了 CGLIB 开发的 jar 包。

BookDaoImpl.java：（实现类）

``` java
public class BookDaoImpl {
    public void save(){
        System.out.println("保存图书...");
    }
    public void update(){
        System.out.println("修改图书...");
    }
}
```

MyCglibUtils.java：

``` java
public class MyCglibUtils {
    /**
	 * 使用CGLIB方式生成代理的对象
	 * @return
	 */
    public static BookDaoImpl getProxy() {
        Enhancer enhancer = new Enhancer();
        // 设置父类
        enhancer.setSuperclass(BookDaoImpl.class);
        // 设置回调函数
        enhancer.setCallback(new MethodInterceptor() {
            // 代理对象的方法执行，回调函数就会执行
            public Object intercept(Object obj, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                if(method.getName().equals("save")){
                    System.out.println("记录日志...");
                }
                // 正常执行
                return methodProxy.invokeSuper(obj, args);
            }
        });
        // 生成代理对象
        BookDaoImpl proxy = (BookDaoImpl) enhancer.create();
        return proxy;
    }
}
```

测试：

``` java
@Test
public void run1(){
    // 目标对象
    BookDaoImpl dao = new BookDaoImpl();
    dao.save();
    dao.update();
    System.out.println("============================");
    // 使用CGLIB方式生成代理对象
    BookDaoImpl proxy = MyCglibUtils.getProxy();
    proxy.save();
    proxy.update();
}
```

运行结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-62682230.jpg)

////////////////////////////////////////////////////////////

来了解下企业中正常编写代码顺序：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-16287534.jpg)

如上，我们很多人写程序，就是直接干，从 JSP 页面到 Action 层，到 Service 层 再到 Dao 层。

在企业中一般都会是这样，先定义 service 接口，因为做一个系统，系统的业务是固定的，一般接口都是技术带头人定义的，然后在编写 service 实现类，再 Dao 层接口，再 Dao 层实现类，然后写完了进行测试，测试完成了再去写页面。一般大公司都是这个规范，如果是小公司可能就是直接干，遇到问题再说。所以在大公司工作是很枯燥的，因为有可能你只负责某一块。



## 五、AOP功能基于AspectJ的配置文件方式

### 5.1 AOP的相关术语

```
1. Joinpoint(连接点)   -- 所谓连接点是指那些被拦截到的点。在spring中,这些点指的是方法,因为spring只支持方法类型的连接点
2. Pointcut(切入点)        -- 所谓切入点是指我们要对哪些Joinpoint进行拦截的定义
3. Advice(通知/增强)    -- 所谓通知是指拦截到Joinpoint之后所要做的事情就是通知.通知分为前置通知,后置通知,异常通知,最终通知,环绕通知(切面要完成的功能)
4. Introduction(引介) -- 引介是一种特殊的通知在不修改类代码的前提下, Introduction可以在运行期为类动态地添加一些方法或Field
5. Target(目标对象)     -- 代理的目标对象
6. Weaving(织入)      -- 是指把增强应用到目标对象来创建新的代理对象的过程
7. Proxy（代理）        -- 一个类被AOP织入增强后，就产生一个结果代理类
8. Aspect(切面)           -- 是切入点和通知的结合，以后咱们自己来编写和配置的
```

下图解释下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-511290.jpg)

### 5.2 AspectJ的XML方式完成AOP的开发



大概步骤：

```
1. 步骤一：创建JavaWEB项目，引入具体的开发的jar包
    * 先引入Spring框架开发的基本开发包
    * 再引入Spring框架的AOP的开发包
        * spring的传统AOP的开发的包
            * spring-aop-4.2.4.RELEASE.jar
            * com.springsource.org.aopalliance-1.0.0.jar

        * aspectJ的开发包
            * com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar
            * spring-aspects-4.2.4.RELEASE.jar

2. 步骤二：创建Spring的配置文件，引入具体的AOP的schema约束
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:aop="http://www.springframework.org/schema/aop"
           xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

3. 步骤三：创建包结构，编写具体的接口和实现类
    * com.itheima.demo2
        * CustomerDao           -- 接口
        * CustomerDaoImpl       -- 实现类

4. 步骤四：将目标类配置到Spring中
    <bean id="customerDao" class="com.itheima.demo3.CustomerDaoImpl"/>

5. 步骤五：定义切面类
    public class MyAspectXml {
        // 定义通知
        public void log(){
            System.out.println("记录日志...");
        }
    }

6. 步骤六：在配置文件中定义切面类
    <bean id="myAspectXml" class="com.itheima.demo3.MyAspectXml"/>

7. 步骤七：在配置文件中完成aop的配置
    <aop:config>
        <!-- 引入切面类 -->
        <aop:aspect ref="myAspectXml">
            <!-- 定义通知类型：切面类的方法和切入点的表达式 -->
            <aop:before method="log" pointcut="execution(public * com.itheima.demo3.CustomerDaoImpl.save(..))"/>
        </aop:aspect>
    </aop:config>

8. 完成测试
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class Demo3 {
        @Resource(name="customerDao")
        private CustomerDao customerDao;
        @Test
        public void run1(){
            customerDao.save();
            customerDao.update();
            customerDao.delete();
        }
    }
```

CustomerDao.java：

``` java
public interface CustomerDao {
	public void save();
	public void update();
}
```

CustomerDaoImpl.java：

``` java
public class CustomerDaoImpl implements CustomerDao {
	public void save() {
		// 模拟异常
		// int a = 10/0;	
		System.out.println("保存客户...");
	}
	public void update() {
		System.out.println("修改客户...");
	}
}
```

MyAspectXml.java：

``` java
/**
 * 切面类：切入点 + 通知
 * @author Administrator
 */
public class MyAspectXml {
	
	/**
	 * 通知（具体的增强）
	 */
	public void log(){
		System.out.println("记录日志...");
	}
	
	/**
	 * 最终通知：方法执行成功或者出现异常，都会执行
	 */
	public void after(){
		System.out.println("最终通知...");
	}
	
	/**
	 * 方法执行之后，执行后置通知。程序出现了异常，后置通知不会执行的。
	 */
	public void afterReturn(){
		System.out.println("后置通知...");
	}
	
	/**
	 * 环绕通知：方法执行之前和方法执行之后进行通知，默认的情况下，目标对象的方法不能执行的。需要手动让目标对象的方法执行
	 */
	public void around(ProceedingJoinPoint joinPoint){
		System.out.println("环绕通知1...");
		try {
			// 手动让目标对象的方法去执行
			joinPoint.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		System.out.println("环绕通知2...");
	}
}
```

applicationContext.xml：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> <!-- bean definitions here -->
	
	<!-- 配置客户的dao -->
	<bean id="customerDao" class="com.itheima.demo3.CustomerDaoImpl"/>
	
	<!-- 编写切面类配置好 -->
	<bean id="myAspectXml" class="com.itheima.demo3.MyAspectXml"/>
	
	<!-- 配置AOP -->
	<aop:config>
		<aop:aspect ref="myAspectXml">
			<!-- 切入点的表达式 
				1. execution()	固定的，不能不写
				2. public 可以省略不写
				3. void，返回值可以出现 * 表示任意的返回值，返回值类型不能不写
				4. 可以使用 * 代替的，不能不编写的，简写方式：*..*方法
				5. *DaoImpl
				6. 方法 save*
				7. 方法的参数：
			-->
			<!-- <aop:before method="log" pointcut="execution(public void com.itheima.demo3.CustomerDaoImpl.save())"/> -->
			<!-- public 可以省略不写 -->
			<!-- <aop:before method="log" pointcut="execution(void com.itheima.demo3.CustomerDaoImpl.save())"/> -->
			
			<!-- void，返回值可以出现 * 表示任意的返回值，返回值类型不能不写 -->
			<!-- <aop:before method="log" pointcut="execution(* com.itheima.demo3.CustomerDaoImpl.save())"/> -->
			
			<!-- 包名可以使用 * 代替，不能不写 -->
			<!-- <aop:before method="log" pointcut="execution(* com.itheima.*.CustomerDaoImpl.save())"/> -->
			
			<!-- 包的简写的方式，任意的包的结构 -->
			<!-- <aop:before method="log" pointcut="execution(* *..*.CustomerDaoImpl.save())"/> -->
			
			<!-- 编写类的写法 -->
			<!-- <aop:before method="log" pointcut="execution(* *..*.*DaoImpl.save())"/> -->
			
			<!-- 方法编写 -->
			<!-- <aop:before method="log" pointcut="execution(* *..*.*DaoImpl.save*())"/> -->
			
			<!-- 参数列表：出现一个*，表示一个参数，任意参数使用 .. -->
			<aop:before method="log" pointcut="execution(* *..*.*DaoImpl.save*(..))"/>
		</aop:aspect>
	</aop:config>

</beans>

```

### 5.3 切入点的表达式

```
1. 再配置切入点的时候，需要定义表达式，重点的格式如下：execution(public * *(..))，具体展开如下：
    * 切入点表达式的格式如下：
        * execution([修饰符] 返回值类型 包名.类名.方法名(参数))

    * 修饰符可以省略不写，不是必须要出现的。
    * 返回值类型是不能省略不写的，根据你的方法来编写返回值。可以使用 * 代替。
    * 包名例如：com.itheima.demo3.BookDaoImpl
        * 首先com是不能省略不写的，但是可以使用 * 代替
        * 中间的包名可以使用 * 号代替
        * 如果想省略中间的包名可以使用 .. 

    * 类名也可以使用 * 号代替，也有类似的写法：*DaoImpl
    * 方法也可以使用 * 号代替
    * 参数如果是一个参数可以使用 * 号代替，如果想代表任意参数使用 ..
```

### 5.4 AOP的通知类型

```
1. 前置通知
    * 在目标类的方法执行之前执行。
    * 配置文件信息：<aop:after method="before" pointcut-ref="myPointcut3"/>
    * 应用：可以对方法的参数来做校验

2. 最终通知
    * 在目标类的方法执行之后执行，如果程序出现了异常，最终通知也会执行。
    * 在配置文件中编写具体的配置：<aop:after method="after" pointcut-ref="myPointcut3"/>
    * 应用：例如像释放资源

3. 后置通知
    * 方法正常执行后的通知。       
    * 在配置文件中编写具体的配置：<aop:after-returning method="afterReturning" pointcut-ref="myPointcut2"/>
    * 应用：可以修改方法的返回值

4. 异常抛出通知
    * 在抛出异常后通知
    * 在配置文件中编写具体的配置：<aop:after-throwing method="afterThorwing" pointcut-ref="myPointcut3"/> 
    * 应用：包装异常的信息

5. 环绕通知
    * 方法的执行前后执行。
    * 在配置文件中编写具体的配置：<aop:around method="around" pointcut-ref="myPointcut2"/>
    * 要注意：目标的方法默认不执行，需要使用ProceedingJoinPoint对来让目标对象的方法执行。
```



