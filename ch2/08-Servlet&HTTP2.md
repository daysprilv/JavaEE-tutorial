
# 1. HTTP(超文本传输协议)

## (1) HTTP 简介

HTTP：超文本传输协议，规定数据的格式。

- 浏览器往服务器发送 ---- 请求
- 服务器往浏览器回写 ---- 响应

HTTP 是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在 WWW 中使用的是 HTTP/1.0 的第六版，HTTP/1.1 的规范化工作正在进行之中，而且 HTTP-NG(Next Generation of HTTP) 的建议已经提出。
HTTP 协议的主要特点可概括如下：

1. 支持客户/服务器模式。
2. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于 HTTP 协议简单，使得 HTTP 服务器的程序规模小，因而通信速度很快。
3. 灵活：HTTP 允许传输任意类型的数据对象。正在传输的类型由 Content-Type 加以标记。
4. 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
5. 无状态：HTTP 协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

## (2) URL(统一资源定位符)

HTTP（超文本传输协议）是一个基于请求与响应模式的、无状态的、应用层的协议，常基于 TCP 的连接方式，HTTP1.1 版本中给出一种持续连接的机制，绝大多数的 Web 开发，都是构建在 HTTP 协议之上的 Web 应用。

HTTP URL （URL 是一种特殊类型的 URI，包含了用于查找某个资源的足够的信息）的格式如下：`http://host[":"port][abs_path]`

`http` 表示要通过 HTTP 协议来定位网络资源；`host` 表示合法的 Internet 主机域名或者 IP 地址；`port` 指定一个端口号，为空则使用缺省端口 80；`abs_path` 指定请求资源的 URI；如果 URL 中没有给出 abs_path，那么当它作为请求 URI 时，必须以  `/` 的形式给出，通常这个工作浏览器自动帮我们完成。

例如：

1. 输入：`www.guet.edu.cn`，浏览器自动转换成：`http://www.guet.edu.cn/`
2.  `http:192.168.0.116:8080/index.jsp `

## (3) HTTP请求(Request)

组成部分：（三部分）

``` markdown
请求行 
请求头 
(这里空行)
请求体
```

