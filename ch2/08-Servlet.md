### 1、http简单认识

http：超文本传输协议，规定数据的格式。

- 浏览器往服务器发送 ---- 请求
- 服务器往浏览器回写 ---- 响应

#### 1.1 请求：(request)

组成部分：（三部分）

``` html
请求行 
请求头 
(这里空行)
请求体
```

``` xml
请求行:请求信息的第一行
		格式:请求方式	访问的资源 协议/版本
		例如:GET /day0801/1.html HTTP/1.1
		请求方式:get和post
			get会把参数放在url的后面 post不会
			get参数大小有限制,post请求却没有限制
			get请求没有请求体;post请求有请求体 请求参数放在请求体中
请求头:请求信息的第二行到空行结束
    格式:key/value (value可以是多个值)
    常见的请求头:
    Accept: text/html,image/bmp		--支持数据类型    text/html text/css text/javascript 大类型/小类型 mime类型
    Accept-Charset: ISO-8859-1	--字符集
    Accept-Encoding: gzip		--支持压缩
    Accept-Language:zh-cn 		--语言环境
    Host: www.itcast.cn:80		--访问主机
    If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT	  --缓存文件的最后修改时间
    Referer: http://www.itcast.com/index.jsp	 --来自哪个页面、防盗链
    User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)
    Cookie
    Connection:Keep-Alive   	--链接状态
			
请求体:空行以下的内容
    只有post才有请求体  get请求参数 http://xxxx?username=tom&password=123
    格式:username=tom&password=123
```

#### 1.2 响应：(response)

组成部分：

``` html
响应行 
响应头 
(这里空行)
响应体
```

``` html
响应行:响应信息的第一行
    格式:协议/版本 状态码 状态码说明
    例如:HTTP/1.1 200 OK
    状态码:
    200 正常响应成功
    302 重定向
    304 读缓存
    404 用户操作资源不存在
    500 服务器内部异常
响应头:从响应信息的第二行到空行结束
    格式:key/value(value可以是多个值)
    常见的头
    Location: http://www.it315.org/index.jsp 	--跳转方向 和302一起使用的
    Server:apache tomcat			--服务器型号
    Content-Encoding: gzip 			--数据压缩
    Content-Length: 80 			--数据长度
    Content-Language: zh-cn 		--语言环境
    Content-Type: text/html; charset=GB2312 		--数据类型
    Last-Modified: Tue, 11 Jul 2000 18:23:51 GMT	--最后修改时间
    Refresh: 1;url=http://www.it315.org		--定时刷新
    Content-Disposition: attachment; filename=aaa.zip	--下载
    Set-Cookie:SS=Q0=5Lb_nQ; path=/search
    Expires: -1					--缓存
    Cache-Control: no-cache  			--缓存
    Pragma: no-cache   				--缓存
    Connection: Keep-Alive   			--连接
   
响应体:空行以下的内容
	页面上展示的内容
```



### 2、servlet 技术

servlet：动态的 web 开发技术，本质就是一个类，运行在服务器端的一个 java 程序。

作用：处理业务逻辑,生成动态 web 内容。

编写一个 servlet 步骤：

1. 编写一个类

   a.继承 HttpServlet

   b.重写 doGet 或者 doPost 方法

   ``` java
   public class HelloServlet extends HttpServlet {
   	private static final long serialVersionUID = 1L;
       
   	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
   		//接受参数 eg：http:localhost:8080//myweb/index.html?username=jaybo
           String value = req.getParameter("username");
           //往浏览器回写数据
           //resp.getWriter().print("data:"+value);
           resp.setContentType("text/html;charset=utf-8");
           resp.getWriter().print("数据:"+value);	
           System.out.println(value);
   	}
   	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
   		doGet(request,response);
   	}
   }
   ```


