
## 前言

**Spring框架的学习路线：**

``` xml
1. Spring第一天：Spring的IOC容器之XML的方式，Spring框架与Web项目整合
2. Spring第二天：Spring的IOC容器之注解的方式，Spring的AOP技术
3. Spring第三天：Spring的事务管理、Spring框架的JDBC模板
4. Spring第四天：SSH三大框架的整合
```

这是第三天学习大纲：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-18460953.jpg)



## 一、Spring框架的AOP之注解的方式

大概步骤如下：

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

    </beans>

3. 步骤三：创建包结构，编写具体的接口和实现类
    * com.itheima.demo1
        * CustomerDao           -- 接口
        * CustomerDaoImpl       -- 实现类

4. 步骤四：将目标类配置到Spring中
    <bean id="customerDao" class="com.itheima.demo1.CustomerDaoImpl"/>

5. 步骤五：定义切面类
    * 添加切面和通知的注解
        * @Aspect                   -- 定义切面类的注解

        * 通知类型（注解的参数是切入点的表达式）
            * @Before               -- 前置通知
            * @AfterReturing        -- 后置通知
            * @Around               -- 环绕通知
            * @After                -- 最终通知
            * @AfterThrowing        -- 异常抛出通知

    * 具体的代码如下
        @Aspect
        public class MyAspectAnno {
            @Before(value="execution(public void com.itheima.demo1.CustomerDaoImpl.save())")
            public void log(){
                System.out.println("记录日志...");
            }
        }

6. 步骤六：在配置文件中定义切面类
    <bean id="myAspectAnno" class="com.itheima.demo1.MyAspectAnno"/>

7. 步骤七：在配置文件中开启自动代理
    <aop:aspectj-autoproxy/>

8. 完成测试
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class Demo1 {

        @Resource(name="customerDao")
        private CustomerDao customerDao;

        @Test
        public void run1(){
            customerDao.save();
            customerDao.update();
        }
    }
```

-----------------------------------------------直接上代码学习-------------------------------------------------

MyAspectAnno.java：

``` java
/**
 * 注解方式的切面类
 * @author Administrator
 */
@Aspect
public class MyAspectAnno {
	/** 方式1：
    @Before(value="execution(public * com.itheima.demo1.CustomerDaoImpl.save())")
	public void log(){
		System.out.println("记录日志...");
	}
    */
	/** 方式2：（使用自定义切入点）
	 * 通知类型：@Before前置通知（切入点的表达式）
	 */
	@Before(value="MyAspectAnno.fn()")
	public void log(){
		System.out.println("记录日志...");
	}
	
	/**
	 * 最终通知：方法执行成功或者右异常，都会执行
	 */
	@After(value="MyAspectAnno.fn()")
	public void after(){
		System.out.println("最终通知...");
	}
	
	/**
	 * 环绕通知
	 */
	@Around(value="MyAspectAnno.fn()")
	public void around(ProceedingJoinPoint joinPoint){
		System.out.println("环绕通知1...");
		try {
			// 让目标对象的方法执行
			joinPoint.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		System.out.println("环绕通知2...");
	}
	
	/**
	 * 自动定义切入点	@Pointcut
	 */
	@Pointcut(value="execution(public * com.itheima.demo1.CustomerDaoImpl.save())")
	public void fn(){}
	
}
```

applicationContext.xml 添加：

``` xml
<!-- 开启自动代理 -->
<aop:aspectj-autoproxy/>

<!-- 配置目标对象 -->
<bean id="customerDao" class="com.itheima.demo1.CustomerDaoImpl"/>

<!-- 配置切面类 -->
<bean id="myAspectAnno" class="com.itheima.demo1.MyAspectAnno"/>
```

测试 Demo1.java：

``` java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Demo1 {
    @Resource(name="customerDao")
    private CustomerDao customerDao;

    @Test
    public void run1(){
        customerDao.save();
        customerDao.update();
    }
}
```

**通知类型：**

```
1. 通知类型
    * @Before               -- 前置通知
    * @AfterReturing        -- 后置通知
    * @Around               -- 环绕通知（目标对象方法默认不执行的，需要手动执行）
    * @After                -- 最终通知
    * @AfterThrowing        -- 异常抛出通知

2. 配置通用的切入点
    * 使用@Pointcut定义通用的切入点

