
## 前言

Mybatis 第二天学习大纲：

``` xml
1. 输入映射和输出映射
   1. 输入参数映射
   2. 返回值映射
2. 动态 sql
   1. if
   2. Where
   3. Foreach
   4. sql 片段
3. 关联查询
   1. 一对一关联
   2. 一对多关联
4. Mybatis 整合 Spring
```



## 一、输入映射和输出映射

Mapper.xml 映射文件中定义了操作数据库的 sql，每个 sql 是一个 statement，映射文件是 mybatis 的核心。

### 1.1 parameterType(输入类型)

1、传递简单类型（参考《Mybatis学习笔记1》）

2、传递 POJO 对象：Mybatis 使用 ognl 表达式解析对象字段的值，`#{}`或者`${}`括号中的值为 pojo 属性名称。

3、传递 POJO 包装对象

> 开发中通过 pojo 传递查询条件 ，查询条件是综合的查询条件，不仅包括用户查询条件还包括其它的查询条件（比如将用户购买商品信息也作为查询条件），这时可以使用包装对象传递输入参数。

举例。如有这样需求：根据用户名和性别来查询。

User.java：

``` java
public class User {
	private int id;
	private String username;// 用户姓名
	private String sex;// 性别
	private Date birthday;// 生日
	private String address;// 地址
	
    get()/set()方法，此处省略......
}
```

QueryVo.java：

``` java
public class QueryVo {
    private User user;

    get()/set()方法，此处省略......
}
```

UserMapper.xml：

``` xml
<select id="findUserbyVo" parameterType="cn.itheima.pojo.QueryVo" resultType="cn.itheima.pojo.User">
    select * from user where username like '%${user.username}%' and sex=#{user.sex}
</select>
```

接口 UserMapper.java：

``` java
public List<User> findUserbyVo(QueryVo vo);
```

测试 UserMapperTest.java：

``` java
@Test
public void testFindUserByVo() throws Exception{
    SqlSession openSession = factory.openSession();
    //通过getMapper方法来实例化接口
    UserMapper mapper = openSession.getMapper(UserMapper.class);

    QueryVo vo = new QueryVo();
    User user = new User();
    user.setUsername("王");
    user.setSex("1");
    vo.setUser(user);

    List<User> list = mapper.findUserbyVo(vo);
    System.out.println(list);
}
```

### 1.2 resultType(输出类型)

1、输出简单类型，举例

``` xml
<!-- 获取用户列表总数 -->
<!-- 只有返回结果为一行一列的时候,那么返回值类型才可以指定成基本类型 -->
<select id="findUserCount" resultType="java.lang.Integer">
    select count(*) from user 
</select>
```

数据库查询结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-94571558.jpg)

2、输出 POJO 对象（参考《Mybatis学习笔记1》）

3、输出 POJO 列表（参考《Mybatis学习笔记1》）

### 1.3 resultMap

resultType 可以指定 pojo 将查询结果映射为 pojo，但需要 pojo 的属性名和 sql 查询的列名一致方可映射成功。

如果 sql 查询字段名和 pojo 的属性名不一致，可以通过 resultMap 将字段名和属性名作一个对应关系 ，resultMap 实质上还需要将查询结果映射到 pojo 对象中。 

resultMap 可以实现将查询结果映射为复杂类型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询。

① Mapper.xml 定义：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-33051180.jpg)

使用 resultMap 指定上边定义的 personmap。

② 定义 resultMap：

由于上边的 mapper.xml 中 sql 查询列和 Users.java 类属性不一致，需要定义 resultMap：userListResultMap 将sql查询列和 Users.java 类属性对应起来

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-96852809.jpg)

``` xml
<id />：此属性表示查询结果集的唯一标识，非常重要。如果是多个字段为复合唯一约束则定义多个<id />。
Property：表示User类的属性。
Column：表示sql查询出来的字段名。
Column和property放在一块儿表示将sql查询出来的字段映射到指定的pojo类属性上。

<result />：普通结果，即pojo的属性。
```

③ Mapper 接口定义：`public List<User> findUserListResultMap() throws Exception;`



## 二、动态sql

我们通过案例演示 if、where、sql 封装。

如根据用户姓名或性别查询。

``` xml
<select id="findUserByUserNameAndSex" parameterType="cn.itheima.pojo.User" resultType="cn.itheima.pojo.User">
    select * from user wherw 1=1
    <if test="username != null and username != ''">
        and username like '%${username}%'
    </if>
    <if test="sex != null and sex != ''">
        and sex=#{sex}
    </if>
</select>
```