2. 编写配置文件 (web-inf/web.xml)

   a.注册 servlet

   b.绑定路径

   eg：在 `web.xml` 编写如下

   ``` xml
   <!-- a.注册servlet 使用servlet标签
   	servlet-name：给servlet起个名字，全局唯一
   	servlet-class：将servlet的权限定名 复制过来
   -->
   <servlet>
   	<description></description>
       <display-name>Demo1Servlet</display-name>
       <servlet-name>Demo1Servlet</servlet-name>
       <servlet-class>com.strivbo.servlet.Demo1Servlet</servlet-class>
   </servlet>

   <!-- b.绑定路径 使用servlet-mapping标签
   	servlet-name：使用上面已起好的名字，建议复制
   	url-pattern：访问路径，要求：目前必须以“/”开头唯一
   -->
   <servlet-mapping>
       <servlet-name>Demo1Servlet</servlet-name>
       <url-pattern>/demo1</url-pattern>
   </servlet-mapping>
   ```


   **url-pattern的配置:★**

   - 方式1：完全匹配  必须以`"/"`开始 例如: `/hello /a/b/c`
   - 方式2：目录匹配  必须`"/"`开始  以`"*"`结束   例如: `/a/*`  `/`
   - 方式3：后缀名匹配 以`"*"`开始 以字符结尾 例如: `*.jsp`  `*.do`  `*.action`

   优先级：完全匹配>目录匹配>后缀名匹配

   ``` xml
   练习:
   	有如下的一些映射关系：
   		Servlet1 映射到 /abc/* 
   		Servlet2 映射到 /*
   		Servlet3 映射到 /abc 
   		Servlet4 映射到 *.do 
   	问题:
   	当请求URL为“/abc/a.html”，“/abc/*”和“/*”都匹配，哪个servlet响应
   		Servlet引擎将调用Servlet1。
   	当请求URL为“/abc”时，“/*”和“/abc”都匹配，哪个servlet响应
   		Servlet引擎将调用Servlet3。
   	当请求URL为“/abc/a.do”时，“/abc/*”和“*.do”都匹配，哪个servlet响应
   		Servlet引擎将调用Servlet1。
   	当请求URL为“/a.do”时，“/*”和“*.do”都匹配，哪个servlet响应
   		Servlet引擎将调用Servlet2.
   	当请求URL为“/xxx/yyy/a.do”时，“/*”和“*.do”都匹配，哪个servlet响应
   		Servlet引擎将调用Servlet2。
   ```

    **load-on-startup**：

   在 servlet 标签有一个子标签 load-on-startup

   - 作用：用来修改 servlet 的初始化时机
   - 取值：正整数  值越大优先级越低

   ``` xml
   <servlet>
       <servlet-name>LoginServlet</servlet-name>
       <servlet-class>com.itheima.web.servlet.LoginServlet</servlet-class>
       <load-on-startup>2</load-on-startup>
   </servlet>
   <servlet-mapping>
       <servlet-name>LoginServlet</servlet-name>
       <url-pattern>/login</url-pattern>
   </servlet-mapping>
   ```

   PS：当我们的配置文件里面没有指定配置的话，会查找 tomcat 的`web.xml`。若请求我们自己的项目处理不了，tomcat的默认的 servlet 会帮我们处理信息。

3. 访问：`http://主机:端口号/项目名/路径`，比如访问步骤 2 的 Servlet，则可以 `http://localhost/day09/demo1?username=tom`

   ``` xml
   接受参数:  格式:key=value
   	Sting value=request.getParameter("key")
   	例如: http://localhost/day09/hello?username=tom
   		request.getParameter("username")就可以获取tom值
   回写内容: response
   	response.getWriter().print("success");
   	处理响应数据中文乱码:
   		resp.setContentType("text/html;charset=utf-8"); //建议大家放在方法中的第一行
   ```


### 3、案例驱动学习

1. 先有数据库和表：

   ``` html
   create database day09;
   use day09;
   create table user(
       id int primary key auto_increment,
       username varchar(20),
       password varchar(20),
       email varchar(20),
       name varchar(20),
       sex varchar(10),
       birthday date,
       hobby varchar(50)
   );
   insert into user values (null,'tom','123','tom@126.com','tom','1','1988-01-01',null);
   ```

2. 创建工程：

3. 复制页面：

4. 导入 jar 包：mysql 驱动包、dbutils 、c3p0

5. 导入工具类和配置文件：datasourceUtils、c3p0-config.xml(注意修改数据名称)

6. 创建 servlet（）：接受用户名和密码，调用service层(UserService)完成登录操作。提示信息

7. UserService：`login(username,password)`  调用 dao 

8. dao：通过用户名和密码查询数据库