    @Aspect
    public class MyAspectAnno {
        @Before(value="MyAspectAnno.fn()")
        public void log(){
            System.out.println("记录日志...");
        }
        @Pointcut(value="execution(public void com.itheima.demo1.CustomerDaoImpl.save())")
        public void fn(){}
    }
```



## 二、Spring框架的JDBC模板

### 2.1 JDBC的模板类的演示

```
1. 步骤一：创建数据库的表结构
    create database spring_day03;
    use spring_day03;
    create table t_account(
        id int primary key auto_increment,
        name varchar(20),
        money double
    );

2. 引入开发的jar包
    * 先引入IOC基本的6个jar包
    * 再引入Spring-aop的jar包
    * 最后引入JDBC模板需要的jar包
        * MySQL数据库的驱动包：mysql-connector-java-5.1.7-bin.jar
        * Spring-jdbc.jar
        * Spring-tx.jar

3. 编写测试代码（自己来new对象的方式）
    @Test
    public void run1(){
        // 创建连接池，先使用Spring框架内置的连接池
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///spring_day03");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        // 创建模板类
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        // 完成数据的添加
        jdbcTemplate.update("insert into t_account values (null,?,?)", "测试",10000);
    }
```

如上使用的为 **Spring 框架内置的连接池**。

### 2.2 使用Spring框架来管理模板类

上节中直接在测试代码中编写（即自己 new 对象的方式），我们难道不可以把连接池、模板类交给 Spring 管理吗？可以的。如下：

在 applicationContext.xml 添加：

``` xml
<!-- 内置的连接池：先配置连接池 -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql:///spring_day03"/>
    <property name="username" value="root"/>
    <property name="password" value="root"/>
</bean>

<!-- 配置JDBC的模板类 -->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

然后测试就可以这样编写了：

``` java
/**
 * 测试JDBC的模板类，使用IOC的方式
 * @author Administrator
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class Demo1_1 {
	@Resource(name="jdbcTemplate")
	private JdbcTemplate jdbcTemplate;
	
	@Test
	public void run1(){
		jdbcTemplate.update("insert into t_account values (null,?,?)", "小苍",10000);
	}
}
```

总结步骤如下：

```
1. 刚才编写的代码使用的是new的方式，应该把这些类交给Spring框架来管理。
2. 修改的步骤如下
    * 步骤一：Spring管理内置的连接池
        <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///spring_day03"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
        </bean>

    * 步骤二：Spring管理模板类
        <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
            <property name="dataSource" ref="dataSource"/>
        </bean>

    * 步骤三：编写测试程序
        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext.xml")
        public class Demo2 {

            @Resource(name="jdbcTemplate")
            private JdbcTemplate jdbcTemplate;

            @Test
            public void run2(){
                jdbcTemplate.update("insert into t_account values (null,?,?)", "测试2",10000);
            }
        }
```

### 2.3 Spring框架管理开源的连接池

如果使用的不是 Spring 内置的连接池，使用的开源的连接池，该如何管理开源的连接池呢：

```
1. 管理DBCP连接池
    * 先引入DBCP的2个jar包
        * com.springsource.org.apache.commons.dbcp-1.2.2.osgi.jar
        * com.springsource.org.apache.commons.pool-1.5.3.jar

    * 编写配置文件
        <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql:///spring_day03"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
        </bean>

2. 管理C3P0连接池
    * 先引入C3P0的jar包
        * com.springsource.com.mchange.v2.c3p0-0.9.1.2.jar

    * 编写配置文件
        <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
            <property name="driverClass" value="com.mysql.jdbc.Driver"/>
            <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
            <property name="user" value="root"/>
            <property name="password" value="root"/>
        </bean>
```

### 2.4 Spring框架的JDBC模板的简单操作

```
1. 增删改查的操作
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class SpringDemo3 {

        @Resource(name="jdbcTemplate")
        private JdbcTemplate jdbcTemplate;

        @Test
        // 插入操作
        public void demo1(){
            jdbcTemplate.update("insert into account values (null,?,?)", "冠希",10000d);
        }

        @Test
        // 修改操作
        public void demo2(){
            jdbcTemplate.update("update account set name=?,money =? where id = ?", "思雨",10000d,5);
        }

        @Test
        // 删除操作
        public void demo3(){
            jdbcTemplate.update("delete from account where id = ?", 5);
        }

        @Test
        // 查询一条记录
        public void demo4(){
            Account account = jdbcTemplate.queryForObject("select * from account where id = ?", new BeanMapper(), 1);
            System.out.println(account);
        }

        @Test
        // 查询所有记录
        public void demo5(){
            List<Account> list = jdbcTemplate.query("select * from t_account", new BeanMapper());
            for (Account account : list) {
                System.out.println(account);
            }
        }
    }

    class BeanMapper implements RowMapper<Account>{
        public Account mapRow(ResultSet rs, int arg1) throws SQLException {
            Account account = new Account();
            account.setId(rs.getInt("id"));
            account.setName(rs.getString("name"));
            account.setMoney(rs.getDouble("money"));
            return account;
        }
    }
```

查询所有记录 demo5( )方法的输出结果如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-8025446.jpg)



