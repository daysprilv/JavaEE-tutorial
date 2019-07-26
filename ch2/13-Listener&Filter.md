
# 1. 监听器(listener)

## (1) 监听器简介

什么是监听器？监听器就是一个 Java 类用来监听其他的 JavaBean 的变化。

> 注：监听器和过滤器属于Servlet中的高级技术。

监听器的应用：

- 主要在 Swing 编程
- 在 Android 大量应用

监听器的术语：

- 事件源：被监听的对象，例子：汽车
- 监听器对象：监听的对象，例子：汽车上报警器
- 事件源与监听器绑定
- 事件：指的是事件源的改变
- 获取事件源对象

**servlet 的监听器：**

监听 ServletContext、HttpSession、ServletRequest

事件源和监听器绑定的过程：通过配置完成。

注意：listener 全部是接口

Servlet 中的监听器，提供了 8 个监听器：

- 一类：监听三个域对象的创建和销毁
  - ServletContextListener
  - ServletRequestListener
  - HttpSessionListener
- 二类：监听三个域对象属性的变化
  - ServletContextAttributeListener
  - ServletRequestAttributeListener
  - HttpSessionAttributeListener
- 三类：监听 session 中 javabean 的状态
  - HttpSessionActivationListener(钝化和活化)
  - HttpSessionBindingListener(绑定和解绑)

Servlet 中的监听器：提供了 8 个监听器。

- 一类：监听三个域对象的创建和销毁的监听器。 3个
- 二类：监听三个域对象的属性变更的监听器(属性添加、属性移除、属性替换) 。 3个
- 三类：监听 HttpSession 对象中的 JavaBean 的状态的改变（绑定，解除绑定，钝化和活化） 。 3个

使用步骤：

1. 编写一个类，实现接口
2. 重写方法
3. 编写配置文件（大部分都是）

## (2) 演示各个监听器

### 监听三个对象的创建和销毁

####①ServletContextListener

ServletContextListener：监听ServletContext对象的创建和销毁

``` xml
ServletContextListener
    创建:服务器启动的时候,会为每一个项目都创建一个servletContext
    销毁:服务器关闭的时候,或者项目被移除的时候
    以后用来加载配置文件
```

（1）编写一个类实现监听器的接口

``` java
public class MyServletContextListener implements ServletContextListener{

    @Override
    /**
	 * 监听ServletContext对象的创建的方法:
	 * @param sce
	 */
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext对象被创建了...");
    }

    @Override
    /**
	 * 监听ServletContext对象的销毁的方法:
	 * @param sce
	 */
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext对象被销毁了...");
    }

}
```

（2）通过配置完成监听器和事件源的绑定

``` xml
<!-- 配置监听器 -->
<listener>
    <listener-class>com.itheima.weblistener.MyServletContextListener</listener-class>
</listener>
```

【企业中应用】

``` xml
初始化工作.
加载配置文件:Spring框架.
    ContextLoaderListener:
定时任务调度:
    Timer,TimerTask
```

#### ②HttpSessionListener：监听HttpSession对象的创建和销毁的监听器

HttpSessionListener：监听 HttpSession 对象的创建和销毁的监听器

``` xml
创建:
    java中第一次调用request.getSession()的时候
    jsp访问的时候创建
销毁:(三种情况)
    session超时(默认30分钟)
    手动销毁session，即 session.invalidate()
    服务器非正常关闭(正常关闭序列化到硬盘)
```

（1）编写监听器

``` java
public class MyHttpSessionListener implements HttpSessionListener {
	@Override
	public void sessionCreated(HttpSessionEvent se) {
		System.out.println("HttpSession对象被创建了...");
	}
	@Override
	public void sessionDestroyed(HttpSessionEvent se) {
		System.out.println("HttpSession对象被销毁了...");
	}
}
```

（2）配置监听器

``` xml
<listener>
    <listener-class>com.itheima.weblistener.MyHttpSessionListener</listener-class>
</listener>
```

问题：

1. 访问 html 是否创建 session 对象？答：不会
2. 访问一个 Servlet 是否创建 session 对象？答：不会
3. 访问一个 jsp 是否创建 session 对象？答：会

#### ③ServletRequestListener：监听ServletRequest对象的创建和销毁的监听器