``` markdown
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

HTTP 请求由三部分组成，分别是：请求行、消息报头、请求正文

（1）请求行以一个方法符号开头，以空格分开，后面跟着请求的 URI 和协议的版本，格式如下：`Method Request-URI HTTP-Version CRLF`

> 其中 Method 表示请求方法；Request-URI 是一个统一资源标识符；HTTP-Version 表示请求的 HTTP 协议版本；CRLF 表示回车和换行（除了作为结尾的 CRLF 外，不允许出现单独的 CR 或 LF 字符）。

请求方法（所有方法全为大写）有多种，各个方法的解释如下：

``` xml
POST    在Request-URI所标识的资源后附加新的数据
HEAD    请求获取由Request-URI所标识的资源的响应消息报头
PUT     请求服务器存储一个资源，并用Request-URI作为其标识
DELETE  请求服务器删除Request-URI所标识的资源
TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
CONNECT 保留将来使用
OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求		
```

应用举例：

- GET 方法：在浏览器的地址栏中输入网址的方式访问网页时，浏览器采用 GET 方法向服务器获取资源，例如：`GET /form.html HTTP/1.1 (CRLF)`


- POST 方法要求被请求服务器接受附在请求后面的数据，常用于提交表单。例如：

  ``` markdown
  POST /reg.jsp HTTP/ (CRLF)
  Accept:image/gif,image/x-xbit,... (CRLF)
  ...
  HOST:www.guet.edu.cn (CRLF)
  Content-Length:22 (CRLF)
  Connection:Keep-Alive (CRLF)
  Cache-Control:no-cache (CRLF)
  (CRLF)         //该CRLF表示消息报头已经结束，在此之前为消息报头
  user=jeffrey&pwd=1234  //此行以下为提交的数据
  ```

- HEAD 方法与 GET 方法几乎是一样的，对于 HEAD 请求的回应部分来说，它的 HTTP 头部中包含的信息与通过 GET 请求所得到的信息是相同的。利用这个方法，不必传输整个资源内容，就可以得到 Request-URI 所标识的资源的信息。该方法常用于测试超链接的有效性，是否可以访问，以及最近是否更新。

（2）请求报头后述

（3）请求正文

## (4) HTTP响应(Response)

在接收和解释请求消息后，服务器返回一个 HTTP 响应消息。

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

HTTP 响应也是由三个部分组成，分别是：状态行、消息报头、响应正文

（1）状态行格式如下：

> HTTP-Version Status-Code Reason-Phrase CRLF

其中，HTTP-Versio n 表示服务器 HTTP 协议的版本；Status-Code 表示服务器发回的响应状态代码；Reason-Phrase 表示状态代码的文本描述。

状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：

``` xml
1xx：指示信息--表示请求已接收，继续处理
2xx：成功--表示请求已被成功接收、理解、接受
3xx：要完成请求必须进行更进一步的操作
4xx：客户端错误--请求有语法错误或请求无法实现
5xx：服务器端错误--服务器未能实现合法的请求
```

常见状态代码、状态描述、说明：

``` xml
200 OK      //客户端请求成功
400 Bad Request  //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
403 Forbidden  //服务器收到请求，但是拒绝提供服务
404 Not Found  //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error //服务器发生不可预期的错误
503 Server Unavailable  //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
```

例如：`HTTP/1.1 200 OK （CRLF）`

（2）响应报头后述

（3）响应正文就是服务器返回的资源的内容 

## (5HTTP消息报头

HTTP 消息由客户端到服务器的请求和服务器到客户端的响应组成。请求消息和响应消息都是由开始行（对于请求消息，开始行就是请求行，对于响应消息，开始行就是状态行），消息报头（可选），空行（只有 CRLF 的行），消息正文（可选）组成。

HTTP 消息报头包括普通报头、请求报头、响应报头、实体报头。每一个报头域都是由名字 +`：`+ 空格 + 值 组成，消息报头域的名字是大小写无关的。

**（1）普通报头**

在普通报头中，有少数报头域用于所有的请求和响应消息，但并不用于被传输的实体，只用于传输的消息。

例如：

- Cache-Control：用于指定缓存指令，缓存指令是单向的（响应中出现的缓存指令在请求中未必会出现），且是独立的（一个消息的缓存指令不会影响另一个消息处理的缓存机制），HTTP1.0 使用的类似的报头域为 Pragma。
- 请求时的缓存指令包括：no-cache（用于指示请求或响应消息不能缓存）、no-store、max-age、max-stale、min-fresh、only-if-cached;
- 响应时的缓存指令包括：public、private、no-cache、no-store、no-transform、must-revalidate、proxy-revalidate、max-age、s-maxage.

例如：为了指示 IE 浏览器（客户端）不要缓存页面，服务器端的 JSP 程序可以编写如下：

``` java
response.sehHeader("Cache-Control","no-cache");
//response.setHeader("Pragma","no-cache");作用相当于上述代码，通常两者//合用
这句代码将在发送的响应消息中设置普通报头域：Cache-Control:no-cache
```

- Date 普通报头域表示消息产生的日期和时间
- Connection 普通报头域允许发送指定连接的选项。例如指定连接是连续，或者指定“close”选项，通知服务器，在响应完成后，关闭连接

**（2）请求报头**

请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息。

常用的请求报头：

- Accept：Accept 请求报头域用于指定客户端接受哪些类型的信息。例如：Accept：image/gif，表明客户端希望接受GIF图象格式的资源；Accept：text/html，表明客户端希望接受 html 文本。


- Accept-Charset：Accept-Charset 请求报头域用于指定客户端接受的字符集。例如：Accept-Charset:iso-8859-1,gb2312.如果在请求消息中没有设置这个域，缺省是任何字符集都可以接受。


- Accept-Encoding：Accept-Encoding 请求报头域类似于Accept，但是它是用于指定可接受的内容编码。例如：Accept-Encoding:gzip.deflate。如果请求消息中没有设置这个域服务器假定客户端对各种内容编码都可以接受。


- Accept-Language：Accept-Language 请求报头域类似于 Accept，但是它是用于指定一种自然语言。例如：Accept-Language:zh-cn。如果请求消息中没有设置这个报头域，服务器假定客户端对各种语言都可以接受。


- Authorization：Authorization请求报头域主要用于证明客户端有权查看某个资源。当浏览器访问一个页面时，如果收到服务器的响应代码为401（未授权），可以发送一个包含Authorization请求报头域的请求，要求服务器对其进行验证。


- Host（发送请求时，该报头域是必需的）：Host 请求报头域主要用于指定被请求资源的 Internet 主机和端口号，它通常从 HTTP URL 中提取出来的，例如：

  > 我们在浏览器中输入：`http://www.guet.edu.cn/index.html`
  >
  > 浏览器发送的请求消息中，就会包含Host 请求报头域，如下：
  >
  > Host：`www.guet.edu.cn`，此处使用缺省端口号 80，若指定了端口号，则变成：Host：`www.guet.edu.cn:指定端口号`