这里通过 where 1=1 方式，为了避免 where 关键字后面的第一个词直接就是 “and”而导致语法错误。

但其实也可以用 where：

``` xml
<select id="findUserByUserNameAndSex" parameterType="cn.itheima.pojo.User" resultType="cn.itheima.pojo.User">
    select * from user wherw 1=1
    <!-- where标签作用:
        会自动向sql语句中添加where关键字
        会去掉第一个条件的and关键字
    -->
    <where>
        <if test="username != null and username != ''">
            and username like '%${username}%'
        </if>
        <if test="sex != null and sex != ''">
            and sex=#{sex}
        </if>
    </where>
</select>
```

另外还可以这样：

``` xml
<!-- 封装sql条件,封装后可以重用. 
 id:是这个sql条件的唯一标识 -->
<sql id="user_Where">
    <!-- where标签作用:
    会自动向sql语句中添加where关键字
    会去掉第一个条件的and关键字
    -->
    <where>
        <if test="username != null and username != ''">
            and username like '%${username}%'
        </if>
        <if test="sex != null and sex != ''">
            and sex=#{sex}
        </if>
    </where>
</sql>

<select id="findUserByUserNameAndSex" parameterType="cn.itheima.pojo.User" resultType="cn.itheima.pojo.User">
    select * from user 

    <!-- 调用sql条件 -->
    <include refid="user_Where"></include>
</select>
```

演示 Foreach：（根据用户名 id 查询）

QueryVo.java：

``` java
public class QueryVo {
	private User user;
	private List<Integer> ids; //传递多个用户 id
    
     get()/set()方法，此处省略......
}
```

映射文件 UserMapper.xml 中添加：

``` xml
<select id="findUserByIds" parameterType="cn.itheima.pojo.QueryVo" resultType="cn.itheima.pojo.User">
    select * from user

    select * from user
    <where>
        <if test="ids != null">
            <!-- 
    foreach:循环传入的集合参数
    collection:传入的集合的变量名称
    item:每次循环将循环出的数据放入这个变量中
    open:循环开始拼接的字符串
    close:循环结束拼接的字符串
    separator:循环中拼接的分隔符
     -->
            <foreach collection="ids" item="id" open="id in (" close=")" separator=",">
                #{id}
            </foreach>
        </if>
    </where>
</select>
```

接口 UserMapper.java 中添加：`public List<User> findUserByIds(QueryVo vo);`

部分测试代码：

``` java
QueryVo vo = new QueryVo();
List<Integer> ids = new ArrayList<Integer>();
ids.add(1);
ids.add(16);
ids.add(28);
ids.add(22);
vo.setIds(ids);

List<User> list = mapper.findUserByIds(vo);
System.out.println(list);
```

控制台输出：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-3580023.jpg)



## 三、关联查询

商品订单模型：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-15514946.jpg)

### 3.1 一对一关联：

案例：查询所有订单信息，关联查询下单用户信息。（一个订单对应一个用户，一对一的关系）

SQL 语句：

``` sql
select a.*, b.id uid, username, birthday, sex, address from orders a, user b where a.user_id = b.id
```

数据库输出结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-89548968.jpg)



**方式一：（自动映射）**

定义 PO 类，PO 类中应该包括上边 sql 查询出来的所有字段，CustomOrders.java：

``` java
public class CustomOrders extends Orders{
    private int uid;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址
	
    get/set 方法已省略......
}
```

OrdersCustom 类继承 Orders 类后 OrdersCustom 类包括了 Orders 类的所有字段，只需要定义用户的信息字段即可。

映射文件 UserMapper.xml 文件添加：

``` xml
<!-- 一对一:自动映射 -->
<select id="findOrdersAndUser1" resultType="cn.itheima.pojo.CustomOrders">
    select a.*, b.id uid, username, birthday, sex, address 
    from orders a, user b 
    where a.user_id = b.id
</select>
```

小结：定义专门的 po 类作为输出类型，其中定义了 sql 查询结果集所有的字段。此方法较为简单，企业中使用普遍。

**方式二：使用 resultMap，定义专门的resultMap用于映射一对一查询结果。（手动映射）**

在 Orders 类中加入 User 属性，user 属性中用于存储关联查询的用户信息，因为订单关联查询用户是一对一关系，所以这里使用单个 User 对象存储关联查询的用户信息。

