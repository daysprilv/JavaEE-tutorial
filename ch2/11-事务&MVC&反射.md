
# 1. MVC

MVC 思想：

- servlet --> 缺点：生成 html 内容太麻烦

- jsp --> 缺点：阅读起来不方便,维护比较困难

- jsp + javabean：（jsp 的 model1）

  ``` xml
  jsp:接受请求,展示数据
  javabean:和数据打交道 
  ```

- jsp + javabean + servlet：（jsp 的 model2）

  ``` xml
  jsp:展示数据
  javabean:和数据打交道
  servlet:接受请求,处理业务逻辑		
  ```

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-4379277.jpg)

MVC： 就是将业务逻辑、代码、显示相分离的一种思想

- M：model，模型，作用：主要是封装数据,封装对数据的访问
- V：view，视图，作用：主要是用来展示数据 一般是 jsp 担任的
- C：ctrl，控制，作用：接受请求,找到相应的 javabean 完成业务逻辑

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-60246070.jpg)

MVC 是模型(model)－视图(view)－控制器(controller)的缩写，是一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

jsp 设计模式1 - model1：javabean+jsp

``` xml
<!-- 接受值 -->
<jsp:useBean id="u" class="com.itheima.domain.User"></jsp:useBean><!--相当于  User u=new User()-->
<jsp:setProperty property="name" name="u"/><!--相当于  u.setName(...)-->
<jsp:setProperty property="password" name="u"/>

<!-- 打印值-->
<jsp:getProperty property="name" name="u"/>
```

**反射：**

``` xml
1.获取class对象
    方式1:
    	Class clazz=Class.forName("全限定名")
    方式2:
    	Class clazz=类名.class;
    方式3:
    	Class clazz=对象.getClass;
2.可以获取对应类的构造方法(了解)
    Constructor con = clazz.getConstructor(Class .. paramClass);
    Person p = (Person) con.newInstance(参数);
3.可以通过clazz创建一个对象(了解)
	clazz.newInstance();//相当于调用的无参构造器
4.可以通过clazz获取所有的字段 getFiled()(了解中的了解)
5.可以通过clazz获取所有的方法
    Method m = clazz.getMethod("sleep");//获取公共的方法
    Method m = clazz.getDeclaredMethod("sleep");//获取任意的方法

    注意:若是私有的方法 必须让该方法可以访问
    	m.setAccessible(true);
6.Method对象的invoke是有返回值,他的返回值就是目标方法执行的返回值
```

有了 class 对象之后，无所不能。

javabean 在 model2 中使用

``` xml
BeanUtils:可以看作封装数据一个工具类
    使用步骤:
        1.导入jar包
        2.使用BeanUtils.populate(Object bean,Map map);
```

分层：JavaEE 的三层架构

``` xml
web
	作用：展示数据 ----jsp

        -----servlet-------
        接受请求
        找到对应的service,调用方法 完成逻辑操作
        信息生成或者页面跳转
service 业务层
    作用：完成业务操作，调用dao
dao(data access object 数据访问对象)
    作用: 对数据库的curd操作
```



# 2. 事务

就是一件完整的事情，包含多个操作单元，这些操作要么全部成功，要么全部失败。

例如：转账。包含转出操作和转入操作。

## (1) mysql中的事务

mysql 中事务默认是自动提交，一条 sql 语句就是一个事务。

开启手动事务方式：

``` xml
方式1:关闭自动事务.(了解)
	set autocommit = off;
方式2:手动开启一个事务.(理解)
    start transaction;-- 开启一个事务
    commit;-- 事务提交
    rollback;-- 事务回滚
```

扩展：oracle 中事务默认是手动的，必须手动提交才可以。

## (2) java中的事务

``` xml
Connection接口的api:★
    setAutoCommit(false);//手动开启事务
    commit():事务提交
    rollback():事务回滚
		
扩展:了解 Savepoint还原点
    void rollback(Savepoint savepoint) :还原到那个还原点
    Savepoint setSavepoint() :设置还原点
```

