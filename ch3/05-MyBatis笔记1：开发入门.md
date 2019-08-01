
## 前言

Mybatis 第一天学习大纲：

``` xml
1. Mybatis 的介绍
2. Mybatis 的入门
   1. 使用 jbdc 操作数据库存在的问题
   2. Mybatis 的架构
   3. Mybatis 的入门程序
3. Dao 的开发方法
   1. 原始的 dao 的开发方法
   2. 动态代理的方法
4. SqlMapConfig.xml 文件说明
```



## 一、Mybatis 的介绍 

MyBatis 本是 apache 的一个开源项目 iBatis，2010年这个项目由 apache software foundation 迁移到了 google code，并且改名为 MyBatis。2013年11月迁移到 Github。  MyBatis 是一个优秀的持久层框架，它对 jdbc 的操作数据库的过程进行封装，使开发者只需要关注 SQL 本身，而不需要花费精力去处理例如注册驱动、创建connection、创建 statement、手动设置参数、结果集检索等 jdbc 繁杂的过程代码。

Mybatis 通过 xml 或注解的方式将要执行的各种 statement（statement、preparedStatement、CallableStatement）配置起来，并通过 java 对象和 statement中 的 sql 进行映射生成最终执行的 sql 语句，最后由 Mybatis 框架执行 sql 并将结果映射成 java 对象并返回。



## 二、Mybatis 的入门

在学习之前我们先了解什么是 pojo、domain、po、vo、bo 及区别。

- pojo：不按 mvc 分层,只是 java bean 有一些属性，还有 get、set方法
- domain：不按 mvc 分层,只是 java bean 有一些属性，还有 get、set 方法
- po：用在持久层,还可以再增加或者修改的时候,从页面直接传入 action 中，它里面的 java bean 类名等于表名，属性名等于表的字段名，还有对应的 get、set 方法


- vo：view object 表现层对象，主要用于在高级查询中从页面接收传过来的各种参数，好处是扩展性强
- bo：用在 servie 层，现在企业基本不用

相关资料：[Java中PO、DO、DTO、 VO、 BO、POJO 、DAO、TO的概念](https://www.jianshu.com/p/08b8007f1c84)

这些 po、vo、bo、pojo 可以用在各种层面吗？

> 可以，也就是 po 用在表现层，vo 用在持久层不报错，因为都是普通的 java bean 没有语法错误。但是在企业最好不要混着用，因为这些都是设计的原则，混着用比较乱，不利于代码维护。

### 2.1 使用 jbdc 操作数据库存在的问题 

**1、创建mysql数据库**

先导入创建数据库的 sql 脚本导入到数据库中。

mybatis.sql：

``` sql
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `orders`
-- ----------------------------
DROP TABLE IF EXISTS `orders`;
CREATE TABLE `orders` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL COMMENT '下单用户id',
  `number` varchar(32) NOT NULL COMMENT '订单号',
  `createtime` datetime NOT NULL COMMENT '创建订单时间',
  `note` varchar(100) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `FK_orders_1` (`user_id`),
  CONSTRAINT `FK_orders_id` FOREIGN KEY (`user_id`) REFERENCES `user` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of orders
-- ----------------------------
INSERT INTO `orders` VALUES ('3', '1', '1000010', '2015-02-04 13:22:35', null);
INSERT INTO `orders` VALUES ('4', '1', '1000011', '2015-02-03 13:22:41', null);
INSERT INTO `orders` VALUES ('5', '10', '1000012', '2015-02-12 16:13:23', null);

-- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `birthday` date DEFAULT NULL COMMENT '生日',
  `sex` char(1) DEFAULT NULL COMMENT '性别',
  `address` varchar(256) DEFAULT NULL COMMENT '地址',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=27 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', '王五', null, '2', null);
INSERT INTO `user` VALUES ('10', '张三', '2014-07-10', '1', '北京市');
INSERT INTO `user` VALUES ('16', '张小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('22', '陈小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('24', '张三丰', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('25', '陈小明', null, '1', '河南郑州');
INSERT INTO `user` VALUES ('26', '王五', null, null, null);
```

表如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-50827747.jpg)

orders 表数据：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-54071592.jpg)

user 表数据：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-19078216.jpg)

**2、创建工程**

因为使用的为 MySQL 数据库，使用 JDBC 操作数据库之前得先导入 mysql 的数据库驱动包 `mysql-connector-java-5.1.7-bin.jar`。

**3、JDBC 编程步骤**

1. 加载数据库驱动

2. 创建并获取数据库连接

3. 创建 jdbc statement 对象

4. 设置 sql 语句

5. 设置 sql 语句中的参数（使用 preparedStatement）