- User-Agent：我们上网登陆论坛的时候，往往会看到一些欢迎信息，其中列出了你的操作系统的名称和版本，你所使用的浏览器的名称和版本，这往往让很多人感到很神奇，实际上，服务器应用程序就是从User-Agent这个请求报头域中获取到这些信息。User-Agent 请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器。不过，这个报头域不是必需的，如果我们自己编写一个浏览器，不使用 User-Agent 请求报头域，那么服务器端就无法得知我们的信息了。

请求报头举例：

``` xml
GET /form.html HTTP/1.1 (CRLF)
Accept:image/gif,image/x-xbitmap,image/jpeg,application/x-shockwave-flash,application/vnd.ms-excel,application/vnd.ms-powerpoint,application/msword,/ (CRLF)
Accept-Language:zh-cn (CRLF)
Accept-Encoding:gzip,deflate (CRLF)
If-Modified-Since:Wed,05 Jan 2007 11:21:25 GMT (CRLF)
If-None-Match:W/"80b1a4c018f3c41:8317" (CRLF)
User-Agent:Mozilla/4.0(compatible;MSIE6.0;Windows NT 5.0) (CRLF)
Host:www.guet.edu.cn (CRLF)
Connection:Keep-Alive (CRLF)
(CRLF)
```

**（3）响应报头**

响应报头允许服务器传递不能放在状态行中的附加响应信息，以及关于服务器的信息和对 Request-URI 所标识的资源进行下一步访问的信息。

常用的响应报头


- Location：Location 响应报头域用于重定向接受者到一个新的位置。Location 响应报头域常用在更换域名的时候。

- Server：Server 响应报头域包含了服务器用来处理请求的软件信息。与 User-Agent 请求报头域是相对应的。下面 Server 响应报头域的一个例子：

  ``` xml
  Server：Apache-Coyote/1.1
  WWW-Authenticate
  ```

  `WWW-Authenticate`：响应报头域必须被包含在 401（未授权的）响应消息中，客户端收到 401 响应消息时候，并发送 Authorization 报头域请求服务器对其进行验证时，服务端响应报头就包含该报头域。

  例如：WWW-Authenticate:Basic realm="Basic Auth Test!"  //可以看出服务器对请求资源采用的是基本验证机制。

**（4）实体报头**

请求和响应消息都可以传送一个实体。一个实体由实体报头域和实体正文组成，但并不是说实体报头域和实体正文要在一起发送，可以只发送实体报头域。实体报头定义了关于实体正文（如：有无实体正文）和请求所标识的资源的元信息。

常用的实体报头：


- Content-Encoding：Content-Encoding 实体报头域被用作媒体类型的修饰符，它的值指示了已经被应用到实体正文的附加内容的编码，因而要获得 Content-Type 报头域中所引用的媒体类型，必须采用相应的解码机制。Content-Encoding 这样用于记录文档的压缩方法，如：Content-Encoding：gzip


- Content-Language：Content-Language 实体报头域描述了资源所用的自然语言。没有设置该域则认为实体内容将提供给所有的语言阅读者。如：`Content-Language:da`


- Content-Length：Content-Length 实体报头域用于指明实体正文的长度，以字节方式存储的十进制数字来表示。


- Content-Type：Content-Type实体报头域用语指明发送给接收者的实体正文的媒体类型。例如：

  ``` xml
  Content-Type:text/html;charset=ISO-8859-1
  Content-Type:text/html;charset=GB2312
  ```


- Last-Modified：Last-Modified 实体报头域用于指示资源的最后修改日期和时间。


- Expires：Expires 实体报头域给出响应过期的日期和时间。为了让代理服务器或浏览器在一段时间以后更新缓存中(再次访问曾访问过的页面时，直接从缓存中加载，缩短响应时间和降低服务器负载)的页面，我们可以使用Expires实体报头域指定页面过期的时间。如：`Expires：Thu，15 Sep 2006 16:23:12 GMT`

  > HTTP1.1 的客户端和缓存必须将其他非法的日期格式（包括0）看作已经过期。例如：为了让浏览器不要缓存页面，我们也可以利用 Expires 实体报头域，设置为 0，jsp 中程序如下：`response.setDateHeader("Expires","0");`

##(6) 请求头和响应头小结

请求头：

