
# 1. 什么是JDBC

**jdbc：  **Java 数据库连接(Java Database Connectivity)，是 Java 语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法。JDBC 也是 Sun Microsystems的商标。JDBC 是面向关系型数据库的。

Java 操作数据库。jdbc 是 oracle 公司指定的一套规范（一套接口）

驱动：jdbc 的实现类，由数据库厂商提供。我们就可以通过一套规范操作不同的数据库了(多态)

**jdbc作用：** 连接数据库、发送 sql 语句、处理结果....   

以下是架构图，它显示了驱动程序管理器相对于 JDBC 驱动程序和 Java 应用程序的位置。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-8-93585493.jpg)

*参考： [JDBC 指南](http://wiki.jikexueyuan.com/project/jdbc)*



# 2. jdbc操作步骤

``` xml
1.数据库和表
2.创建一个项目
3.导入驱动jar包
4.编码:
    注册驱动 
    获取连接
    编写sql
    创建预编译的语句执行者
    设置参数
    执行sql
    处理结果
    释放资源
```

初始化数据库和表：

``` html
CREATE DATABASE day07;
USE day07;	

create table category(
    cid varchar(20) primary key,
    cname varchar(20)
);

insert into category values('c001','电器');
insert into category values('c002','服饰');
insert into category values('c003','化妆品');
insert into category values('c004','书籍');
```

①IDE打开之后：

1. 修改字符集 `utf-8`
2. 新建 java 项目
3. 使用的 jdk 为自己的 jdk，不用使用内置 jdk

②使用 junit 单元测试，要求：

1. 方法是 public void xxx(){}
2. 在方法上添加 `@Test`
3. 在 `@Test` 按下 ctrl+1(快速锁定错误)
4. 在方法上右键 `run as --> junit` 就可以执行方法了.

代码：

``` java
@Test
public void f2() throws Exception{
    //注册驱动
    //Class.forName("com.mysql.jdbc.Driver");
    DriverManager.registerDriver(new Driver());
    //获取连接 		ctrl+o 整理包
    Connection conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/day07", "root", "1234");
    //编写sql
    String  sql="select * from category";
    //创建语句执行者
    PreparedStatement st=conn.prepareStatement(sql);
    //设置参数

    //执行sql
    ResultSet rs=st.executeQuery();
    //处理结果
    while(rs.next()){
        System.out.println(rs.getString("cid")+"::"+rs.getString("cname"));
    }
    //释放资源.
    rs.close();
    st.close();
    conn.close();
}
```

但以上写的代码不好，其实很多地方可以进行封装。

***封装①*** 	

通过下面内容，关于 jdbc 的 api 讲解，、可以知道，这里采用`Class.forName("com.mysql.jdbc.Driver");` 该方式注册驱动，可以避免驱动注册两次，所以可以这样写，如下：

``` java
public class JdbcUtils_ {
	// 获取连接
	public static Connection getConnection() throws ClassNotFoundException, SQLException {
		// 注册驱动   ctrl+shift+f格式化代码
		Class.forName("com.mysql.jdbc.Driver");
		// 获取连接 ctrl+o 整理包
		Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/day07", "root", "1234");
		return conn;
	}
	/**
	 * 释放资源
	 * @param conn 连接
	 * @param st 语句执行者
	 * @param rs 结果集
	 */
	public static void closeResource(Connection conn, Statement st, ResultSet rs) {
		closeResultSet(rs);
		closeStatement(st);
		closeConn(conn);
	}
	/**
	 * 释放连接
	 * @param conn 连接
	 */
	public static void closeConn(Connection conn){
		if(conn!=null){
			try {
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			conn=null;
		}	
	}	
	/**
	 * 释放语句执行者
	 * @param st 语句执行者
	 */
	public static void closeStatement(Statement st){
		if(st!=null){
			try {
				st.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			st=null;
		}
	}
	/**
	 * 释放结果集
	 * @param rs 结果集
	 */
	public static void closeResultSet(ResultSet rs){
		if(rs!=null){
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			rs=null;
		}
	}
}
```

然后就可以这样使用了：

``` java
//插入一条数据
@Test
public void f3(){
    Connection conn=null;
    ResultSet rs=null;
    PreparedStatement st=null;
    try {
        //获取连接
        conn=JdbcUtils.getConnection();
        //编写sql
        String sql="insert into  category values(?,?)";
        //获取语句执行者
        st=conn.prepareStatement(sql);
        //设置参数
        st.setString(1, "c006");
        st.setString(2, "户外");
        //执行sql 
        int i=st.executeUpdate();
        //处理结果
        if(i==1){
            System.out.println("success");
        }else{
            System.out.println("fail");
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        //释放资源
        JdbcUtils.closeResource(conn, st, rs);
    }
}
```

***封装②***	

代码如下：

``` java
public class JdbcUtils {
	static final String DRIVERCLASS;
	static final String URL;
	static final String USER;
	static final String PASSWORD;
	static {
		// 获取ResourceBundle ctrl+2 l
		ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
		// 获取指定的内容
		DRIVERCLASS = bundle.getString("driverClass");
		URL = bundle.getString("url");
		USER = bundle.getString("user");
		PASSWORD = bundle.getString("password");
	}
	static {
		// 注册驱动 ctrl+shift+f格式化代码
		try {
			Class.forName(DRIVERCLASS);
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
	// 获取连接
	public static Connection getConnection() throws SQLException {
		// 获取连接 ctrl+o 整理包
		return  DriverManager.getConnection(URL, USER, PASSWORD);
	}
	//
	public static void closeResource(Connection conn, Statement st, ResultSet rs) {
		closeResultSet(rs);
		closeStatement(st);
		closeConn(conn);
	}
	//释放连接
	public static void closeConn(Connection conn) {
		if (conn != null) {
			try {
				conn.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			conn = null;
		}

	}
	//释放语句执行者
	public static void closeStatement(Statement st) {
		if (st != null) {
			try {
				st.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			st = null;
		}

	}
	//释放结果集
	public static void closeResultSet(ResultSet rs) {
		if (rs != null) {
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			rs = null;
		}
	}
}
```

可以看到为了避免`封装①`方式中数据库等信息写死，使用了`ResourceBundle` 来读取配置文件 `jdbc.properties`设置的数据库等信息

其中 `jdbc.properties` 文件内容如下：

``` xml
driverClass=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/day07
user=root
password=1234
```

注，常见的配置文件格式：

1. properties 文件，里面内容的格式 `key=value`
2. xml 文件

若我们的配置文件为 properties，并且放在 src 目录下，我们可以通过 ResourceBundle 工具快速获取里面的配置信息。使用步骤：（`封装②`方式的代码有）

1. 获取 ResourceBundle 对象:  `static ResourceBundle getBundle("文件名称不带后缀名") `
2. 通过 ResourceBundle 对象获取配置信息: `String getString(String key)` 通过执行key获取指定的value



# 2. jdbc-api详解

所有的包，都是 `java.sql` 或者 `javax.sql`

### DriverManager

DriverManager：管理了一组 jdbc 的操作，类。常用方法：

①注册驱动（了解）：`static void registerDriver(Driver driver)` ，通过查看 `com.mysql.jdbc.Driver` 的源码 有如下代码

``` java
static {
    try {
        java.sql.DriverManager.registerDriver(new Driver());
    } catch (SQLException E) {
        throw new RuntimeException("Can't register driver!");
    }
}
```

注：其中 `java.sql.DriverManager.registerDriver(new Driver());`这段代码我们其实已经写过。

驱动注册了两次，我们只需要将静态代码块执行一次，类被加载到内存中会执行静态代码块，并且只执行一次。现在只需要将类加载到内存中即可：

- 方式1：`Class.forName("全限定名");//包名+类名   com.mysql.jdbc.Driver`
- 方式2：`类名.class;`
- 方式3：`对象.getClass();`

②获取连接（掌握）：`static Connection getConnection(String url, String user, String password)`

- 参数1：告诉我们连接什么类型的数据库及连接那个数据库，格式 `协议:数据库类型:子协议 参数`

  例如：

  - mysql：`jdbc:mysql://localhost:3306/数据库名称`
  - oracle：`jdbc:oracle:thin@localhost:1521@实例`

- 参数2：账户名 root

- 参数3：密码

### Driver: java.sql 

**(1) Connection：连接；接口**

常用方法：

获取语句执行者：

- (了解)`Statement createStatement()` : 获取普通的语句执行者；会出现 sql 注入问题（了解下 sql 注入：[sql注入](https://baike.baidu.com/item/SQL%E6%B3%A8%E5%85%A5)）
- `PreparedStatement prepareStatement(String sql)` ：获取预编译语句执行者
- (了解)`CallableStatement prepareCall(String sql)`：获取调用存储过程的语句执行者

了解：

- `setAutoCommit(false) `: 手动开启事务
- `commit()`: 提交事务
- `rollback()`: 事务回滚

(2) Statement: 语句执行者；接口

(3) PreparedStatement: 预编译语句执行者；接口

常用方法：

- 设置参数：`setXxx (int 第几个问号, Object 实际参数);`  常见的方法：`setInt`、`setString`、`setObject`

- 执行 sql：

  ① `ResultSet executeQuery()`：执行 r 语句 返回值：结果集 

  ② `int executeUpdate()`：执行 cud 语句 返回值：影响的行数

(4) ResultSet：结果集；接口

执行查询语句之后返回的结果。

常见方法：

- boolean next()：判断是否有下一条记录，若有返回 true 且将光标移到下一行，若没有呢则返回 false。光标一开始处于第一条记录的上面。

- 获取具体内容：`getXxx(int或string)` 若参数为 int：第几列；若参数为string: 列名(字段名)。例如获取 cname 的内容可以通过`getString(2)` 或 `getString("cname")`方法。

  常用方法：`geeInt`、`getString` 也可以获取int值、`getObject`可以获取任意



# 4. 连接池

## 连接池介绍

我们在使用 jdbc 的时候，每操作一次，都需要获取连接(创建)，用完之后把连接释放掉了(销毁)。这样肯定不行，假设网站一天 10 万访问量，数据库服务器就需要创建 10 万次连接，极大的浪费数据库的资源，并且极易造成数据库服务器内存溢出、拓机。（摘自网上）

我们可以使用**连接池**来优化 jdbc 操作。连接池更多介绍参考网上资料。

连接池概述：管理数据库的连接。作用：提高项目的性能。就是在连接池初始化的时候存入一定数量的连接，用的时候通过方法获取，不用的时候归还连接即可。

所有的连接池**必须实现**一个接口 `javax.sql.DataSource` 接口

- 获取连接方法：`Connection getConnection() `

- 归还连接的方法就是以前的释放资源的方法，调用 `connection.close();`

  注：这里 close() 方法原来用于关闭，现在用于归还。怎么做到的呢？这里用到了**方法的增强**。

方法增强的方式：

1. 继承；
2. 装饰者模式（静态代理）；
3. 动态代理

**装饰者模式：**

装饰者模式使用步骤：

1. 装饰者和被装饰者实现同一个接口或者继承同一个类
2. 装饰者中要有被装饰者的引用
3. 对需要增强的方法进行加强
4. 对不需要加强的方法调用原来方法

来试试自定义一个连接池：

``` java
//简易的连接池
public class MyDataSource {
    static LinkedList<Connection> pool=new LinkedList<>();
    static{
        //初始化的时候 需要放入3个连接
        for (int i = 0; i < 3; i++) {
            try {
                Connection conn = JdbcUtils.getConnection();
                pool.addLast(conn);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    //从连接池中获取连接
    public static Connection getConnection(){
        //获取连接的时候需要判断list是否为空
        if(pool.isEmpty()){
            //在添加2个连接进去
            for (int i = 0; i < 3; i++) {
                try {
                    Connection conn = JdbcUtils.getConnection();
                    pool.addLast(conn);
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        System.out.println("从池中获取一个连接");
        return pool.removeFirst();
    }
    //归还连接的方法
    public static void addBack(Connection conn){
        //将conn放入到list的最后面即可
        pool.addLast(conn);
        System.out.println("连接已归还");
    }
}
```

常见的连接池：

- DBCP
- C3P0

## DBCP连接池（理解）

apache 组织。使用步骤：

第一步：导入 jar 包(commons-dbcp-1.4.jar和commons-pool-1.5.6.jar)

第二步：使用 api（两种方式）

a. 硬编码

``` java
//创建连接池
BasicDataSource ds = new BasicDataSource();

//配置信息
ds.setDriverClassName("com.mysql.jdbc.Driver");
ds.setUrl("jdbc:mysql:///day07");
ds.setUsername("root");
ds.setPassword("1234");
```

b. 配置文件

``` java
实现编写一个properties文件
//存放配置文件
Properties prop = new Properties();
prop.load(new FileInputStream("src/dbcp.properties"));
//设置
//prop.setProperty("driverClassName", "com.mysql.jdbc.Driver");

//创建连接池
DataSource ds = new BasicDataSourceFactory().createDataSource(prop);
```

dbcp 代码示例：

``` java
public class DbcpDemo {
	@Test
	//硬编码
	public void f1() throws Exception{
		//创建连接池
		BasicDataSource ds = new BasicDataSource();
		
		//配置信息
		ds.setDriverClassName("com.mysql.jdbc.Driver");
		ds.setUrl("jdbc:mysql:///day07");
		ds.setUsername("root");
		ds.setPassword("1234");
		
		Connection conn=ds.getConnection();
		String sql="insert into category values(?,?);";
		PreparedStatement st=conn.prepareStatement(sql);
		
		//设置参数
		st.setString(1, "c011");
		st.setString(2, "饮料");
		
		int i = st.executeUpdate();
		System.out.println(i);
		JdbcUtils.closeResource(conn, st, null);
	}
	
	@Test
	public void f2() throws Exception{
		//存放配置文件
		Properties prop = new Properties();
		prop.load(new FileInputStream("src/dbcp.properties"));
		//设置
		//prop.setProperty("driverClassName", "com.mysql.jdbc.Driver");
		
		//创建连接池
		DataSource ds = new BasicDataSourceFactory().createDataSource(prop);
		
		Connection conn=ds.getConnection();
		String sql="insert into category values(?,?);";
		PreparedStatement st=conn.prepareStatement(sql);
		
		//设置参数
		st.setString(1, "c012");
		st.setString(2, "饮料1");	
		int i = st.executeUpdate();
		System.out.println(i);
		JdbcUtils.closeResource(conn, st, null);
	}
}
```

## C3P0连接池

Hibernate 和 Spring 使用是 c3p0，有自动回收空闲连接的功能。

使用步骤：

1. 导入 jar 包(c3p0-0.9.1.2.jar)

2. 使用 api（两种方式）

   a.硬编码(不推荐) ：`new ComboPooledDataSource()`

   b.配置文件

   配置文件的名称: `c3p0.properties` 或者 `c3p0-config.xml`
   配置文件的路径: src 下

   ``` java
   //编码只需要一句话
   new ComboPooledDataSource()//使用默认的配置
   new ComboPooledDataSource(String configName)//使用命名的配置 若配置的名字找不到,使用默认的配置
   ```

c3p0 代码示例：

``` java
public class C3p0Demo {
    @Test
    //硬编码
    public void f1() throws Exception{
        ComboPooledDataSource ds = new ComboPooledDataSource();

        //设置基本参数
        ds.setDriverClass("com.mysql.jdbc.Driver");
        ds.setJdbcUrl("jdbc:mysql:///day07");
        ds.setUser("root");
        ds.setPassword("1234");

        Connection conn=ds.getConnection();
        String sql="insert into category values(?,?);";
        PreparedStatement st=conn.prepareStatement(sql);

        //设置参数
        st.setString(1, "c013");
        st.setString(2, "毒药");

        int i = st.executeUpdate();
        System.out.println(i);
        JdbcUtils.closeResource(conn, st, null);
    }

    @Test
    public void f2() throws Exception{
        //ComboPooledDataSource ds = new ComboPooledDataSource();
        ComboPooledDataSource ds =new ComboPooledDataSource("itcast12321");//若查找不到命名的配置 使用默认的配置
        Connection conn=ds.getConnection();
        String sql="insert into category values(?,?);";
        PreparedStatement st=conn.prepareStatement(sql);

        //设置参数
        st.setString(1, "c124");
        st.setString(2, "解药");

        int i = st.executeUpdate();
        System.out.println(i);
        JdbcUtils.closeResource(conn, st, null);
    }
}
```



# 5. 使用dbutils完成curd操作

使用原生的 jdbc 方式可以完成 curd 操作，但也可以进一步简化 jdbc 的使用，可以使用 dbutils，它是什么呢？它是 apache 组织的一个工具类，是一个 jdbc 框架，对传统操作数据库的类进行二次封装，可以把结果集转化成 List，更方便我们使用。

使用步骤：

1. 导入 jar 包(commons-dbutils-1.4.jar)

2. 创建一个 queryrunner 类

   queryrunner 作用: 操作 sql 语句；构造方法 `new QueryRunner(Datasource ds);`

3. 编写 sql

4. 执行 sql：如 `query(..)` 执行r操作、`update(...)` 执行 cud 操作

**核心类或接口**

- QueryRunner：类名

    > 作用: 操作sql语句；构造器: `new QueryRunner(Datasource ds);`
    >
    > 注意: 底层帮我们创建连接,创建语句执行者 ,释放资源.
    >
    > 常用方法: `query(..):`、`update(..):`		

- DbUtils：释放资源，控制事务，类

  - `closeQuietly(conn)`: 内部处理了异常
  - `commitAndClose(Connection conn)`: 提交事务并释放连接
  	 ....			

- ResultSetHandler：封装结果集；接口

  ``` xml
  ArrayHandler, ArrayListHandler, BeanHandler, BeanListHandler, ColumnListHandler, KeyedHandler, MapHandler, MapListHandler, ScalarHandler
  ```

  ``` xml
  (了解)ArrayHandler, 将查询结果的第一条记录封装成数组,返回
  (了解)ArrayListHandler, 将查询结果的每一条记录封装成数组,将每一个数组放入list中返回
  ★★BeanHandler, 将查询结果的第一条记录封装成指定的bean对象,返回
  ★★BeanListHandler, 将查询结果的每一条记录封装成指定的bean对象,将每一个bean对象放入list中 返回.
  (了解)ColumnListHandler, 将查询结果的指定一列放入list中返回 
  (了解)MapHandler, 将查询结果的第一条记录封装成map,字段名作为key,值为value 返回
  ★MapListHandler, 将查询结果的每一条记录封装map集合,将每一个map集合放入list中返回
  ★ScalarHandler,针对于聚合函数 例如:count(*) 返回的是一个Long值
  ```

代码示例：

``` java
public class CURDDemo {
	@Test
	public void insert() throws SQLException{
		//1.创建queryrunner类
		QueryRunner qr = new QueryRunner(DataSourceUtils.getDataSource());	
		//2.编写sql
		String sql="insert into category values(?,?)";	
		//3.执行sql
		qr.update(sql, "c201","厨房电器");
	}
	
	@Test
	public void update() throws SQLException{
		QueryRunner qr=new QueryRunner(DataSourceUtils.getDataSource());
		String sql="update category set cname = ? where cid = ?";
		qr.update(sql, "达电器","c000");
	}
    
    //.....
    @Test
	public void arrayHandler() throws SQLException{
		QueryRunner qr=new QueryRunner(DataSourceUtils.getDataSource());
		String sql="select * from category";
		Object[] query = qr.query(sql, new ArrayHandler());
		System.out.println(Arrays.toString(query));
	}
	
	@Test
	public void arrayListHandler() throws SQLException{
		QueryRunner qr=new QueryRunner(DataSourceUtils.getDataSource());
		String sql="select * from category";
		List<Object[]> list = qr.query(sql, new ArrayListHandler());
		for (Object[] obj : list) {
			System.out.println(Arrays.toString(obj));
		}
	}
}
```



# 6. 装饰者模式

静态代理使用步骤：

1. 要求装饰者和被装饰者实现同一个接口或者继承同一个类
2. 要求装饰者要有被装饰者的引用
3. 对需要加强的方法进行加强
4. 对不需要加强的方法调用原来的方法

Car 类：

``` java
public interface Car {
	void run();
	void stop();
}
```

QQ 类：（被装饰者 ，被代理类）

``` java
public class QQ implements Car {

	@Override
	public void run() {
		System.out.println("qq在跑");
	}

	@Override
	public void stop() {
		System.out.println("刹得住车");
	}
}
```

CarWarp 类：（装饰者，代理类  //这里改装了 QQ 的 `run( )` 方法，`stop( )` 方法没改装）
``` java
public class CarWarp implements Car {
    private Car car;

    public CarWarp(Car car){
        this.car=car;
    }

    @Override  
    public void run() {
        System.out.println("加上电池");
        System.out.println("我终于可以5秒破百了..");
    }
    @Override
    public void stop() {
        car.stop();
    }
}
```

TTT 类：（测试）

``` java
public class TTT {
	public static void main(String[] args) {
		//如果只有 QQ 这个，正常
        QQ qq = new QQ();
		/*qq.run();
		qq.stop();*/
		
        //现在需要改装，加强需要加强的方法
		CarWarp warp = new CarWarp(qq);
		warp.run();		//这里调用了加强的方法
		warp.stop();	//这里的方法没加强
	}
}
```

TTT 运行结果：

``` html
加上电池
我终于可以5秒破百了..
刹得住车
```



关于对以上的理解，我试着在捋一遍：首先 Car 车是个接口，存在 QQ 车（被装饰者）需要改装（即需要对方法加强），比如对 QQ 车的 `run( )` 方法加强。这里使用装饰者 CarWarp 可以达到目的。

我们可以再看一个代码例子；

``` java
//静态代理模式
//接口
interface ClothFactory {
    void productCloth();
}

// 被代理类
class NikeColthFactory implements ClothFactory {

    @Override
    public void productCloth() {
        System.out.println("Nike工厂生产了一批衣服");
    }
}

// 代理类
class ProxFactory implements ClothFactory {
    ClothFactory cf;

    public ProxFactory(ClothFactory cf) {
        this.cf = cf;
    }

    @Override
    public void productCloth() {
        System.out.println("代理类开始执行，收代理费$1000");
        cf.productCloth();
    }
}

public class TestClothProduct {
    public static void main(String[] args) {
        NikeColthFactory nike = new NikeColthFactory();// 创建被代理类的对象
        ProxFactory prxoy = new ProxFactory(nike);// 创建代理类的对象
        prxoy.productCloth();
    }
}
```

运行结果：

``` xml
代理类开始执行，收代理费$1000
Nike工厂生产了一批衣服
```