6. 通过 statement 执行 sql 并获取结果

7. 对 sql 执行结果进行解析处理

8. 释放资源（resultSet、preparedstatement、connection）


**4、JDBC 代码** 

``` java
public static void main(String[] args) {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    ResultSet resultSet = null;	
    try {
        //加载数据库驱动
        Class.forName("com.mysql.jdbc.Driver");	
        //通过驱动管理类获取数据库链接
        connection =  DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "root");
        //定义sql语句 ?表示占位符
        String sql = "select * from user where username = ?";
        //获取预处理statement
        preparedStatement = connection.prepareStatement(sql);
        //设置参数，第一个参数为sql语句中参数的序号（从1开始），第二个参数为设置的参数值
        preparedStatement.setString(1, "王五");
        //向数据库发出sql执行查询，查询出结果集
        resultSet =  preparedStatement.executeQuery();
        //遍历查询结果集
        while(resultSet.next()){
            System.out.println(resultSet.getString("id")+"  "+resultSet.getString("username"));
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        //释放资源
        if(resultSet!=null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(preparedStatement!=null){
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(connection!=null){
            try {
                connection.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

    }

}
```

上边使用 jdbc 的原始方法（未经封装）实现了查询数据库表记录的操作。

5、jdbc 问题总结如下

> ① 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。
>
> ② Sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变 java 代码。
>
> ③ 使用 preparedStatement 向占有位符号传参数存在硬编码，因为 sql 语句的 where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
>
> ④  对结果集解析存在硬编码（查询列名），sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成 pojo 对象解析比较方便。

### 2.2 Mybatis 的架构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-80822540.jpg)

1、mybatis配置

SqlMapConfig.xml，此文件作为 mybatis 的全局配置文件，配置了 mybatis 的运行环境等信息。

mapper.xml 文件即 sql 映射文件，文件中配置了操作数据库的 sql 语句。此文件需要在 SqlMapConfig.xml 中加载。

2、通过 mybatis 环境等配置信息构造 SqlSessionFactory 即**会话工厂**

3、**由会话工厂创建 sqlSession 即会话，操作数据库需要通过 sqlSession 进行**

4、mybatis 底层自定义了 Executor 执行器接口操作数据库，Executor 接口有两个实现，一个是基本执行器、一个是缓存执行器。

5、Mapped Statement 也是 mybatis 一个底层封装对象，它包装了 mybatis 配置信息及 sql 映射信息等。 mapper.xml 文件中一个 sql 对应一个Mapped Statement对象，sql 的 id 即是 Mapped statement 的 id。

6、Mapped Statement 对 sql 执行输入参数进行定义，包括 HashMap、基本类型、pojo，Executor 通过 MappedStatement 在执行 sql 前将输入的 java 对象映射至 sql 中，输入参数映射就是 jdbc 编程中对 preparedStatement 设置参数。

7、Mapped Statement 对 sq l执行输出结果进行定义，包括 HashMap、基本类型、pojo，Executor 通过 MappedStatement 在执行 sql 后将输出结果映射至 java 对象中，输出结果映射过程相当于 jdbc 编程中对结果的解析处理过程。

补充：

> - Hibernate 是个 ORM 框架，但 Mybatis 不是，没有真正意义上的映射关系，相比 Hibernate 简单多，它就是对 jdbc 轻量级的封装。相关资料：
>   - [ORM框架简介](https://blog.csdn.net/papima/article/details/78219000)
>   - [MyBatis不是完整的ORM框架？ - 知乎](https://www.zhihu.com/question/39454008)
> - 在 Hibernate 都不用写 SQL 语句，但在 Mybatis 你需要写 SQL 语句，这样有个好处，假如公司有数据库高手，那么你就可以直接发送 mapper.xml 文件叫 TA 帮忙修改或优化 SQL 语句，再发给你来。

### 2.3 Mybatis 的入门程序

mybaits 的代码由 GitHub 管理，地址：https://github.com/mybatis/mybatis-3/releases

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-99054114.jpg)

> mybatis-3.4.6.jar----mybatis的核心包
>
> lib----mybatis的依赖包
>
> mybatis-3.4.6.pdf----mybatis使用手册

#### 2.3.1 以案例驱动学习

需求，实现以下功能：

``` xml
根据用户id查询一个用户信息
根据用户名称模糊查询用户信息列表
添加用户
删除用户
更新用户
```

**工程搭建：**

1、Eclipse 下创建项目

2、引入 jar 包（加入 mybatis 核心包、依赖包、数据驱动包）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-47966857.jpg)

3、在 classpath 下创建 `log4j.properties`，如下：