代码：

``` java
public class AccountService {
    /**
	 * 转账
	 * @param fromUser 转出方
	 * @param toUser 转入方
	 * @param money 金额
	 * @throws Exception 
	 */
    public void account(String fromUser, String toUser, String money) throws Exception {
        AccountDao dao = new AccountDao();
        Connection conn=null;
        try {
            //0.开启事务
            conn = JdbcUtils.getConnection();
            conn.setAutoCommit(false);

            //1.转出
            dao.accountOut(conn,fromUser,money);

            int i=1/0;

            //2.转入
            dao.accountIn(conn,toUser,money);

            //3.事务提交
            conn.commit();
            JdbcUtils.closeConn(conn);
        } catch (Exception e) {
            e.printStackTrace();

            //事务回滚
            conn.rollback();
            JdbcUtils.closeConn(conn);
            throw e;
        }	
    }
}
```

案例：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-44118142.jpg)

``` xml
步骤分析:
	1.数据库和表
	2.新建一个项目 day1301
	3.导入jar包和工具类
		驱动 jdbcUtils
		c3p0及其配置文件和工具类
		dbutils
	4.新建一个account.jsp 表单
	5.accountservlet:
		接受三个参数
		调用accountservice.account方法完成转账操作
		打印信息
	6.account方法中:
		使用jdbc不考虑事务
		调用dao完成转出操作
		调用dao完成转入操作
	7.dao中
```

**一旦出现异常，钱飞了！！！**

要想避免这事情，必须添加事务，在 service 添加事务，为了保证所有的操作在一个事务中，必须保证使用的是同一个连接。

在 Service 层我们获取了连接，开启了事务。如何 DAO 层使用此连接呢？

``` xml
方法1:
	向下传递参数.注意连接应该在service释放
方法2:
    可以将connection对象绑定当前线程上
    jdk中有一个ThreadLocal类,
    ThreadLocal 实例通常是类中的 private static 字段，
    它们希望将状态与某一个线程（例如，用户 ID 或事务 ID）相关联。
///////////////////////////////
ThreadLocal的方法:
	构造:
        new ThreadLocal()
        set(Object value):将内容和当前线程绑定
        Object get():获取和迪昂前线程绑定的内容
        remove():将当前线程和内容解绑
内部维护了map集合
    map.put(当前线程,内容);
    map.get(当前线程)
    map.remove(当前线程)
```

封装了：`AccountService4tl.java` 

```java
/**
	 * 获取数据源
	 * @return 连接池
	 */
	public static DataSource getDataSource(){
		return ds;
	}
	
	/**
	 * 从当前线程上获取连接
	 * @return 连接
	 * @throws SQLException
	 */
	public static Connection getConnection() throws SQLException{
		Connection conn = tl.get();
		if(conn==null){
			//第一次获取 创建一个连接 和当前的线程绑定
			 conn=ds.getConnection();
			 
			 //绑定
			 tl.set(conn);
		}
		return conn;
	}
	
	
	
	/**
	 * 释放资源
	 * 
	 * @param conn
	 *            连接
	 * @param st
	 *            语句执行者
	 * @param rs
	 *            结果集
	 */
	public static void closeResource(Connection conn, Statement st, ResultSet rs) {
		closeResource(st, rs);
		closeConn(conn);
	}
	
	 
	public static void closeResource(Statement st, ResultSet rs) {
			closeResultSet(rs);
			closeStatement(st);
	}

	/**
	 * 释放连接
	 * 
	 * @param conn
	 *            连接
	 */
	public static void closeConn(Connection conn) {
		if (conn != null) {
			try {
				conn.close();
				//和当前的线程解绑
				tl.remove();
			} catch (SQLException e) {
				e.printStackTrace();
			}
			conn = null;
		}

	}

	/**
	 * 释放语句执行者
	 * 
	 * @param st
	 *            语句执行者
	 */
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

	/**
	 * 释放结果集
	 * 
	 * @param rs
	 *            结果集
	 */
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
	
	/**
	 * 开启事务
	 * @throws SQLException
	 */
	public static void startTransaction() throws SQLException{
		//获取连接//开启事务
		getConnection().setAutoCommit(false);;
	}
	
	/**
	 * 事务提交
	 */
	public static void commitAndClose(){
		try {
			//获取连接
			Connection conn = getConnection();
			//提交事务
			conn.commit();
			//释放资源
			conn.close();
			//解除绑定
			tl.remove();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 事务回滚
	 */
	public static void rollbackAndClose(){
		try {
			//获取连接
			Connection conn = getConnection();
			//事务回滚
			conn.rollback();
			//释放资源
			conn.close();
			//解除绑定
			tl.remove();
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
```