## 三、Spring框架的事务管理

### 3.1 事务的回顾

```
1. 事务：指的是逻辑上一组操作，组成这个事务的各个执行单元，要么一起成功,要么一起失败！
2. 事务的特性
    * 原子性
    * 一致性
    * 隔离性
    * 持久性

3. 如果不考虑隔离性,引发安全性问题
    * 读问题:
        * 脏读:
        * 不可重复读:
        * 虚读:

    * 写问题:
        * 丢失更新:

4. 如何解决安全性问题
    * 读问题解决，设置数据库隔离级别
    * 写问题解决可以使用 悲观锁和乐观锁的方式解决
```

### 3.2 Spring框架的事务管理相关的类和API

```
1. PlatformTransactionManager接口     -- 平台事务管理器.(真正管理事务的类)。该接口有具体的实现类，根据不同的持久层框架，需要选择不同的实现类！
2. TransactionDefinition接口          -- 事务定义信息.(事务的隔离级别,传播行为,超时,只读)
3. TransactionStatus接口              -- 事务的状态

4. 总结：上述对象之间的关系：平台事务管理器真正管理事务对象.根据事务定义的信息TransactionDefinition 进行事务管理，在管理事务中产生一些状态.将状态记录到TransactionStatus中

5. PlatformTransactionManager接口中实现类和常用的方法
    1. 接口的实现类
        * 如果使用的Spring的JDBC模板或者MyBatis框架，需要选择DataSourceTransactionManager实现类
        * 如果使用的是Hibernate的框架，需要选择HibernateTransactionManager实现类

    2. 该接口的常用方法
        * void commit(TransactionStatus status) 
        * TransactionStatus getTransaction(TransactionDefinition definition) 
        * void rollback(TransactionStatus status) 

6. TransactionDefinition
    1. 事务隔离级别的常量
        * static int ISOLATION_DEFAULT                  -- 采用数据库的默认隔离级别
        * static int ISOLATION_READ_UNCOMMITTED 
        * static int ISOLATION_READ_COMMITTED 
        * static int ISOLATION_REPEATABLE_READ 
        * static int ISOLATION_SERIALIZABLE 

    2. 事务的传播行为常量（不用设置，使用默认值）
        * 先解释什么是事务的传播行为：解决的是业务层之间的方法调用！！

        * PROPAGATION_REQUIRED（默认值） -- A中有事务,使用A中的事务.如果没有，B就会开启一个新的事务,将A包含进来.(保证A,B在同一个事务中)，默认值！！
        * PROPAGATION_SUPPORTS          -- A中有事务,使用A中的事务.如果A中没有事务.那么B也不使用事务.
        * PROPAGATION_MANDATORY         -- A中有事务,使用A中的事务.如果A没有事务.抛出异常.

        * PROPAGATION_REQUIRES_NEW（记）-- A中有事务,将A中的事务挂起.B创建一个新的事务.(保证A,B没有在一个事务中)
        * PROPAGATION_NOT_SUPPORTED     -- A中有事务,将A中的事务挂起.
        * PROPAGATION_NEVER             -- A中有事务,抛出异常.

        * PROPAGATION_NESTED（记）     -- 嵌套事务.当A执行之后,就会在这个位置设置一个保存点.如果B没有问题.执行通过.如果B出现异常,运行客户根据需求回滚(选择回滚到保存点或者是最初始状态)
```