``` xml
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

> mybatis 默认使用 log4j 作为输出日志信息。关于 classpath 具体指哪个路径，参考网上资料：[java项目里classpath具体指哪儿个路径](https://blog.csdn.net/u011095110/article/details/76152952)。
>
> 关于 classpath 路径我的理解：（以项目案例来说）
>
> 如下工程目录：
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-98405182.jpg)
>
> 在本地磁盘下目录是这样的：![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-42298661.jpg)
>
> 另外：在工程的 Build Path 中可以看到三个文件夹都有加入到 Build Path 中。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-68532651.jpg)
>
> 并且可以看到最后三个文件下生成的 class 字节码文件和配置文件都存在如上图设置的 mybatis0523/bin 目录下：
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-13525227.jpg)
>
> 所以 classpath 路径，应该是指 Build Path 下设置的输出文件夹路径，也即加入到 Build Path 的目录。（有时间我再实践验证...）

4、在 classpath 下创建 `SqlMapConfig.xml`，如下：

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 和spring整合后 environments配置将废除-->
    <environments default="development">
        <environment id="development">
            <!-- 使用jdbc事务管理-->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8" />
                <property name="username" value="root" />
                <property name="password" value="root" />
            </dataSource>
        </environment>
    </environments>	
    
</configuration>
```

> `SqlMapConfig.xml` 是 mybatis 核心配置文件，上边文件的配置内容为数据源、事务管理。

5、新建 PO 类，PO 类作为 mybatis 进行 sql 映射使用，PO 类通常与数据库表对应，`User.java` 如下：

``` java
public class User {
    private int id;
    private String username;// 用户姓名
    private String sex;// 性别
    private Date birthday;// 生日
    private String address;// 地址

    get/set 方法（略......）
```

6、在 classpath 下的 config 目录下创建 sql 映射文件 `Users.xml`：

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="test">
</mapper>
```

> namespace ：命名空间，用于隔离 sql 语句，后面会讲另一层非常重要的作用。

7、加载映射文件。mybatis 框架需要加载映射文件，将 `Users.xml` 添加到 `SqlMapConfig.xml`，如下：

``` xml
<mappers>
    <mapper resource="User.xml"/>
</mappers>
```

**① 根据 id 查询用户信息**

在映射文件 User.xml 中添加：

``` xml
<!-- 根据id获取用户信息 -->
<select id="findUserById" parameterType="java.lang.Integer" resultType="cn.itheima.pojo.User">
    select * from user where id=#{id}
</select>
```

> id：sql 语句唯一标识
>
> parameterType：指定传入参数类型，定义输入到 sql 中的映射类型
>
> resultType：定义返回结果映射类型
>
> `#{}`占位符：起到占位作用，如果传入的是基本类型（string、long、double、int、boolean、float 等），那么`#{}`中的变量名称可以随意写

测试程序：

``` java
@Test
public void testFindUserById() throws Exception{
    String resource = "SqlMapConfig.xml";
    //通过流将核心配置文件读取进来
    InputStream inputStream = Resources.getResourceAsStream(resource);
    //通过核心配置文件输入流来创建会话工厂
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(inputStream);
    //通过工厂创建会话
    SqlSession openSession = factory.openSession();
    //第一个参数:所调用的sql语句= namespace+.+sql的ID
    User user = openSession.selectOne("test.findUserById", 1);
    System.out.println(user);
    openSession.close();
}
```

运行结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-32847645.jpg)

**② 根据用户名查询用户信息**

在映射文件 User.xml 中添加：

``` xml
<select id="findUserByUserName" parameterType="java.lang.String" resultType="cn.itheima.pojo.User">
    select * from user where username like '%${value}%'
</select>
```