使用：

```java
public class AccountService4DB {
	/**
	 * 转账
	 * @param fromUser 转出方
	 * @param toUser 转入方
	 * @param money 金额
	 * @throws Exception 
	 */
	public void account(String fromUser, String toUser, String money) throws Exception {
		AccountDao4DB dao = new AccountDao4DB();
		try {
			//0.开启事务
			DataSourceUtils.startTransaction();
			
			//1.转出
			dao.accountOut(fromUser,money);
			
			int i=1/0;
			
			//2.转入
			dao.accountIn(toUser,money);
			
			//3.事务提交
			DataSourceUtils.commitAndClose();
		} catch (Exception e) {
			e.printStackTrace();
			DataSourceUtils.rollbackAndClose();
			throw e;
		}

	}
}
```



``` xml
DButils:
	1.创建queryrunner
	2.编写sql
	3.执行sql
QueryRunner:
	构造:
		new QueryRunner(DataSource ds):自动事务
		new QueryRunner():手动事务
	常用方法:
		update(Connection conn,String sql,Object ... params):执行的cud操作
		query(Connection conn....):执行查询操作
	注意:
		一旦使用手动事务,调用方法的时候都需要手动传入connection,并且需要手动关闭连接
```

## (3) 事务的总结

### ①事务的特性(ACID)

- 原子性：事务里面的操作单元不可切割，要么全部成功，要么全部失败
- 一致性：事务执行前后，业务状态和其他业务状态保持一致
- 隔离性：一个事务执行的时候最好不要受到其他事务的影响
- 持久性：一旦事务提交或者回滚，这个状态都要持久化到数据库中

### ②如果不考虑事务的隔离性，引发一些安全性问题

读问题：三类

- 脏读：一个事务读到了另一个事务未提交的数据
- 不可重复读：一个事务读到了另一个事务已经提交(update)的数据，引发一个事务中的多次查询结果不一致
- 虚读(幻读)：一个事务读到了另一个事务已经提交的（insert）数据，导致多次查询的结果不一致

### ③解决读问题

通过设置数据库的隔离级别来避免上面的问题

``` sql
read uncommitted  	读未提交	上面的三个问题都会出现
read committed  	读已提交	可以避免脏读的发生
repeatable read		可重复读	可以避免脏读和不可重复读的发生
serializable		串行化		可以避免所有的问题
```

演示脏读的发生：

``` sql
将数据库的隔离级别设置成 读未提交
	set session transaction isolation level read uncommitted;
查看数据库的隔离级别
	select @@tx_isolation;
```

避免脏读的发生，将隔离级别设置成：读已提交

``` sql
set session transaction isolation level read committed; 不可避免不可重复读的发生
```

避免不可重复读的发生，将隔离级别设置成：可重复读

``` sql
set session transaction isolation level  repeatable read;
```

演示串行化 可以避免所有的问题：

``` sql
set session transaction isolation level  serializable;
锁表操作.
```

四种隔离级别的效率：`read uncommitted`>`read committed`>`repeatable read`>`serializable`

四种隔离级别的安全性：`read uncommitted`<`read committed`<`repeatable read`<`serializable`

开发中绝对不允许脏读发生！

- mysql中默认隔离级别：repeatable read

- oracle中默认隔离级别：read committed