ServletRequestListener：监听 ServletRequest 对象的创建和销毁的监听器

``` xml
创建:客户端向服务器发送请求的时候.
销毁:服务器为这次请求作出了响应时候.
```

（1）编写一个监听器

``` java
public class MyServletRequestListener implements ServletRequestListener {
    public void requestInitialized(ServletRequestEvent sre)  { 
    	System.out.println("ServletRequest被创建了...");
    }
    public void requestDestroyed(ServletRequestEvent sre)  { 
    	System.out.println("ServletRequest被销毁了...");
    }	
}
```

（2）配置监听器

``` xml
<listener>
    <listener-class>com.itheima.weblistener.MyServletRequestListener</listener-class>
</listener>
```

问题：

1. 访问 html 是否创建 request 对象？答：会
2. 访问一个 Servlet 是否创建 request 对象？答：会
3. 访问一个 jsp 是否创建 request 对象？答：会



### 监听三个对象属性的变化(添加 替换 删除)

#### ①ServletContextAttributeListener

ServletContextAttributeListener：监听 ServletContext 对象中的属性变更的监听器

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-52970812.jpg)

#### ②HttpSessionAttributeListener

HttpSessionAttributeListener：监听 HttpSession 对象中的属性变更的监听器

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-43199677.jpg)

#### ③ServletRequestAttributeListener：监听ServletRequest对象中的属性变更的监听器

ServletRequestAttributeListener：监听 ServletRequest 对象中的属性变更的监听器

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-8478345.jpg)

### 监听session中javabean的状态

``` xml
三类监听器非常特殊：
	监听器作用在JavaBean上.JavaBean可以自己感知在session中状态.
	这类监听器不用配置.
```

注意：这两个接口需要 javabean 实现，是让 javabean 感知到自己的状态

- HttpSessionBindingListener(绑定和解绑)：检测 java 是否添加到 session 或者从 session 中移除


- HttpSessionActivationListener(钝化和活化)

  - 钝化：javabean 从 session 中序列化到磁盘上
  - 活化：javabean 从磁盘上加载到了 session 中

  可以通过配置文件修改 javabean 什么时候钝化。修改一个项目，只需要在项目下`/meta-info`创建一个`content.xml` 内容如下：

  ``` xml
  <Context>
      <!--
          maxIdleSwap	:1分钟 如果session不使用就会序列化到硬盘.
          directory	:itheima 序列化到硬盘的文件存放的位置.
         -->
      <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1">
          <Store className="org.apache.catalina.session.FileStore" directory="itheima"/>
      </Manager>
  </Context>
  ```

#### ①HttpSessionBindingListener

HttpSessionBindingListener：监听 HttpSession 中的 JavaBean 的绑定和解除绑定的状态。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-12813748.jpg)

### ②HttpSessionActivationListener

HttpSessionActivationListener：监听 HttpSession 中的 JavaBean 的钝化和活化的状态

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-57191187.jpg)

- `sessionDidActivate(HttpSessionEvent se);`：活化
- `sessionWillPassivate(HttpSessionEvent se);`：钝化




# 2. 过滤器(Filter)

什么是过滤器？一个实现了特殊接口的 Java 类，实现对请求资源的过滤功能。

- 过滤器是 Servlet 技术中最为实用的技术。
- Filter 是一个接口

过滤器的作用：对目标资源进行过滤

- 自动登录、解决网站乱码、进行页面静态化、进行响应压缩等等

看下面这张图理解：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-8263968.jpg)

编写 filter 步骤：

``` xml
1. 编写一个类
   a. 实现 filter 接口
   b. 重写方法
2. 编写配置文件
   a. 注册 filter
   b. 绑定路径
3. 测试
```