关于事务的传播行为的理解可以先看下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-32704644.jpg)



这里 save 方法当做 A，find 方法当做 B，假设传播行为设置为 `PROPAGATION_SUPPORTS`，则 A 中有事务，使用 A 中的事务，如果 A 中没有事务，那么 B 也不使用事务。

### 3.3 搭建事务管理转账案例的环境（强调：简化开发，以后DAO可以继承JdbcDaoSupport类）

```
1. 步骤一：创建WEB工程，引入需要的jar包
    * IOC的6个包
    * AOP的4个包
    * C3P0的1个包
    * MySQL的驱动包
    * JDBC目标2个包
    * 整合JUnit测试包

2. 步骤二：引入配置文件
    * 引入配置文件
        * 引入log4j.properties

        * 引入applicationContext.xml
            <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
                <property name="driverClass" value="com.mysql.jdbc.Driver"/>
                <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
                <property name="user" value="root"/>
                <property name="password" value="root"/>
            </bean>

3. 步骤三：创建对应的包结构和类
    * com.itheima.demo1
    * AccountService
    * AccountServlceImpl
    * AccountDao
    * AccountDaoImpl

4. 步骤四:引入Spring的配置文件,将类配置到Spring中
    <bean id="accountService" class="com.itheima.demo1.AccountServiceImpl">
    </bean>

    <bean id="accountDao" class="com.itheima.demo1.AccountDaoImpl">
    </bean>

5. 步骤五：在业务层注入DAO ,在DAO中注入JDBC模板（强调：简化开发，以后DAO可以继承JdbcDaoSupport类）
    <bean id="accountService" class="com.itheima.demo1.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"/>
    </bean>

    <bean id="accountDao" class="com.itheima.demo1.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"/>
    </bean>

6. 步骤六：编写DAO和Service中的方法
    public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
        public void outMoney(String out, double money) {
            this.getJdbcTemplate().update("update t_account set money = money = ? where name = ?", money,out);
        }
        public void inMoney(String in, double money) {
            this.getJdbcTemplate().update("update t_account set money = money + ? where name = ?", money,in);
        }
    }

7. 步骤七：编写测试程序.
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class Demo1 {

        @Resource(name="accountService")
        private AccountService accountService;

        @Test
        public void run1(){
            accountService.pay("冠希", "美美", 1000);
        }
    }
```

关于步骤 5 中，AccountDaoImpl.java 代码按理应该如下这样写：

``` java
public class AccountDaoImpl implements AccountDao {
    private JdbcTemplate jdbcTemplate;
    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
    
    //扣钱
    public void outMoney(String out, double money) {
        jdbcTemplate.update("update t_account set money = money - ? where name = ?", money,out);
    }
    //加钱
    public void inMoney(String in, double money) {
        jdbcTemplate.update("update t_account set money = money + ? where name = ?", money,in);
    }
}
```

按逻辑来说来说在业务层注入 DAO ，在 DAO 中注入 JDBC 模板，applicationContext.xml 添加如下：

``` xml
<!-- 配置C3P0的连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
</bean>
<!-- 配置JDBC的模板类。 在 JDBC 模板注入连接池-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!-- DAO 中注入 JDBC 模板 -->
<bean id="accountDao" class="com.itheima.demo1.AccountDaoImpl">
    <property name="jdbcTemplate" ref="jdbcTemplate"/> 
</bean>
```

**解释：** 毕竟业务层需要操作 Dao 层对象，Dao 层需要操作 JDBC 模板类对象，JDBC 需要连接池对象，所以如上。

但是为了简化开发，其实 DAO 可以继承 JdbcDaoSupport 这个类？AccountDaoImpl.java 可以如下编写：（继承 JdbcDaoSupport ）