Order.java：

``` java
public class Orders {
    private Integer id;
    private Integer userId;
    private String number;
    private Date createtime;
    private String note; 
    private User user;  //加入User属性
    
    get/set 方法已省略......
}
```

映射文件 UserMapper.xml 文件添加：

``` xml
<!-- 一对一:手动映射 -->
<!-- 
 id:resultMap的唯一标识
 type:将查询出的数据放入这个指定的对象中
 注意:手动映射需要指定数据库中表的字段名与java中pojo类的属性名称的对应关系
  -->
<resultMap type="cn.itheima.pojo.Orders" id="orderAndUserResultMap">
    <!-- id标签指定主键字段对应关系
      column:列,数据库中的字段名称
      property:属性,java中pojo中的属性名称
   -->
    <id column="id" property="id"/>

    <!-- result:标签指定非主键字段的对应关系 -->
    <result column="user_id" property="userId"/>
    <result column="number" property="number"/>
    <result column="createtime" property="createtime"/>
    <result column="note" property="note"/>

    <!-- 这个标签指定单个对象的对应关系,表示进行关联查询单条记录
      property:指定将关联查询的结果存储在Orders中的user属性中
      javaType:表示关联查询的结果类型，这里即 user属性的类型
  	-->
    <association property="user" javaType="cn.itheima.pojo.User">
        <id column="uid" property="id"/> <!--表示查询结果的uid列对应关联对象的id属性 -->
        <result column="username" property="username"/>
        <result column="birthday" property="birthday"/>
        <result column="sex" property="sex"/>
        <result column="address" property="address"/>
    </association>
</resultMap>

<!-- 这里 resultMap 指定 orderAndUserResultMap。-->
<select id="findOrdersAndUser2" resultMap="orderAndUserResultMap">
    select a.*, b.id uid, username, birthday, sex, address 
    from orders a, user b 
    where a.user_id = b.id
</select>
```

测试：

``` java
@Test
public void testFindOrdersAnduUser2() throws Exception{
    SqlSession openSession = factory.openSession();
    //通过getMapper方法来实例化接口
    UserMapper mapper = openSession.getMapper(UserMapper.class);

    List<Orders> list = mapper.findOrdersAndUser2();
    System.out.println(list);
}
```

### 3.2 一对多关联

案例：查询所有用户信息及用户关联的订单信息。（一个用户可以有多个订单，一对多的关系）

SQL 语句：

``` sql
select a.*, b.id oid ,user_id, number, createtime from user a, orders b where a.id = b.user_id
```

查询结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-87147709.jpg)

User.java：

``` java
public class User {
	private int id;
	private String username;// 用户姓名
	private String sex;// 性别
	private Date birthday;// 生日
	private String address;// 地址
	
	private List<Orders> ordersList;
 
    get/set 方法已省略......
}
```

在映射文件 UserMappering.xml 中添加：

``` xml
<resultMap type="cn.itheima.pojo.User" id="userAndOrdersResultMap">
    <id column="id" property="id"/>
    <result column="username" property="username"/>
    <result column="birthday" property="birthday"/>
    <result column="sex" property="sex"/>
    <result column="address" property="address"/>

    <!-- 指定对应的集合对象关系映射
          property:将数据放入User对象中的ordersList属性中
          ofType:指定ordersList属性的泛型类型,此处可以使用别名
   -->
    <collection property="ordersList" ofType="cn.itheima.pojo.Orders">
        <id column="oid" property="id"/>
        <result column="user_id" property="userId"/>
        <result column="number" property="number"/>
        <result column="createtime" property="createtime"/>
    </collection>
</resultMap>

<select id="findUserAndOrders" resultMap="userAndOrdersResultMap">
    select a.*, b.id oid ,user_id, number, createtime 
    from user a, orders b where a.id = b.user_id
</select>
```



## 四、Mybatis整合Spring

1、整合思路

> - SqlSessionFactory 对象应该放到 spring 容器中作为单例存在。
> - 传统 dao 的开发方式中，应该从 spring 容器中获得 sqlsession 对象。
> - Mapper 代理形式中，应该从 spring 容器中直接获得 mapper 的代理对象。
> - 数据库的连接以及数据库连接池事务管理都交给 spring 容器来完成。

2、整合需要的包

> 1、spring 的 jar 包
> 2、Mybatis 的 jar 包
> 3、Spring+mybatis 的整合包。
> 4、Mysql 的数据库驱动 jar 包。
> 5、数据库连接池的 jar 包。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-54483258.jpg)