``` xml
Filter接口的方法:
	init(FilterConfig config):初始化操作
	doFilter(ServletRequest request, ServletResponse response, FilterChain chain):处理业务逻辑
	destroy() :销毁操作
filter的生命周期(了解)
	filter单实例多线程
	filter在服务器启动的时候 服务器创建filter 调用init方法 实现初始化操作
	请求来的时候,创建一个线程 根据路径调用dofilter 执行业务逻辑
	当filter被移除的时候或者服务器正常关闭的时候 调用destory方法 执行销毁操作.
FilterChain:过滤链
	通过chain的dofilter方法,可以将请求放行到下一个过滤器,直到最后一个过滤器放行才可以访问到servlet|jsp
	doFilter()放行方法
★url-pattern配置
	3种
	完全匹配	必须以"/" 开始  例如: /a/b
	目录匹配	必须以"/" 开始 以"*"结束  例如:/a/b/*
	后缀名匹配	以"*."开始 以字符结束   例如 :  *.jsp  *.do  *.action
例如:
	afilter  路径  /*
	bFilter  路径  /demo4
★一个资源有可能被多个过滤器匹配成功,多个过滤器的执行顺序是按照web.xml中filter-mapping的顺序执行的
```

使用过滤器：

（1）编写一个类实现

``` java
public class FilterDemo1 implements Filter{
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
			
	}

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		
		System.out.println("FilterDemo1执行了...");
		
		// 放行
		chain.doFilter(request, response);
	}

	@Override
	public void destroy() {
		
	}
}
```

（2）对过滤器进行配置

``` xml
<filter>
    <filter-name>FilterDemo1</filter-name>
    <filter-class>com.itheima.filter.FilterDemo1</filter-class>
</filter>

<filter-mapping>
    <filter-name>FilterDemo1</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

过滤器的生命周期：

``` xhtml
Servlet的生命周期(*****)
    Filter生命周期：过滤器从创建到销毁的过程.
    服务器启动的时候，服务器就会创建过滤器的对象，每次访问被拦截目标资源，过滤器中的doFilter的方法就会执行。当服务器关闭的时候，服务器就会销毁Filter对象。
```

FilterConfig 的作用：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-3-97369115.jpg)

``` java
// 获得初始化参数:过滤器的初始化参数.
String username = fConfig.getInitParameter("username");
String password = fConfig.getInitParameter("password");
System.out.println("初始化参数"+username+"   "+password);

// 获得所有的初始化参数的名称:
Enumeration<String> names = fConfig.getInitParameterNames();
while(names.hasMoreElements()){
    String name = names.nextElement();
    String value = fConfig.getInitParameter(name);
    System.out.println(name+"    "+value);
}

// 获得过滤器的配置的名称：
String filterName = fConfig.getFilterName();
System.out.println("过滤器名称"+filterName);

```

FilterChain，过滤器链：

过滤器链中的过滤器的执行的顺序跟 `<filter-mapping>` 的配置顺序有关。

方法：`void DoFilter(ServletRequest request, ServletResponse response);`

Filter 的配置：

``` xml
【url-pattern的配置】与servlet中的配置一样：
三种配置:
    完全路径匹配：以 / 开始   /aaa /aaa/bbb
    目录匹配:     以 / 开始   /*  /aaa/*
    扩展名匹配：  不能以 / 开始  *.do  *.jsp  *.action

【servlet-name的配置】通过url-pattern拦截一个Servlet的资源，也可以通过servlet-name标签进行拦截.

【dispatcher的配置】
REQUEST	：默认值.
FORWARD	：拦截转发
ERROR		：拦截跳转到错误页面.全局错误页面.  
INCLUDE	：拦截在一个页面中包含另一个页面.
```

**案例-网站的字符集编码**

需求：在整个网站中，可能会有 get 请求或 post 请求向服务器提交参数，参数中往往有中文信息，在后台每个 servlet 中都需要去处理乱码。无论 get 请求或者是 post 请求提交到 servlet 中，就可以直接调用 getParameter 方法将乱码处理好。

分析：

- 技术分析：

  - Filter 的使用：在到达目标资源之前对 request 中的 getParameter( ) 方法进行增强。
  - 增强一个类的某个方法有以下几种方式：
    1. 继承：能够控制这个类的构造
    2. 装饰者模式（静态代理）：被增强的对象和增强的对象实现相同的接口，在增强对象中获得到被增强对象的引用
    3. 动态代理：这个类实现了接口即可

- 步骤分析：

  1. 设计一个页面：向 servlet 中提交中文

  2. 编写过滤器的方式到达目标资源之前进行增强
     - 增强 request 中的 getParameter( ) 方法

  3. 放行：将增强的 request 放行

  