``` java
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
    //扣钱
    public void outMoney(String out, double money) {
        this.getJdbcTemplate().update("update t_account set money = money - ? where name = ?", money,out);
    }
    //加钱
    public void inMoney(String in, double money) {
        this.getJdbcTemplate().update("update t_account set money = money + ? where name = ?", money,in);
    }
}
```

JdbcDaoSupport 是一个什么类？可以查看源代码发现：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-48531406.jpg)

可以看到 setDataSource( ) 方法中如果模板类对象为空，会根据连接池创建一个模板，并赋值给成员属性模板类对象 jdbcTemplate。这里代码啥意思？划重点，这里代码表示创建模板对象之前的前提得是，我先有一个连接池，再根据连接池创建 JDBC 模板类对象。

所以在配置文件写注入配置的时候，DAO 层可以考虑这样写：

``` xml
<bean id="accountDao" class="com.itheima.demo1.AccountDaoImpl">
    <!-- <property name="jdbcTemplate" ref="jdbcTemplate"/> -->
    <property name="dataSource" ref="dataSource"/>
</bean>
```

直接注入连接池即可，而不需要再注入 JDBC 模板类。因为注入连接池之后，底层会根据连接池创建一个 JDBC 模板。（请再回头看 JdbcDaoSupport  源码截图体会。）

推导过程：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-16748540.jpg)





### 3.4 Spring框架的事务管理的分类

```
1. Spring的事务管理的分类
    1. Spring的编程式事务管理（不推荐使用）
        * 通过手动编写代码的方式完成事务的管理（不推荐）

    2. Spring的声明式事务管理（底层采用AOP的技术）
        * 通过一段配置的方式完成事务的管理（重点掌握注解的方式）
```

**1、Spring框架的事务管理之编程式的事务管理（了解）**

```
1. 说明：Spring为了简化事务管理的代码:提供了模板类 TransactionTemplate，所以手动编程的方式来管理事务，只需要使用该模板类即可！！

2. 手动编程方式的具体步骤如下：
    1. 步骤一:配置一个事务管理器，Spring使用PlatformTransactionManager接口来管理事务，所以咱们需要使用到他的实现类！！
        <!-- 配置事务管理器 -->
        <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource"/>
        </bean>

    2. 步骤二:配置事务管理的模板
        <!-- 配置事务管理的模板 -->
        <bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
            <property name="transactionManager" ref="transactionManager"/>
        </bean>

    3. 步骤三:在需要进行事务管理的类中,注入事务管理的模板.
        <bean id="accountService" class="com.itheima.demo1.AccountServiceImpl">
            <property name="accountDao" ref="accountDao"/>
            <property name="transactionTemplate" ref="transactionTemplate"/>
        </bean>

    4. 步骤四:在业务层使用模板管理事务:
        // 注入事务模板对象
        private TransactionTemplate transactionTemplate;
        public void setTransactionTemplate(TransactionTemplate transactionTemplate) {
            this.transactionTemplate = transactionTemplate;
        }

        public void pay(final String out, final String in, final double money) {
            transactionTemplate.execute(new TransactionCallbackWithoutResult() {

                protected void doInTransactionWithoutResult(TransactionStatus status) {
                    // 扣钱
                    accountDao.outMoney(out, money);
                    int a = 10/0;
                    // 加钱
                    accountDao.inMoney(in, money);
                }
            });
        }
```



推导过程思路：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-22-82580791.jpg)

完整 applicationContext.xml 配置：

``` xml
<!-- 配置C3P0的连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
</bean>

<!-- 配置平台事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!-- 手动编码，提供了模板类，使用该类管理事务比较简单 -->
<bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
    <property name="transactionManager" ref="transactionManager"/>
</bean>

<!-- 配置JDBC的模板类 
 <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
  <property name="dataSource" ref="dataSource"/>
 </bean>
 -->

<!-- 配置业务层和持久层 -->
<bean id="accountService" class="com.itheima.demo1.AccountServiceImpl">
    <property name="accountDao" ref="accountDao"/>
    <property name="transactionTemplate" ref="transactionTemplate"/>
</bean>

<bean id="accountDao" class="com.itheima.demo1.AccountDaoImpl">
    <!-- <property name="jdbcTemplate" ref="jdbcTemplate"/> -->
    <property name="dataSource" ref="dataSource"/>
</bean>
```