**JDBC 的隔离级别的设置：** Connection 的 api

- `void setTransactionIsolation(int level) `  //level 是常量

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-5699946.jpg)

**使用 DBUtils 的进行事务的管理：**

``` java
QueryRunner：
    构造：
    QueryRunner();
    QueryRunner(DataSource ds);
方法：
    T query(String sql,ResultSetHanlder<T> rsh,Object... params);
    T query(Connection conn,String sql,ResultSetHanlder<T> rsh,Object... params);
    int update(String sql,Object... params);
    int update(Connection conn,String sql,Object... params);

方法分类：
    没有事务：
    QueryRunner(DataSource ds);
    T query(String sql,ResultSetHanlder<T> rsh,Object... params);
    int update(String sql,Object... params);

    有事务：
    QueryRunner();
    T query(Connection conn,String sql,ResultSetHanlder<T> rsh,Object... params);
    int update(Connection conn,String sql,Object... params);
```

### ④演示

**演示脏读的发生：**

``` xml
步骤一：分别开启两个窗口A,B:
步骤二：分别查询两个窗口的隔离级别：
	select @@tx_isolation
步骤三：设置A窗口的隔离级别为read uncommitted
	set session transaction isolation level read uncommitted;
步骤四：在两个窗口中分别开启事务：
	start transaction;
步骤五：在B窗口中转账操作：
	update account set money = money - 1000 where name='守义';
	update account set money = money + 1000 where name='凤儿';
在B窗口中没有提交事务的！！！
步骤六：在A窗口中进行查询：
	已经转账成功!!!(脏读:一个事务中读到了另一个事务未提交的数据)

```

**演示避免脏读，不可重复读发生：**

``` xml
步骤一：分别开启两个窗口A,B:
步骤二：分别查询两个窗口的隔离级别：
	select @@tx_isolation
步骤三：设置A窗口的隔离级别为read committed;
	set session transaction isolation level read committed;
步骤四：在两个窗口中分别开启事务：
	start transaction;
步骤五：在B窗口中完成转账的操作：
	update account set money = money - 1000 where name='守义';
	update account set money = money + 1000 where name='凤儿';
在B窗口中没有提交事务！！！
步骤六：在A窗口中进行查询：
	没有转账的结果！！！（已经避免了脏读）
步骤七：在B窗口中提交事务！！！
	commit;
步骤八：在A窗口中进行查询：
	转账成功！！！（不可重复读：一个事务读到了另一个事务已经提交的update的数据，导致一次事务中多次查询结果不一致.）
```

**演示避免不可重复读：**

``` xml
步骤一：分别开启两个窗口A,B:
步骤二：分别查询两个窗口的隔离级别：
	select @@tx_isolation
步骤三：设置A窗口的隔离级别为repeatable read;
	set session transaction isolation level repeatable read;
步骤四：在两个窗口中分别开始事务：
	start transaction;
步骤五：在B窗口中完成转账的操作：
	update account set money = money - 1000 where name='守义';
	update account set money = money + 1000 where name='凤儿';
在B窗口没有提交事务！！！
步骤六：在A窗口中进行查询：
没有转账成功！！！（已经避免脏读）
步骤七：在B窗口提交事务！！！
	commit;
步骤八：在A窗口进行查询：
	转账没有成功！！（避免不可重复读）
步骤九：在A窗口中结束事务,再重新查询.
```

**演示隔离级别为 serializable：**

``` xml
步骤一：分别开启两个窗口A,B:
步骤二：分别查询两个窗口的隔离级别：
	select @@tx_isolation
步骤三：设置A窗口的隔离级别为serializable;
	set session transaction isolation level serializable;
步骤四：在两个窗口中分别开启事务：
	start transaction;
步骤五：在B窗口中完成一个insert操作:
	insert into account values (null,'芙蓉',10000);
事务没有提交！！
步骤六：在A窗口中进行查找：
	没有任何结果！！！（不可以事务并发执行的）
步骤七：在B窗口中结束事务！
```