3、整合的步骤：

> 第一步：创建一个 java 工程。
>
> 第二步：导入 jar 包。（上面提到的jar包）
>
> 第三步：mybatis 的配置文件 sqlmapConfig.xml
>
> 第四步：编写 Spring 的配置文件
>
> 1. 数据库连接及连接池
> 2. 事务管理（暂时可以不配置）
> 3. sqlsessionFactory 对象，配置到 spring 容器中
> 4. mapeer 代理对象或者是 dao 实现类配置到 spring 容器中。
>
> 第五步：编写 dao 或者 mapper 文件
>
> 第六步：测试。

SqlMapConfig.xml：

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases> 
        <package name="cn.itheima.pojo"/>
    </typeAliases>
    <mappers>
        <mapper resource="User.xml"/>
    </mappers>
</configuration>
```

ApplicationContext.xml：

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

    <!-- 整合Sql会话工厂归spring管理 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 指定mybatis核心配置文件 -->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"></property>
        <!-- 指定会话工厂使用的数据源 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 
          方式1：配置原生Dao实现  	
          注意:class必须指定Dao实现类的全路径名称
     -->
    <bean id="userDao" class="cn.itheima.dao.UserDaoImpl">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
    </bean>

    <!-- 方式2：Mapper接口代理实现 -->
    <!-- 	<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean"> -->
    <!-- 配置mapper接口的全路径名称 -->
    <!-- 		<property name="mapperInterface" value="cn.itheima.mapper.UserMapper"></property> -->
    <!-- 		<property name="sqlSessionFactory" ref="sqlSessionFactory"></property> -->
    <!-- 	</bean> -->

    <!-- 方式3：使用包扫描的方式批量引入Mapper
     	扫描后引用的时候可以使用类名,首字母小写.
      -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定要扫描的包的全路径名称,如果有多个包用英文状态下的逗号分隔 -->
        <property name="basePackage" value="cn.itheima.mapper"></property>
    </bean>
</beans>
```

db.properties：

``` xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
jdbc.username=root
jdbc.password=admin
```

### 4.1 传统dao的开发方式

接口+实现类来完成。需要 dao 实现类需要继承 SqlsessionDaoSupport 类。

UserDao.java：

``` java
public interface UserDao {
	public User findUserById(Integer id);	
	public List<User> findUserByUserName(String userName);
}
```

UserDaoImpl.java：

``` java
public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {
	@Override
	public User findUserById(Integer id) {
		//sqlSesion是线程不安全的,所以它的最佳使用范围在方法体内
		SqlSession openSession = this.getSqlSession();
		User user = openSession.selectOne("test.findUserById", id);
		//整合后会话归spring管理,所以不需要手动关闭.
		//openSession.close();
		return user;
	}

	@Override
	public List<User> findUserByUserName(String userName) {
		SqlSession openSession = this.getSqlSession();
		List<User> list = openSession.selectList("test.findUserByUserName", userName);
		return list;
	}	
}
```

> **解释：**对比之前写的用构造方法注入会话工厂的方式
>
> ``` java
> public class UserDaoImpl implements UserDao {
>
> 	private SqlSessionFactory sqlSessionFactory;
>
> 	//通过构造方法注入
> 	public UserDaoImpl(SqlSessionFactory sqlSessionFactory) {
> 		this.sqlSessionFactory = sqlSessionFactory;
> 	}
>     @Override
> 	public User findUserById(Integer id) {
> 		//sqlSesion是线程不安全的,所以它的最佳使用范围在方法体内
> 		SqlSession openSession = sqlSessionFactory.openSession();
> 		User user = openSession.selectOne("test.findUserById", id);
> 		return user;
> 	}
>     其他代码已省略。。。。。
> }
> ```
>
> 可以注意到这里 UserDaoImpl 采用了继承 SqlSessionDaoSupport 的方式，为什么可以这样呢？查看 Spring 配置文件 ApplicationContext.xml 可以注意到：
>
> ``` xml
> <bean id="userDao" class="cn.itheima.dao.UserDaoImpl">
>     <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
> </bean>
> ```
>
> 交给 Spring 管理了。（如果不是很清晰，在回顾下 Spring 相关知识）

配置 dao，把 dao 实现类配置到 spring 容器中：