**2、Spring框架的事务管理之声明式事务管理，即通过配置文件来完成事务管理(AOP思想)**

声明式事务管理又分成两种方式：

1. 基于AspectJ的XML方式（重点掌握）
2. 基于AspectJ的注解方式（重点掌握）

**① 基于AspectJ的XML方式：（重点掌握）**

```
1. 步骤一:恢复转账开发环境

2. 步骤二:引入AOP的开发包

3. 步骤三:配置事务管理器
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

4. 步骤四:配置事务增强
    <!-- 配置事务增强 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!--
                name        ：绑定事务的方法名，可以使用通配符，可以配置多个。
                propagation ：传播行为
                isolation   ：隔离级别
                read-only   ：是否只读
                timeout     ：超时信息
                rollback-for：发生哪些异常回滚.
                no-rollback-for：发生哪些异常不回滚.
             -->
            <!-- 哪些方法加事务 -->
            <tx:method name="pay" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

5. 步骤五:配置AOP的切面
    <!-- 配置AOP切面产生代理 -->
    <aop:config>
        <aop:advisor advice-ref="myAdvice" pointcut="execution(* com.itheima.demo2.AccountServiceImpl.pay(..))"/>
    </aop:config>

    * 注意：如果是自己编写的切面，使用<aop:aspect>标签，如果是系统制作的，使用<aop:advisor>标签。

6. 步骤六:编写测试类
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext2.xml")
    public class Demo2 {

        @Resource(name="accountService")
        private AccountService accountService;

        @Test
        public void run1(){
            accountService.pay("冠希", "美美", 1000);
        }
    }
```

applicationContext.xml：

``` xml
<!-- 配置C3P0的连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
</bean>

<!-- 配置平台事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!-- 声明式事务（采用XML配置文件的方式） -->
<!-- 先配置通知 -->
<tx:advice id="myAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- 给方法设置数据库属性（隔离级别，传播行为） -->
        <tx:method name="pay" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>

<!-- 配置AOP：如果是自己编写的AOP，使用aop:aspect配置，使用的是Spring框架提供的通知aop:advisor -->
<aop:config>
    <!-- aop:advisor，是Spring框架提供的通知 -->
    <aop:advisor advice-ref="myAdvice" pointcut="execution(public * com.itheima.demo2.AccountServiceImpl.pay(..))"/>
</aop:config>

<!-- 配置业务层和持久层 -->
<bean id="accountService" class="com.itheima.demo2.AccountServiceImpl">
    <property name="accountDao" ref="accountDao"/>
</bean>

<bean id="accountDao" class="com.itheima.demo2.AccountDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

**② 基于AspectJ的注解方式：（重点掌握，最简单的方式）**

```
1. 步骤一:恢复转账的开发环境

2. 步骤二:配置事务管理器
    <!-- 配置事务管理器  -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

3. 步骤三:开启注解事务
    <!-- 开启注解事务 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>

4. 步骤四:在业务层上添加一个注解:@Transactional

5. 编写测试类
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext3.xml")
    public class Demo3 {

        @Resource(name="accountService")
        private AccountService accountService;

        @Test
        public void run1(){
            accountService.pay("冠希", "美美", 1000);
        }
    }
```

注：关于步骤四，如果在类上添加了注解 `@Transactional`，则表示类中所有方法全部都有事务。

applicationContext.xml：

``` xml
<!-- 配置C3P0的连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql:///spring_day03"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
</bean>
 
<!-- 配置平台事务管理器 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!-- 开启事务的注解 -->
<tx:annotation-driven tr
                       ansaction-manager="transactionManager"/>

<!-- 配置业务层和持久层 -->
<bean id="accountService" class="com.itheima.demo3.AccountServiceImpl">
    <property name="accountDao" ref="accountDao"/>
</bean>

<bean id="accountDao" class="com.itheima.demo3.AccountDaoImpl">
    <property name="dataSource" ref="dataSource"/>
</bean>
```



---

相关阅读：[Spring 教程 - 极客学院](http://wiki.jikexueyuan.com/project/spring/)