> 如果返回结果为集合，可以调用 selectList() 方法，这个方法返回的结果就是一个集合，所以映射文件中应该配置成集合泛型的类型。
>
> `${}`拼接符：字符串原样拼接，如果传入的参数是基本类型（string、long、double、int、boolean、float 等），那么`${}`中的变量名称必须是 value。
>
> 注意：拼接符有 sql 注入的风险，所以慎重使用。（关于什么是 sql 注入参考网上资料，如：[sql注入实例分析](https://www.jianshu.com/p/928751b7fbe2)）

测试代码中主要的一行：`List<User> list = openSession.selectList("test.findUserByUserName", "王");`

---------------------------------------------- 小结 ------------------------------------------------------

**#{} 和 ${}：**

> `#{} `表示一个占位符号，通过`#{}`可以实现 preparedStatement 向占位符中设置值，自动进行 java 类型和 jdbc 类型转换，`#{}`可以有效防止 sql 注入。`#{}`可以接收简单类型值或 pojo 属性值。如果 parameterType 传输单个简单类型值，`#{}` 括号中可以是 value 或其它名称。
>
> `${}`表示拼接 sql 串，通过`${}`可以将 parameterType 传入的内容拼接在 sql 中且不进行 jdbc 类型转换， `${} `可以接收简单类型值或 pojo 属性值，如果 parameterType 传输单个简单类型值，`${}`括号中只能是value。

**parameterType和resultType：**

> parameterType：指定输入参数类型，mybatis 通过 ognl 从输入对象中获取参数值拼接在 sql 中。
>
> resultType：指定输出结果类型，mybatis 将 sql 查询结果的一行记录数据映射为 resultType 指定类型的对象。

**selectOne和selectList：**

> `selectOne()`：查询一条记录，如果使用 selectOne 查询多条记录则抛出异常：
>
> ``` xml
> org.apache.ibatis.exceptions.TooManyResultsException: Expected one result (or null) to be returned by selectOne(), but found: 3
> 	at org.apache.ibatis.session.defaults.DefaultSqlSession.selectOne(DefaultSqlSession.java:70)
> ```
>
> `selectList()`：可以查询一条或多条记录。

/////////////////////////////////////////////////////////////////////////////////////////////

**③ 添加用户** 

在映射文件 User.xml 中添加：

``` xml
<!-- 未添加自增主键，可以看到最后的运行结果 id 为 0。 -->
<!--<insert id="insertUser" parameterType="cn.itheima.pojo.User" >
    insert into user (username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
</insert> 
-->
<insert id="insertUser" parameterType="cn.itheima.pojo.User" >
    <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
        select LAST_INSERT_ID()
    </selectKey>
    insert into user (username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
</insert>
```

> `#{}`：如果传入的是 pojo 类型，那么`#{}`中的变量名称必须是 pojo 中对应的`属性.属性.属性.....`
>
> 如果要返回数据库自增主键：可以使用 select LAST_INSERT_ID()
>
> -  `select LAST_INSERT_ID()`：是 mysql 的函数，返回 auto_increment 自增列新记录 id 值
> - keyProperty：将返回的主键放入传入参数的 Id 中保存（这里即放入 User 对象的 id 属性）
> - order：selectKey 的执行顺序，是相对于 insert 语句来说，由于 mysql 的自增原理是执行完 insert 语句之后才将主键生成，所以这里 selectKey 的执行顺序为 AFTER（在 insert 前执行是 BEFORE）
> - resultType：id 的类型，也就是 keyproperties 中属性的类型
>
> 如果 mysql 使用 uuid 实现主键，则先把数据库主键 id 类型改为字符串 string 类型。这里需要增加通过 select uuid() 得到 uuid 值
>
> ``` xml
> <insert  id="insertUser" parameterType="cn.itcast.mybatis.po.User">
>     <selectKey resultType="java.lang.String" order="BEFORE" 
>                keyProperty="id">
>         select uuid()
>     </selectKey>
>     insert into user(id,username,birthday,sex,address) 
>     values(#{id},#{username},#{birthday},#{sex},#{address})
> </insert>
> ```
>
> 注意：这里使用的 order 是 “BEFORE”，因为先得生成一个 uuid 再把该值放入传入的 User 的 id 属性
>
> 

部分测试代码：

``` java
User user = new User();
user.setUsername("赵四");
user.setBirthday(new Date());
user.setSex("1");
user.setAddress("北京昌平");
System.out.println("====" + user.getId());

openSession.insert("test.insertUser", user);
//提交事务(mybatis会自动开启事务,但是它不知道何时提交,所以需要手动提交事务)
openSession.commit();
```

这里解释一点：为什么测试代码中未看到开启事务却需要提交事务？这是因为 Mybatis 会自动开启事务，但不知道何时提交，所以需要手动提交事务。

**④ 删除用户**

在映射文件  User.xml 中添加：

``` xml
<delete id="delUserById" parameterType="int">
    delete from user where id=#{id}
</delete>
```

测试代码中主要的一行：`openSession.delete("test.delUserById", 29);`

**⑤ 更新用户** 

在映射文件 User.xml 中添加：

``` xml
<update id="updateUserById" parameterType="cn.itheima.pojo.User">
    update user set username=#{username} where id=#{id}
</update>
```

部分测试代码：

``` java
User user = new User();
user.setId(28);
user.setUsername("王麻子");
openSession.update("test.updateUserById", user);
```

### 2.4 SqlSession的使用范围

SqlSession 中封装了对数据库的操作，如：查询、插入、更新、删除等。

通过 SqlSessionFactory（会话工厂） 创建 SqlSession（会话），而 SqlSessionFactory 是通过 SqlSessionFactoryBuilder 进行创建。

**SqlSessionFactoryBuilder：**

> SqlSessionFactoryBuilder 用于创建 SqlSessionFacoty，SqlSessionFacoty 一旦创建完成就不需要SqlSessionFactoryBuilder 了，因为 SqlSession 是通过 SqlSessionFactory 生产，所以可以将SqlSessionFactoryBuilder 当成一个工具类使用，最佳使用范围是方法范围即方法体内局部变量。

**SqlSessionFactory：**

> SqlSessionFactory 是一个接口，接口中定义了 openSession 的不同重载方法，SqlSessionFactory 的最佳使用范围是整个应用运行期间，一旦创建后可以重复使用，通常以单例模式管理 SqlSessionFactory。

**SqlSession：**

> SqlSession 是一个面向用户的接口， sqlSession 中定义了数据库操作方法。
>
> 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不能共享使用，它也是线程不安全的。因此最佳的范围是请求或方法范围。绝对不能将SqlSession实例的引用放在一个类的静态字段或实例字段中。
>
> 打开一个 SqlSession，使用完毕就要关闭它。通常把这个关闭操作放到 finally 块中以确保每次都能执行关闭。如下：
>
> ``` java
> SqlSession session = sqlSessionFactory.openSession();
> try {
>     // do work
> } finally {
>     session.close();
> }
>
> ```



## 三、Dao 的开发方法

### 3.1 原始 Dao 的开发方法

原始 Dao 开发方法需要程序员编写 Dao 接口和 Dao 实现类。

UserDao.java：

``` java
public interface UserDao {
    public User findUserById(Integer id);
    public List<User> findUserByUserName(String userName);
}
```

UserDaoImpl.java：

``` java
public class UserDaoImpl implements UserDao {

    private SqlSessionFactory sqlSessionFactory;

    //通过构造方法注入
    public UserDaoImpl(SqlSessionFactory sqlSessionFactory) {
        this.sqlSessionFactory = sqlSessionFactory;
    }

    @Override
    public User findUserById(Integer id) {
        //sqlSesion是线程不安全的,所以它的最佳使用范围在方法体内
        SqlSession openSession = sqlSessionFactory.openSession();
        User user = openSession.selectOne("test.findUserById", id);
        return user;
    }

    @Override
    public List<User> findUserByUserName(String userName) {
        SqlSession openSession = sqlSessionFactory.openSession();
        List<User> list = openSession.selectList("test.findUserByUserName", userName);
        return list;
    }
}
```

问题：为什么不能把 SqlSession 也弄成公共的，即定义在方法体外？因为如果弄成公共的，如果来一个 User 对象，同一时间点，有人进行更新操作、有人进行删除操作，那数据就乱了。所以需要放在方法体内，用完了就销毁了，其他人用不了。

测试代码 UserDaoTest.java：

``` java
public class UserDaoTest {

    private SqlSessionFactory factory;

    //作用:在测试方法前执行这个方法
    @Before
    public void setUp() throws Exception{
        String resource = "SqlMapConfig.xml";
        //通过流将核心配置文件读取进来
        InputStream inputStream = Resources.getResourceAsStream(resource);
        //通过核心配置文件输入流来创建会话工厂
        factory = new SqlSessionFactoryBuilder().build(inputStream);
    }

    @Test
    public void testFindUserById() throws Exception{
        //将初始化好的工厂注入到实现类中
        UserDao userDao = new UserDaoImpl(factory);
        User user = userDao.findUserById(1);
        System.out.println(user);
    }

    @Test
    public void testFindUserByUserName () throws Exception{
        UserDao userDao = new UserDaoImpl(factory);
        List<User> list = userDao.findUserByUserName("王");
        System.out.println(list);
    }
}
```

### 3.2 动态代理方式（Mapper接口开发方法）

**1、开发规范**

Mapper 接口开发方法只需要程序员编写 Mapper 接口（相当于 Dao 接口），由 Mybatis 框架根据接口定义创建接口的动态代理对象，代理对象的方法体同上边 Dao 接口实现类方法。

Mapper接口开发需要遵循以下规范：

1.  Mapper.xml 文件中的 namespace 与mapper 接口的类路径相同。
2. Mapper 接口方法名和 Mapper.xml 中定义的每个 statement 的 id 相同
3. Mapper 接口方法的输入参数类型和 mapper.xml 中定义的每个 sql 的 parameterType 的类型相同
4. Mapper 接口方法的输出参数类型和 mapper.xml 中定义的每个 sql 的 resultType 的类型相同

**2、UserMapper.xml（映射文件）**

定义 mapper 映射文件 UserMapper.xml（内容同 Users.xml），需要修改 namespace 的值为 UserMapper 接口路径。将 UserMapper.xml 放在classpath 下 mapper目录下。

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 
mapper接口代理实现编写规则:
 1. 映射文件中namespace要等于接口的全路径名称
 2. 映射文件中sql语句id要等于接口的方法名称
 3. 映射文件中传入参数类型要等于接口方法的传入参数类型
 4. 映射文件中返回结果集类型要等于接口方法的返回值类型
 -->
<mapper namespace="cn.itheima.mapper.UserMapper">

    <!-- 
 id:sql语句唯一标识
 parameterType:指定传入参数类型
 resultType:返回结果集类型
 #{}占位符:起到占位作用,如果传入的是基本类型(string,long,double,int,boolean,float等),那么#{}中的变量名称可以随意写.
  -->
    <select id="findUserById" parameterType="int" resultType="cn.itheima.pojo.User">
        select * from user where id=#{id}
    </select>

    <!-- 
 如果返回结果为集合,可以调用selectList方法,这个方法返回的结果就是一个集合,所以映射文件中应该配置成集合泛型的类型
 ${}拼接符:字符串原样拼接,如果传入的参数是基本类型(string,long,double,int,boolean,float等),那么${}中的变量名称必须是value
 注意:拼接符有sql注入的风险,所以慎重使用
  -->
    <select id="findUserByUserName" parameterType="string" resultType="user">
        select * from user where username like '%${value}%'
    </select>

    <!-- 
 #{}:如果传入的是pojo类型,那么#{}中的变量名称必须是pojo中对应的属性.属性.属性.....
 如果要返回数据库自增主键:可以使用select LAST_INSERT_ID()
  -->
    <insert id="insertUser" parameterType="cn.itheima.pojo.User" >
        <!-- 执行 select LAST_INSERT_ID()数据库函数,返回自增的主键
  keyProperty:将返回的主键放入传入参数的Id中保存.
  order:当前函数相对于insert语句的执行顺序,在insert前执行是before,在insert后执行是AFTER
  resultType:id的类型,也就是keyproperties中属性的类型
  -->
        <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
            select LAST_INSERT_ID()
        </selectKey>
        insert into user (username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
    </insert>
</mapper>
```

3、UserMapper.java（接口文件）

``` java
public interface UserMapper {
    public User findUserById(Integer id);

    //动态代理形势中,如果返回结果集问List,那么mybatis会在生成实现类的使用会自动调用selectList方法
    public List<User> findUserByUserName(String userName);

    public void insertUser(User user);
}
```

接口定义有如下特点：

1. Mapper 接口方法名和 Mapper.xml 中定义的 statement 的 id 相同
2. Mapper 接口方法的输入参数类型和 mapper.xml 中定义的 statement 的 parameterType 的类型相同
3. Mapper 接口方法的输出参数类型和 mapper.xml 中定义的 statement 的 resultType 的类型相同

**4、加载 UserMapper.xml 文件**

修改 SqlMapConfig.xml 文件：

``` xml
<!-- 加载映射文件 -->
<!-- 使用class属性引入接口的全路径名称:
		使用规则:
			1. 接口的名称和映射文件名称除扩展名外要完全相同
			2. 接口和映射文件要放在同一个目录下
 -->
<mappers>
    <mapper resource="cn.itheima.mapper.UserMapper"/>
</mappers>
```

补充：如果有很多 mapper 映射文件要加载呢，则每个都写上很麻烦，使用包扫描的方式批量引入Mapper 接口，如下：

``` xml
<!-- 使用包扫描的方式批量引入Mapper接口 
    使用规则:
    1. 接口的名称和映射文件名称除扩展名外要完全相同
    2. 接口和映射文件要放在同一个目录下
  -->

<package name="cn.itheima.mapper"/>
```

**5、UserMapperTest.java（测试）**

``` java
public class UserMapperTest {
    private SqlSessionFactory factory;

    //作用:在测试方法前执行这个方法
    @Before
    public void setUp() throws Exception{
        String resource = "SqlMapConfig.xml";
        //通过流将核心配置文件读取进来
        InputStream inputStream = Resources.getResourceAsStream(resource);
        //通过核心配置文件输入流来创建会话工厂
        factory = new SqlSessionFactoryBuilder().build(inputStream);
    }

    @Test
    public void testFindUserById() throws Exception{
        SqlSession openSession = factory.openSession();
        //通过getMapper方法来实例化接口
        UserMapper mapper = openSession.getMapper(UserMapper.class);

        User user = mapper.findUserById(1);
        System.out.println(user);
    }

    @Test
    public void testFindUserByUserName() throws Exception{
        SqlSession openSession = factory.openSession();
        //通过getMapper方法来实例化接口
        UserMapper mapper = openSession.getMapper(UserMapper.class);

        List<User> list = mapper.findUserByUserName("王");

        System.out.println(list);
    }

    @Test
    public void testInsertUser() throws Exception{
        SqlSession openSession = factory.openSession();
        //通过getMapper方法来实例化接口
        UserMapper mapper = openSession.getMapper(UserMapper.class);

        User user = new User();
        user.setUsername("老王");
        user.setSex("1");
        user.setBirthday(new Date());
        user.setAddress("北京昌平");

        mapper.insertUser(user);

        openSession.commit();
    }
}
```

补充：

selectOne和selectList

> 动态代理对象调用 `sqlSession.selectOne()` 和 `sqlSession.selectList()` 是根据 mapper 接口方法的返回值决定，如果返回 list 则调用 selectList 方法，如果返回单个对象则调用 selectOne 方法。

namespace

> mybatis 官方推荐使用 mapper 代理方法开发 mapper 接口，程序员不用编写 mapper 接口实现类，使用 mapper 代理方法时，输入参数可以使用 pojo 包装对象或 map 对象，保证 dao 的通用性。



## 四、SqlMapConfig.xml 文件说明

**1、配置内容**

SqlMapConfig.xml 中配置的内容和顺序如下：

``` xml
properties（属性）
settings（全局配置参数）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境集合属性对象）
	environment（环境子属性对象）
		transactionManager（事务管理）
		dataSource（数据源）
mappers（映射器）
```

**2、properties（属性）**

SqlMapConfig.xml 可以引用 java 属性文件中的配置信息如下：

在 classpath下定义 `db.properties` 文件：

``` xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
jdbc.username=root
jdbc.password=root
```

SqlMapConfig.xml 引用如下：

``` xml
<properties resource="db.properties"></properties>
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${jdbc.driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </dataSource>
    </environment>
</environments>
```

注意，MyBatis 将按照下面的顺序来加载属性

1. 在 properties 元素体内定义的属性首先被读取。
2. 然后会读取 properties 元素中 resource 或 url 加载的属性，它会覆盖已读取的同名属性。

**3、typeAliases（类型别名）**

mybatis 支持的别名：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-53504460.jpg)

类型别名什么意思呢？在如下映射文件中，parameterType 的值之前写的是`java.lang.Integer`：

``` xml
<select id="findUserById" parameterType="java.lang.Integer" resultType="cn.itheima.pojo.User">
    select * from user where id=#{id}
</select>
```

但其实可以写为 Mybatis 支持的别名 int：

``` xml
<select id="findUserById" parameterType="int" resultType="cn.itheima.pojo.User">
    select * from user where id=#{id}
</select>
```

如果碰到`cn.itheima.pojo.User`呢？ ---> 可以自定义别名。如下：

在 SqlMapConfig.xml 中配置：

``` xml
<typeAliases>
    <!-- 单个别名定义 
		type:类的全路劲名称
		alias:别名
	-->
    <typeAlias alias="user" type="cn.itcast.mybatis.po.User"/>
</typeAliases>
```

如果有很多这样类需要别名定义呢？可以使用包扫描方式：

``` xml
<typeAliases> 
    <!-- 使用包扫描的方式批量定义别名，如下，会自动扫描cn.itheima.pojo下的所有pojo
  定义后别名等于类名,不区分大小写,但是建议按照java命名规则来,首字母小写,以后每个单词的首字母大写（驼峰式命名）
  -->
    <package name="cn.itheima.pojo"/>
    <!--<package name="其它包"/> -->
</typeAliases>
```

注：**①定义后别名等于类名并且不区分大小写**；②这里所说不区分大小写什么意思呢？ 如下例

``` xml
<select id="findUserById" parameterType="java.lang.Integer" resultType="UserR">
    select * from user where id=#{id}
</select>
```

resultType 的值写为 UseR 也没问题的。

**4、mappers（映射器）**

Mapper 配置的几种方法：

- `<mapper resource=" " />`

  > 使用相对于类路径的资源
  >
  > 如：`<mapperresource="sqlmap/User.xml" />`

- `1.1.1  <mapper class=" " />`

  > 使用mapper接口类路径
  >
  > 如：`<mapperclass="cn.itcast.mybatis.mapper.UserMapper"/>`
  >
  > 注意：此种方法要求 mapper 接口名称和 mapper 映射文件名称相同，且放在同一个目录中。

- `<package name=""/>`

  > 注册指定包下的所有 mapper 接口
  >
  > 如：`<packagename="cn.itcast.mybatis.mapper"/>`
  >
  > 注意：此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中。
  >
  > ```xml
  > <!-- 使用包扫描的方式批量引入Mapper接口 
  >     使用规则:
  >     1. 接口的名称和映射文件名称除扩展名外要完全相同
  >     2. 接口和映射文件要放在同一个目录下
  >   -->
  > <package name="cn.itheima.mapper"/>
  > ```



## 五、小结

### 5.1 Mybatis解决jdbc编程的问题

1、数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。

解决：在 SqlMapConfig.xml 中配置数据链接池，使用连接池管理数据库链接。

2、sql 语句写在代码中造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变 java 代码。

解决：将 Sql 语句配置在 `XXXXmapper.xml` 文件中与 java 代码分离。

3、向 sql 语句传参数麻烦，因为 sql 语句的 where 条件不一定，可能多也可能少，占位符需要和参数一一对应。

解决：Mybatis 自动将 java 对象映射至 sql 语句，通过 statement 中的 parameterType 定义输入参数的类型。

4、对结果集解析麻烦，sql 变化导致解析代码变化，且解析前需要遍历，如果能将数据库记录封装成 pojo 对象解析比较方便。

解决：Mybatis 自动将 sql 执行结果映射至 java 对象，通过 statement 中的 resultType 定义输出结果的类型。

### 5.2 mybatis与hibernate不同

Mybatis 和 Hibernate 不同，它不完全是一个 ORM 框架，因为 MyBatis 需要程序员自己编写 sql 语句，不过mybatis 可以通过 XML 或注解方式灵活配置要运行的 sql 语句，并将 java 对象和 sql 语句映射生成最终执行的 sql，最后将 sql 执行的结果再映射生成 java 对象。

Mybatis 学习门槛低，简单易学，程序员直接编写原生态 sql，可严格控制 sql 执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，例如互联网软件、企业运营类软件等，因为这类软件需求变化频繁，一但需求变化要求成果输出迅速。但是灵活的前提是 mybatis 无法做到数据库无关性，如果需要实现支持多种数据库的软件则需要自定义多套 sql 映射文件，工作量大。

Hibernate 对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件（例如需求固定的定制化软件）如果用 Hibernate 开发可以节省很多代码，提高效率。但是 Hibernate 的学习门槛高，要精通门槛更高，而且怎么设计 O/R 映射，在性能和对象模型之间如何权衡，以及怎样用好 Hibernate 需要具有很强的经验和能力才行。

总之，按照用户的需求在有限的资源环境下只要能做出维护性、扩展性良好的软件架构都是好架构，所以框架只有适合才是最好。 

小总结：

``` xml
1. mybatis是一个持久层框架, 作用是跟数据库交互完成增删改查
2.原生Dao实现(需要接口和实现类)
4.动态代理方式(只需要接口)
	mapper接口代理实现编写规则:
	1) 映射文件中namespace要等于接口的全路径名称
	2) 映射文件中sql语句id要等于接口的方法名称
	3) 映射文件中传入参数类型要等于接口方法的传入参数类型
	4) 映射文件中返回结果集类型要等于接口方法的返回值类型