![](http://p35l3ejfq.bkt.clouddn.com/18-5-9/91974438.jpg)



### 4、Servlet 总结

#### 4.1 Servlet 的体系结构

 servlet、genericservlet、httpservlet 之间的关系：http://www.cnblogs.com/ellisonDon/archive/2012/10/25/2738366.html

> eclipse 下查看源码，关联 tomcat 源码，即关联``

servlet：（接口）

![](http://p35l3ejfq.bkt.clouddn.com/18-5-31/62624171.jpg)

Genericservlet：（抽象类；实现了 Servlet 接口）

![](http://p35l3ejfq.bkt.clouddn.com/18-5-31/64832517.jpg)

HttpServlet：（抽象类；继承自 GenericServlet 类；实现了 Servlet 接口）

![](http://p35l3ejfq.bkt.clouddn.com/18-5-31/38410826.jpg)

当编写一个 servlet 时，必须直接或间接实现 servlet 接口，最可能实现的方法就是扩展`javax.servlet.genericservlet` 或 `javax.servlet.http.httpservlet`。当实现 `javax.servlet.servlet` 接口时必须实现5个方法。

``` xml
servlet常用方法:
		void init(ServletConfig config):初始化
		void service(ServletRequest request,ServletResponse response):服务 处理业务逻辑
		void destroy():销毁
		String getServletInfo():获取servlet信息（不常用）
		ServletConfig getServletConfig() :获取当前servlet的配置对象
---------------------------------------------------------------------------------------------
GenericServlet常用方法:	
		除了service方法没有显示，其他都实现了
		空参的Init() 若我们自己想对servlet进行初始化操作,重写这个init()方法即可
--------------------------------------------------------------------------------------------
HttpServlet常用方法：
		service做了实现，把参数强转，调用了重载的service方法
			重载的service方法获取请求的方式,根据请求方式的不同调用相应doXxx()方法
		doGet和doPost方法
```

#### 4.2 Servlet 生命周期

- void init(ServletConfig config)：初始化

  ``` xml
  初始化方法
  * 执行者:服务器
  * 执行次数:一次
  * 执行时机:默认第一次访问的时候
  ```

- void service(ServletRequest request,ServletResponse response)：服务 处理业务逻辑

  ``` xml
  * 服务
  * 执行者:服务器
  * 执行次数:请求一次执行一次
  * 执行时机:请求来的时候
  ```

- void destroy()：销毁

  ``` xml
  * 销毁
  * 执行者:服务器
  * 执行次数:只执行一次
  * 执行时机:当servlet被移除的时候或者服务器正常关闭的时候
  ```


> **serlvet是单实例多线程**
>
> 默认第一次访问的时候，服务器创建servlet，并调用init实现初始化操作，并调用一次service方法。
>
> 每当请求来的时候，服务器创建一个线程，调用service方法执行自己的业务逻辑。
>
> 当serlvet被移除的时候服务器正常关闭的时候，服务器调用servlet的destroy方法实现销毁操作.



**---------------------------------通过案例学习----------------------------------------**

eg1：当我们登录失败，提示"用户名密码不匹配",3秒以后跳转到登录页面。

技术分析：定时刷新

``` xml
常见的响应头-refresh
	响应头格式:
		refresh:秒数;url=跳转的路径
	设置响应头:
		response.setHeader(String key,String value);设置字符串形式的响应头
		response.addHeader(String key,String value);追加响应头, 若之前设置设置过这个头,则追加;若没有设置过,则设置
	设置定时刷新:
		response.setHeader("refresh","3;url=/day0901/login.htm");
步骤分析:
	登录失败之后,修改业务逻辑
		打印之后添加一个头信息即可
```

eg2：统计登录成功的总人次。在一个用户登录成功之后，获取之前登录成功总人次,将次数+1，在访问另一个servlet的时候显示登录成功的总人次。

技术分析：**ServletContext**（上下文，全局管理者）

``` xml
ServletContext:
	上下文(全局管理者)
	常用的方法:
		setAttribute(String key,Object value);//设置值
		Object getAttribute(String key);//获取值
		removeAttribute(String key)://移除值
获取全局管理者:
	getServletContext():
```

``` xml
步骤分析:
	1.在项目启动的时候,初始化登录次数
		在loginservlet的init()方法中获取全局管理者,将值初始化为0,放入servletcontext上
	2.登录成功之后,在loginservlet中获取全局管理者,获取登录成功的总次数
	3.然后将次数+1,让后将值设置回去
	4.当访问showServlet的时候,获取全局管理者,获取登录成功的总次数,然后在页面上打印出来即可
```

///////////////////////////////////////////////////////////////////////////////////////

**ServletConfig：**(了解下)

``` xml
servlet配置对象
	作用:
		1.获取当前servlet的名称
		2.获取当前servlet的初始化参数
		3.获取全局管理者
	方法:
		String getServletName():获取当前servlet的名称(web.xml配置的servlet-name)
		
		String  getInitParameter(String key):通过名称获取指定的参数值
		Enumeration getInitParameterNames() :获取所有的参数名称
			初始化参数是放在 web.xml文件 
				servlet标签下子标签 init-param
		★getServletContext():获取全局管理者
```

servletconfig 是由服务器创建的，在创建 servlet 的同时也创建了它，通过 servlet 的 init(ServletConfig config)

将config 对象传递给 servlet，由 servlet 的 getServletConfig 方法获取。



**ServletContext：**（理解）

上下文(全局管理者)，一个项目的引用，代表了当前项目。当项目启动的时候，服务器为每一个web项目创建一个

servletcontext对象。当项目被移除的时候或者服务器关闭的时候servletcontext销毁。

作用：

1. 获取全局的初始化参数
2. 共享资源(xxxAttribute)
3. 获取文件资源
4. 其他操作

``` xml
获取servletcontext:
		方式1:了解 
			getServletConfig().getServletContext()
		方式2:
			getServletContext()
	常用方法:
		1.了解
			String  getInitParameter(String key):通过名称获取指定的参数值
			Enumeration getInitParameterNames() :获取所有的参数名称	
			 在根标签下有一个 context-param子标签 用来存放初始化参数
				<context-param>
					<param-name>encoding</param-name>
					<param-value>utf-8</param-value>
				</context-param>
		2.xxxAttribute
		3.
			String getRealPath(String path):获取文件部署到tomcat上的真实路径(带tomcat路径)
				getRealPath("/"):D:\javaTools\apache-tomcat-7.0.52\webapps\day09\
			InputStream getResourceAsStream(String path):以流的形式返回一个文件
		4.获取文件的mime类型  大类型/小类型
			String getMimeType(String 文件名称)
```

///////////////////////////////////////////////////////

**域对象：**★★★

``` xml
servletcontext
	当成map集合
		常用方法:
			xxxAttribute()
	servletcontext创建和销毁:
		当项目启动的时候,服务器为每一个web项目创建一个servletcontext对象.
		当项目被移除的时候或者服务器关闭的时候servletcontext销毁
	存放:
		共享的数据
//////////////////////////////////////////////////
获取文件的路径:
	通过类加载器获取文件:2.txt 放在classes目录下无论是java项目还是web项目都可以
		类.class.getClassLoader().getResource("2.txt").getPath()
```



#### 4.3 小总结

servlet：本质上就是一个类，运行在服务器端的一个 java 小程序，生成动态web内容处理业务逻辑。

编写servlet步骤：

1. 编写一个类 继承了HttpServlet 重写了doGet()或者doPost()

2. 打开WEB-INF下的 `web.xml`编写配置文件：

   ``` xml
   <!-- 注册servlet  -->
   <servlet>
       <servlet-name>给servlet起个名字 名字唯一</servlet-name>
       <servlet-class>servlet的全限定名</servlet-class>
   </servlet>
   <!-- 绑定路径	-->
   <serlvet-mapping>
       <servlet-name>给servlet起个名字 名字唯一</servlet-name>
       <url-pattern>访问路径 目前都是以"/"开始</url-pattern>
   </serlvet-mapping>
   ```

3. 访问路径：`http://主机:端口号/项目名/路径`

servlet的体系结构：`Servlet--->GenericServlet-->HttpServlet`

httpServlet中的方法：

> 实现了service方法，强转了两个参数，调用了重载的service方法
>
> 重载的service方法中，获取请求的方式，根据请求方式的不同调用相应的doXxx方法
>
> doGet()：处理get请求的逻辑，
>
> doPost()：处理post请求的逻辑 (只有表单提交的时候把method设置成post的时候)

``` xml
service(ServletRequest request,ServletResponse response):在这个方法中 将两个参数强转,调用了重载service方法
	service(HttpServletRequest request,HttpServletResponse response):获取请求的方式,根据请求方式的不同调用相应doXxx方法
	doGet和doPost方法:用来处理我们自己业务逻辑
```

其他：

``` xml
servlet的生命周期 ★★★

url-pattern的配置:
		完全匹配  /a/b
		目录匹配  /a/b/*
		后缀名匹配 *.jsp
		优先级:完全匹配 >目录匹配 >后缀名匹配
	  一个路径对应一个servlet
	  一个servlet可以对应多个路径
完全匹配>目录匹配>后缀名匹配
	
load-on-startup:修改servlet的初始化时机

若我们自己的项目处理不了请求，服务器里面defaultservlet来处理.
```

``` xml
servletConfig:(了解)
	servlet的配置对象
	作用:
		获取servlet的名称
		获取servlet初始化参数
		★获取全局管理者
servletContext:
	上下文(全局管理者)
	作用:
		1.获取全局的初始化参数
		2.共享资源
		3.获取资源
		4.获取文件的mime类型
	获取:
		在servlet中直接调用 getServletContext()
	常用方法:
		String getInitParameter(String key):根据key获取指定的初始化参数
			<context-param>
				<param-name>
				<param-value>
			
		String getRealPath(String 文件路径):获取的指定文在在tomcat上的绝对路径
			文件路径从项目的根目录往后写
		InputStream getREsourceAsStream(String 文件路径):以流的形式返回一个文件
		
		String getMimeType(文件名):格式 大类型/小类型
		
/////////////////////////////////////
域对象:
	创建:在服务器启动的时候,服务器会为每一个项目创建一个全局管理者,servletcontext就是当前项目的引用
	销毁:在项目被移除或者服务器关闭的时候销毁
	常用的方法:
		xxxAttribute()
			setAttribute(String key,Object value)
			Object getAttribute(String key)
			removeAttribute(String key)
//////////////////////////////////////////
通过类加载器获取文件的路径(处于classes目录下的文件)
	类.class.getClassLoader().getReource("文件路径").getPath()
	类.class.getClassLoader().getReourceAsStream("文件路径")
```



**使用 request 和 response**

response：

``` xml
作用:往浏览器写东西
组成部分:
	响应行 响应头 响应体
操作响应行 
	格式:
		协议/版本 状态码 状态码说明
    状态码:
        1xx:已发送请求
        2xx:已完成响应
        	200:正常响应
        3xx:还需浏览器进一步操作
            302:重定向 配合响应头:location
            304:读缓存
        4xx:用户操作错误
            404:用户操作错误.
            405:访问的方法不存在
        5xx:服务器错误
        	500:内部异常
    常用方法:
    	setStatus(int 状态码):针对于 1 2 3 
        了解 :sendError(int 状态码):针对于 4xx和5xx
```

操作响应头: 
``` xml
格式:key/value(value可以是多个值)
    常用的方法:
        setHeader(String key,String value):设置字符串形式的响应头
        了解:setIntHeader(String key,int value):设值整形的响应头
        了解:setDateHeader(String key,long value):设值时间的响应头

        addHeader(String key,String value):添加置字符串形式的响应头 之前设置过则追加,若没有设置过则设置
        了解:addIntHeader(String key,int value):添加整形的响应头
        了解:addDateHeader(String key,long value):添加时间的响应头
    常用的响应头:
        location:重定向
        refresh:定时刷新
        content-type:设置文件的mime类型,设置响应流的编码及告诉浏览器用什么编码打开
        content-disposition:文件下载

    重定向:	
        方式1:
        	★response.sendRedirect("/day10/loc2");
        方式2:
        	response.setStatus(302);
        	respooen.setHeader("location","/day10/loc2");

    定时刷新:
    	方案1:设置头 refresh 
    		respooen.setHeader("refresh","秒数;url=跳转的路径");
   	 	方案2:http的meta标签
    		<meta http-equiv="refresh" content="3;url=/day10/refresh2.html">
```

操作响应体：

``` xml
页面上要展示的内容
常用方法:
    Writer getWriter():字符流
    ServletOutputStream getOutputStream() :字节流

    自己写的东西用字符流,其他一概用字节流.

处理响应中文乱码:
    方式1:★
    	response.setContentType("text/html;charset=utf-8");
    方式2:理解
    	response.setHeader("content-type", "text/html;charset=utf-8");

    注意:
        两个流互斥
        当响应完成之后,服务器会判断一下流是否已经关闭,若没有关闭,服务器会帮我们关闭.(底层使用的缓冲流)
```

案例：文件下载

1、超链接下载

`<a href="/day10/download/day10.txt">下载 day10.txt</a>`，若浏览器能解析该资源的mime类型,则打开;若不能接下则下载;

2、编码下载，通过servlet完成

``` xml
<a href="/day10/download?name=day10.txt">下载 day10.txt</a>
a.设置文件的mime类型
    String mimeType=context.getMimeType(文件名)
    response.setContentType(mimeType);

b.设置下载头信息 content-disposition
	response.setHeader("content-disposition", "attachment;filename="+文件名称);

c.提供流
	response.getOutputStream();

扩展：使用commons-io工具类。对拷流：IOUtils.copy(is,os);
```



**请求中文乱码问题：**

> 对于 get 请求：参数追加到地址栏，会使用utf-8编码,服务器(tomcat7)接受到请求之后，使用iso-8859-1解码，所以会出现乱码；
>
> 对于 post 请求：参数是放在请求体中，服务器获取请求体的时候使用iso-8859-1解码，也会出现乱码。

``` xml
通用的方法:
	new String(参数.getBytes("iso-8859-1"),"utf-8");
针对于post请求来说:只需要将请求流的编码设置成utf-8即可
	request.setCharacterEncoding("utf-8");
```

扩展：

- `URLEncoder.encode(s, "utf-8");` 指定编码
- `URLDecoder.decode(s8, "iso8859-1");` 指定解码

文件下载扩展：

- 中文名称的文件名下载的时候名称会出现问题
- 常见的浏览器需要提供文件名称的utf-8编码
- 对于火狐来说需要提供文件名称的base64编码
  - 方案1：使用工具类
  - 方案2：网上的方式(8成好使) `new String(filename.getByte("gbk"),"iso8859-1");`



**请求和转发的区别：**

转发：

![](http://p35l3ejfq.bkt.clouddn.com/18-5-31/78285358.jpg)

重定向：

![](http://p35l3ejfq.bkt.clouddn.com/18-5-31/69589705.jpg)

> - 重定向发送两次请求，请求转发一次请求
> - 重定向地址栏发生该表，请求转发不变
> - 重定向是从浏览器发送,请求转发是服务器内部
> - 重定向不存在request域对象，请求转发可以使用request域对象
> - 重定向是response的方法，请求转发是request的方法
> 	 重定向可以请求站外资源，请求转发不可以	



#### 4.4 回顾

``` xml
response:
	操作响应行
		状态码
		常用方法:
			setStatus(int code)  针对的1xx 2xx 3xx
	操作响应头:
		setHeader(String key,String value):设置
		addHeader(String key,String value):添加
		
		常用的响应头:
			location:重定向:
				response.sendRedirect("跳转路径");
			refresh:定时刷新
				response.setHeader("refresh","秒数;url=路径");//java
				meta标签//html代码
			content-type:设置文件的mimeType,及设置响应流的编码并且通知浏览器用什么编码打开
				response.setContentType("text/html;charset=utf-8");
			content-disposition:设置文件下载
				response.setHeader("content-disposition","attachment;filename="+文件名称);
	操作响应体
		getWriter():
		getOutputStream():
		注意:俩流互斥,服务器帮我们关闭此流
	响应的中文乱码:
		response.setContentType("text/html;charset=utf-8");
////////////////////////////////
文件下载:
	1.超链接下载
	2.编码下载(两个头一个流)
		设置文件的mimetype
		设置下载头信息
		对拷流
	扩展:
		文件名称中文问题:
			方法1:
				firefox :base64
				其他:utf-8
			方法2:(八九成可以使用)
				new String(filename.getBytes("gbk"),"iso8859-1");
////////////////////////////////
request:请求 获取浏览器发送过来的数据
	操作请求行
		getMethod():请求方式
		getContextPath():获取项目名称
		getRemoteAddr():获取请求的ip地址
		
	操作请求头
		String getHeader(String key)
		常见的头信息:
			user-agent:获取浏览器内核
			referer:页面从那里跳转过来的
	操作请求参数
		String getParameter(String key):
		String[] getParameterValues(String key):
		Map<String ,String[]> getParameterMap();
		
	请求的中文乱码:
		对于get请求:参数追加到地址栏,会使用utf-8编码,服务器(tomcat7)接受到请求之后,使用iso-8859-1解码,所以会出现乱码
		对于post请求,参数是放在请求体中,服务器获取请求体的时候使用iso-8859-1解码,也会出现乱码
		
		通用的方法:
			new String(参数.getBytes("iso-8859-1"),"utf-8");
		针对于post请求来说:只需要将请求流的编码设置成utf-8即可
			request.setCharacterEncoding("utf-8");
	域对象:request
		请求转发:
			request.getRequestDispatcher("内部路径").forward(request,response);
		request生命周期:
			一次请求
////////////////
扩展:封装数据:
	apache的BeanUtils
		1.导入两个jar包
		2.调用 BeanUtils.populate(Object bean,map);
```



### 5、更多学习

- [servlet就是这么简单](https://mp.weixin.qq.com/s/JLhi3Lvr5fruanDXGnXmSg)（servlet 相关知识点总结的不错，并且有代码演示）