``` xml
请求头
Accept: text/html,image/*		--支持数据类型
Accept-Charset: ISO-8859-1	--字符集
Accept-Encoding: gzip		--支持压缩
Accept-Language:zh-cn 		--语言环境
Host: www.itcast.cn:80		--访问主机
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT	  --缓存文件的最后修改时间
Referer: http://www.itcast.com/index.jsp	 --来自哪个页面、防盗链
User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)
Cookie
Connection: close/Keep-Alive   	--链接状态
Date: Tue, 11 Jul 2000 18:23:51 GMT	--时间
```

响应头：

``` xml
Location: http://www.it315.org/index.jsp 	--跳转方向
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
Connection: close/Keep-Alive   			--连接
Date: Tue, 11 Jul 2000 18:23:51 GMT
```



# 2. Servlet技术

servlet：动态的 web 开发技术，本质就是一个类，运行在服务器端的一个 java 程序。

作用：处理业务逻辑,生成动态 web 内容。

编写一个 servlet 步骤：

①编写一个类

a. 继承 HttpServlet

b. 重写 doGet 或者 doPost 方法

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

②编写配置文件：`web-inf/web.xml`

a.注册 servlet

b.绑定路径

在 `web.xml` 编写如下

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

**url-pattern的配置：**

   - 方式1：完全匹配  必须以`"/"`开始 例如: `/hello /a/b/c`
   - 方式2：目录匹配  必须`"/"`开始  以`"*"`结束   例如: `/a/*`  `/`
   - 方式3：后缀名匹配 以`"*"`开始 以字符结尾 例如: `*.jsp`  `*.do`  `*.action`

优先级：完全匹配 > 目录匹配 > 后缀名匹配

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

另外，在 servlet 标签有一个子标签 load-on-startup

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

> 注：当我们的配置文件里面没有指定配置的话，会查找 tomcat 的`web.xml`。若请求我们自己的项目处理不了，tomcat 的默认的 servlet 会帮我们处理信息。

③访问：`http://主机:端口号/项目名/路径`，比如访问步骤②的 Servlet，则可以 `http://localhost/day09/demo1?username=tom`

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



# 3. 案例驱动学习

①先有数据库和表：

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

②创建工程：

③复制页面：

④导入 jar 包：mysql 驱动包、dbutils 、c3p0

⑤导入工具类和配置文件：datasourceUtils、c3p0-config.xml(注意修改数据名称)

⑥创建 servlet（）：接受用户名和密码，调用service层(UserService)完成登录操作。提示信息

⑦UserService：`login(username,password)`  调用 dao 

⑧dao：通过用户名和密码查询数据库

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-9-91974438.jpg)



# 4. Servlet总结

## (1) Servlet的体系结构

servlet、genericservlet、httpservlet 之间的关系：[servlet、genericservlet、httpservlet之间的区别 - ellisonDon - 博客园](http://www.cnblogs.com/ellisonDon/archive/2012/10/25/2738366.html)

servlet，接口：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-31-62624171.jpg)

Genericservlet，抽象类；实现了 Servlet 接口：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-31-64832517.jpg)

HttpServlet，抽象类；继承自 GenericServlet 类；实现了 Servlet 接口：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-31-38410826.jpg)

当编写一个 servlet 时，必须直接或间接实现 servlet 接口，最可能实现的方法就是扩展`javax.servlet.genericservlet` 或 `javax.servlet.http.httpservlet`。当实现 `javax.servlet.servlet` 接口时必须实现 5 个方法。

``` xml
servlet常用方法:
		void init(ServletConfig config):初始化
		void service(ServletRequest request,ServletResponse response):服务 处理业务逻辑
		void destroy():销毁
		String getServletInfo():获取servlet信息（不常用）
		ServletConfig getServletConfig() :获取当前servlet的配置对象
-----------------------------------------------------------------------------------------
GenericServlet常用方法:	
		除了service方法没有显示，其他都实现了
		空参的Init() 若我们自己想对servlet进行初始化操作,重写这个init()方法即可
-----------------------------------------------------------------------------------------
HttpServlet常用方法：
		service做了实现，把参数强转，调用了重载的service方法
			重载的service方法获取请求的方式,根据请求方式的不同调用相应doXxx()方法
		doGet和doPost方法
```

## (2) Servlet生命周期(重点)

(1) void init(ServletConfig config)：初始化

``` xml
初始化方法
* 执行者:服务器
* 执行次数:一次
* 执行时机:默认第一次访问的时候
```

(2) void service(ServletRequest request, ServletResponse response)：服务，处理业务逻辑