5. #{}占位符:占位
	如果传入的是基本类型,那么#{}中的变量名称可以随意写
	如果传入的参数是pojo类型,那么#{}中的变量名称必须是pojo中的属性.属性.属性...

6. ${}拼接符:字符串原样拼接
	如果传入的是基本类型,那么${}中的变量名必须是value
	如果传入的参数是pojo类型,那么${}中的变量名称必须是pojo中的属性.属性.属性...
	注意:使用拼接符有可能造成sql注入,在页面输入的时候可以加入校验,不可输入sql关键字,不可输入空格
7. 映射文件:
	1)传入参数类型通过parameterType属性指定
	2)返回结果集类型通过resultType属性指定
8. hibernate和mybatis区别:
	hibernate:它是一个标准的orm框架,比较重量级,学习成本高.
		优点:高度封装,使用起来不用写sql,开发的时候,会减低开发周期.
		缺点:sql语句无法优化
		应用场景:oa(办公自动化系统), erp(企业的流程系统)等,还有一些政府项目,
			总的来说,在用于量不大,并发量小的时候使用.
	mybatis:它不是一个orm框架, 它是对jdbc的轻量级封装, 学习成本低,比较简单
		有点:学习成本低, sql语句可以优化, 执行效率高,速度快
		缺点:编码量较大,会拖慢开发周期
		应用场景: 互联网项目,比如电商,P2p等
		     总的来说是用户量较大,并发高的项目.
```