``` xml
<!-- 
      配置原生Dao实现  	
      注意:class必须指定Dao实现类的全路径名称
 -->
<bean id="userDao" class="cn.itheima.dao.UserDaoImpl">
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```

测试代码 UserDaoTest.java：

``` java
public class UserDaoTest {

    private ApplicationContext applicatonContext;

    @Before
    public void setUp() throws Exception{
        String configLocation = "classpath:ApplicationContext.xml";
        applicatonContext = new ClassPathXmlApplicationContext(configLocation);
    }

    @Test
    public void testFindUserById() throws Exception{
        //获取UserDao对象, getBean中的字符串是在ApplicationContext.xml中声明的
        UserDao userDao = (UserDao)applicatonContext.getBean("userDao");

        User user = userDao.findUserById(1);
        System.out.println(user);
    }
}
```

注：整合后会话归 Spring 管理，所以不需要手动关闭。

### 4.2 Mapper代理形式开发dao

开发 Mapper 文件：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-30189891.jpg)

如图，使用 Mapper 代理方式开发 dao 就不需要 dao 层接口和实现类了。其中 UserMapper.java 如下：（都是一些接口，这种方式就没有实现类了）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-99279414.jpg)

配置 Mapper 的代理，在 ApplicationContext 中添加：

``` xml
<!-- Mapper接口代理实现 -->
<bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean"> 
    <!-- 配置mapper接口的全路径名称 -->
    <property name="mapperInterface" value="cn.itheima.mapper.UserMapper"></property> 
    <property name="sqlSessionFactory" ref="sqlSessionFactory"></property> 
</bean> 
```

测试代码 UserMapperTest.java：

``` java
public class UserMapperTest {
    private ApplicationContext applicatonContext;

    @Before
    public void setUp() throws Exception{
        String configLocation = "classpath:ApplicationContext.xml";
        applicatonContext = new ClassPathXmlApplicationContext(configLocation);
    }

    @Test
    public void testFindUserById() throws Exception{
        UserMapper userMapper = (UserMapper)applicatonContext.getBean("userMapper");

        User user = userMapper.selectByPrimaryKey(1);
        System.out.println(user);
    }
```

### 4.3 扫描包形式配置mapper

如果很多 mapper，如上那样配置 mapper 方式，单个配置非常麻烦，一般不会这样做。一般采用包扫描方式批量引入 mapper。在 Spring 配置文件 ApplicationContext 中添加：

``` xml
<!-- 使用包扫描的方式批量引入Mapper
 	扫描后引用的时候可以使用类名,首字母小写.
  -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!-- 指定要扫描的包的全路径名称,如果有多个包用英文状态下的逗号分隔 -->
    <property name="basePackage" value="cn.itheima.mapper"></property>
</bean>
```



## 五、总结

``` xml
1. 输入映射(就是映射文件中可以传入哪些参数类型)
	1)基本类型
	2)pojo类型
	3)Vo类型
2. 输出映射(返回的结果集可以有哪些类型)
	1)基本类型
	2)pojo类型
	3)List类型
3. 动态sql:动态的拼接sql语句,因为sql中where条件有可能多也有可能少
	1)where:可以自动添加where关键字,还可以去掉第一个条件的and关键字
	2)if:判断传入的参数是否为空
	3)foreach:循环遍历传入的集合参数
	4)sql:封装查询条件,以达到重用的目的

4. 对单个对象的映射关系:
	1)自动关联(偷懒的办法):可以自定义一个大而全的pojo类,然后自动映射其实是根据数据库总的字段名称和
		pojo中的属性名称对应.
	2)手动关联: 需要指定数据库中表的字段名称和java的pojo类中的属性名称的对应关系.
		使用association标签
5. 对集合对象的映射关系
	只能使用手动映射:指定表中字段名称和pojo中属性名称的对应关系
		使用collection标签
6. spring和mybatis整合
	整合后会话工厂都归spring管理
	1)原生Dao实现:
		需要在spring配置文件中指定dao实现类
		dao实现类需要继承SqlSessionDaoSupport超类
		在dao实现类中不要手动关闭会话,不要自己提交事务.
	2)Mapper接口代理实现:
		在spring配置文件中可以使用包扫描的方式,一次性的将所有mapper加载

7. 逆向工程:自动生成Pojo类,还可以自动生成Mapper接口和映射文件
	注意:生成的方式是追加而不是覆盖,所以不可以重复生成,重复生成的文件有问题.
		如果想重复生成将原来生成的文件删除
```