``` xml
* 服务
* 执行者:服务器
* 执行次数:请求一次执行一次
* 执行时机:请求来的时候
```

(3) void destroy()：销毁

``` xml
* 销毁
* 执行者:服务器
* 执行次数:只执行一次
* 执行时机:当servlet被移除的时候或者服务器正常关闭的时候
```

**serlvet 是单实例多线程：**

- 默认第一次访问的时候，服务器创建 servlet，并调用 init 实现初始化操作，并调用一次 service 方法。

- 每当请求来的时候，服务器创建一个线程，调用 service 方法执行自己的业务逻辑。

- 当 serlvet 被移除的时候服务器正常关闭的时候，服务器调用 servlet 的 destroy 方法实现销毁操作。

例子1：当我们登录失败，提示"用户名密码不匹配"，3秒以后跳转到登录页面。

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

例子2：统计登录成功的总人次。在一个用户登录成功之后，获取之前登录成功总人次,将次数+1，在访问另一个servlet的时候显示登录成功的总人次。

技术分析：ServletContext（上下文，全局管理者）

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

上下文（全局管理者），一个项目的引用，代表了当前项目。当项目启动的时候，服务器为每一个 web 项目创建一个。

servletcontext 对象。当项目被移除的时候或者服务器关闭的时候 servletcontext 销毁。

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

**域对象：**

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

## (3) Servlet总结

servlet：本质上就是一个类，运行在服务器端的一个 Java 小程序，生成动态 web 内容处理业务逻辑。

编写 servlet 步骤：

1. 编写一个类 继承了 HttpServlet 重写了 doGet() 或者 doPost()

2. 打开 WEB-INF 下的  `web.xml` 编写配置文件：

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

servlet 的体系结构：`Servlet --> GenericServlet --> HttpServlet`

httpServlet 中的方法：

- 实现了 service 方法，强转了两个参数，调用了重载的 service 方法

- 重载的 service 方法中，获取请求的方式，根据请求方式的不同调用相应的 doXxx 方法

- doGet()：处理 get 请求的逻辑，

- doPost()：处理 post 请求的逻辑（只有表单提交的时候把 method 设置成 post 的时候）

``` xml
service(ServletRequest request,ServletResponse response):在这个方法中 将两个参数强转,调用了重载
service方法

service(HttpServletRequest request,HttpServletResponse response):获取请求的方式,根据请求方式的不同调用相应doXxx方法

doGet和doPost方法:用来处理我们自己业务逻辑
```

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

（1）超链接下载

`<a href="/day10/download/day10.txt">下载 day10.txt</a>`，若浏览器能解析该资源的 mime 类型，则打开；若不能接下则下载。

（2）编码下载，通过 servlet 完成

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

> 对于 Get 请求：参数追加到地址栏，会使用 utf-8编码，服务器（如 tomcat7）接受到请求之后，使用 iso-8859-1 解码，所以会出现乱码；
>
> 对于 post 请求：参数是放在请求体中，服务器获取请求体的时候使用 iso-8859-1 解码，也会出现乱码。

``` xml
通用的方法:
	new String(参数.getBytes("iso-8859-1"),"utf-8");
针对于post请求来说:只需要将请求流的编码设置成utf-8即可
	request.setCharacterEncoding("utf-8");
```

扩展：

- `URLEncoder.encode(s, "utf-8");`：指定编码
- `URLDecoder.decode(s8, "iso8859-1");`：指定解码

文件下载扩展：

- 中文名称的文件名下载的时候名称会出现问题
- 常见的浏览器需要提供文件名称的 utf-8 编码
- 对于火狐来说需要提供文件名称的 base64 编码
  - 方案1：使用工具类
  - 方案2：网上的方式（8层好使）`new String(filename.getByte("gbk"),"iso8859-1");`

**转发和重定向的区别：**

- 重定向发送两次请求，请求转发一次请求
- 重定向地址栏发生该表，请求转发不变
- 重定向是从浏览器发送,请求转发是服务器内部
- 重定向不存在request域对象，请求转发可以使用request域对象
- 重定向是response的方法，请求转发是request的方法
  重定向可以请求站外资源，请求转发不可以	

转发：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190220012119.png)

重定向：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190220012334.png)

## (4) Servlet回顾

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



# 5. 优质博文学习

[servlet就是这么简单](https://mp.weixin.qq.com/s/JLhi3Lvr5fruanDXGnXmSg) - Servlet 相关知识点总结的不错，并且有代码演示。



