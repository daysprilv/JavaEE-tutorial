
# 1. JSP(Java服务器页面)

JSP：java server pages（Java服务器页面）

本质上 jsp 就是一个 servlet，在 html 代码中嵌套 java 代码，运行在服务器端,处理请求，生成动态的内容。

对应的 java 和 class 文件在 tomcat 目录下的 work 目录。后缀名 `.jsp`

执行流程：

1. 浏览器发送请求，访问 jsp 页面
2. 服务器接受请求，jspSerlvet 会帮我们查找对应的 jsp 文件
3. 服务器将 jsp 页面翻译成java文件.
4. jvm 会将 java 编译成`.class`文件
5. 服务器运行 class 文件，生成动态的内容.
6. 将内容发送给服务器,
7. 服务器组成响应信息，发送给浏览器
8. 浏览器接受数据，解析展示

作用：将内容的生成和信息的展示相分离。

运行在服务器端，本质上就是一个serlvet，产生的 java 文件和 class 保留在 tomcat 的 work 目录下。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-31-64240789.jpg)

## (1) jsp脚本

- `<%  %>` java代码片段。（翻译成Servlet内部的内容）
- `<%=  %>` 输出表达式 相当于out.print();（翻译成`out.print();`）
- `<%!  %>` 声明成员（翻译成Servlet的service方法内部东西）

``` markdown
<%...%> java程序片段
	生成成jsp的service方法中
<%=...%> 输出表达式
	生成成jsp的service方法中,相当于在java中调用out.print(..)
<%!...%> 声明成员
	成员位置.
```

## (2) jsp的指令

- 作用：声明 jsp 页面的一些属性和动作

- 格式：`<%@指令名称 属性="值" 属性="值"%>`

- jsp 指令的分类：

  - page：主要声明jsp页面的一些属性
  - include：静态包含
  - taglib：导入标签库

  注意：

  1. 一个页面中可以出现多个指令
  2. 指令可以放在任意位置，一般都放在 jsp 页面最上面

**page 指令：** 

``` xml
重要属性:3个
    contentType:设置响应流的编码,及通知浏览器用什么编码打开.设置文件的mimetype
    pageEncoding:设置页面的编码
    import:导入所需要的包
    contentType和pageEncoding联系:
        若两者都出现的时候,各自使用各自的编码
        若只出现一者,两个都使用出现的这个编码
        若两者都不出现,使用服务器默认的编码 tomcat7使用的iso-8859-1
了解属性:
    language:当前jsp页面里面可以嵌套的语言
    buffer:设置jsp页面的流的缓冲区的大小
    autoFlush:是否自动刷新
    extends:声明当前jsp的页面继承于那个类.必须继承的是httpservlet 及其子类
    session:设置jsp页面是否可以使用session内置对象
    isELIgnored:是否忽略el表达式
    errorPage:当前jsp页面出现异常的时候要跳转到的jsp页面
    isErrorPage:当前jsp页面是否是一个错误页面
    	若值为true,可以使用jsp页面的一个内置对象 exception
```

**include 指令：**

``` xml
静态包含,就是将其他页面或者servlet的内容包含进来,一起进行编译运行.生成一个java文件.
格式:	<%@include file="相对路径或者是内部路径" %>
例如:	<%@include file="/jsp/include/i1.jsp" %>

路径:
    相对路径:
        ./或者什么都不写 当前路径
        上一级路径  ../
   绝对路径:
        带协议和主机的绝对路径
        不带协议和主机的绝对路径
        /项目名/资源

   内部路径:
    	不带协议和主机的绝对路径去掉项目名
    	请求转发 静态包含 动态包含
```

**taglib指令：** 导入标签库

``` xml
格式:
	<%@taglib prefix="前缀名" uri="名称空间" %>
若导入之后
    <前缀名:标签 .. >
例如:
    <c:if test="">输出内容</c:if>
```

例子：

``` jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<c:set var="day" value="4"/><!-- 相当于 pageContext.setAttribute("day",3)  -->
	<c:choose>
		<c:when test="${day==1 }">
			周1
		</c:when>
		<c:when test="${day==2}">
			周2
		</c:when>
		<c:when test="${day==3 }">
			周3
		</c:when>
		<c:otherwise>
			其他
		</c:otherwise>
	</c:choose>
</body>
</html>
```

## (3) jsp的内置对象(9大内置对象)

内置对象：在 jsp 页面上可以直接使用的对象。JSP 的内置对象有哪些？常用的方法？真实对象是谁？

| 内置对象                |        类型         |
| ----------------------- | :-----------------: |
| out                     |      JspWriter      |
| request（一次请求）     | HttpServletRequest  |
| response                | HttpServletResponse |
| session（一次会话）     |     HttpSession     |
| exception               |      Throwable      |
| page                    |    Servlet(this)    |
| config                  |    ServletConfig    |
| application（整个项目） |   ServletContext    |
| pageContext（一个页面） |     PageContext     |

**jsp 的四个域对象：** 

``` xml
application		整个项目		applicationScope 	应用范围	
session			一次会话		sessionScope 		会话范围
request			一次请求		requestScope		请求范围
pageContext		一个页面		pageScope			页面范围
```

如下一个 jsp页面：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-31-4496273.jpg)

该 jsp 页面转为 Java 的源代码如下：（得启动 tomcat 服务后在 work 目录下才能找到）

``` java
public final class index_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent {

  private static final javax.servlet.jsp.JspFactory _jspxFactory =
          javax.servlet.jsp.JspFactory.getDefaultFactory();

  private static java.util.Map<java.lang.String,java.lang.Long> _jspx_dependants;

  private volatile javax.el.ExpressionFactory _el_expressionfactory;
  private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;

  public java.util.Map<java.lang.String,java.lang.Long> getDependants() {
    return _jspx_dependants;
  }

  public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
    if (_el_expressionfactory == null) {
      synchronized (this) {
        if (_el_expressionfactory == null) {
          _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }
    return _el_expressionfactory;
  }

  public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
    if (_jsp_instancemanager == null) {
      synchronized (this) {
        if (_jsp_instancemanager == null) {
          _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
        }
      }
    }
    return _jsp_instancemanager;
  }

  public void _jspInit() {
  }

  public void _jspDestroy() {
  }

  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
        throws java.io.IOException, javax.servlet.ServletException {

    final javax.servlet.jsp.PageContext pageContext;
    javax.servlet.http.HttpSession session = null;
    final javax.servlet.ServletContext application;
    final javax.servlet.ServletConfig config;
    javax.servlet.jsp.JspWriter out = null;
    final java.lang.Object page = this;
    javax.servlet.jsp.JspWriter _jspx_out = null;
    javax.servlet.jsp.PageContext _jspx_page_context = null;


    try {
      response.setContentType("text/html; charset=UTF-8");
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;

      out.write("\r\n");
      out.write("<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\" \"http://www.w3.org/TR/html4/loose.dtd\">\r\n");
      out.write("<html>\r\n");
      out.write("<head>\r\n");
      out.write("<meta http-equiv=\"Content-Type\" content=\"text/html; charset=UTF-8\">\r\n");
      out.write("<title>首页</title>\r\n");
      out.write("</head>\r\n");
      out.write("<body>\r\n");
      out.write("\thello 你好哈\r\n");
      out.write("</body>\r\n");
      out.write("</html>");
    } catch (java.lang.Throwable t) {
      if (!(t instanceof javax.servlet.jsp.SkipPageException)){
        out = _jspx_out;
        if (out != null && out.getBufferSize() != 0)
          try {
            if (response.isCommitted()) {
              out.flush();
            } else {
              out.clearBuffer();
            }
          } catch (java.io.IOException e) {}
        if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
      }
    } finally {
      _jspxFactory.releasePageContext(_jspx_page_context);
    }
  }
}
```

pagecontext 作用的理解：

``` xml
1.域对象
	xxxAttribute()
2.操作其他域对象
	方法：xxxAttribute(...,int scope);
    scope取值:
        APPLICATION_SCOPE 
        SESSION_SCOPE 
        REQUEST_SCOPE 
        PAGE_SCOPE 
3.获取其他的内置对象
    getXxx()
    注意:
    	getRequest():获取request内置对象
4.便捷查找,
	findAttribute(String key):
		依次从pagecontext,request,session,application四个域中,查找相应的属性,若查找到了返回值,且结束该次查找
		若查找不到,返回一个null
```

## (4) jsp的动作标签

语法：`<jsp:动作标签 属性=""/>`

为什么使用标签：简化代码的编写，尽量要少在 jsp 中使用`<% %>`

``` xml
<jsp:forward>:请求转发  相当于java中  request.getRequestDispatcher(..).forward(..);
	<jsp:forward page="内部路径"></jsp:forward>
<jsp:include>:动态包含
	就是将被包含页面或者servlet的运行结果包含到当前页面中.
<jsp:param />
   传递参数.
<jsp:useBean />
<jsp:setProperty />
<jsp:getProperty />
```

静态包含和动态包含的区别？

> 静态包含相对于代码的 copy，最终被翻译成一个 Servlet 解释执行的。动态包含，包含其他页面的运行的结果，最终翻译成多个 Servlet 解释执行的。



# 2. EL(表达式语言)

## (1) EL介绍

EL：Expression Language，表达式语言。

jsp 的内置表达式语言，从 jsp2.0 开始。

用来代替 `<%=..%>`

作用：

1. 获取域中数据 
2. 执行运算 
3. 获取常见的 web 对象
4. 调用 java 的方法

语法：`${el表达式}`

jsp 中尽量少使用`<% %>`代码块，使用 el 和 jstl 替换页面中`<% %>`

**获取域中数据：**

- 获取简单数据：`${pageScope|requestScope|sessionScope|applicationScope.属性名}`

  但经常使用是：`${属性名}`，依次从 pageContext、request、session、application 查找指定属性，若查找到返回值，结束该次查找；若查找不到，返回 `“”`

  注意：若属性名中出现了`"."|"+"|"-"`等特殊符号，需要使用 scope 获取。例如：`${requestScope["arr.age"]}`

- 获取复杂数据：

  - 获取数组中的数据：`${域中的名称[index]}`
  - 获取 list 中的数据：`${域中的名称[index]}`
  - 获取 map 中的数据：`${域中的名称.键名}`

- javabean 导航： `${域中javabean名称.bean属性}`

  什么是 javabean？

  java 语言编写的一个可重用的组件，狭义上来说是我们编写的一个普通的 java 类，例如：User、Person

  javabean 规范：

  1. 必须是一个公共的具体的类  public class

  2. 提供私有的字段  private String id; //id称之为字段

  3. 提供公共访问字段的方法 get|set|is 方法

     ``` java
     public String getId(){..}
     一旦有公共的方法之后，get|set|is之后的内容：将首字母小写。将这个东西称之为bean属性，id就是一个bean属性
     ```

  4. 提供一个无参的构造器

  5. 一般实现序列化接口  serializable

     比如如下的 User 类：（setName( ) 这里的 name 就是 bean 属性）

     ``` java
     public class User {
         private String id;
         private String username;
         private String password;
         public String getId() {
             return id;
         }
         public void setId(String id) {
             this.id = id;
         }
         public String getName() {
             return username;
         }
         public void setName(String username) {
             this.username = username;
         }
         public String getPassword() {
             return password;
         }
         public void setPassword(String password) {
             this.password = password;
         }	
     }
     ```

使用 EL 的例子：

``` jsp
<h1>EL获取数据</h1>
<%
pageContext.setAttribute("pname", "王守义");
	request.setAttribute("rname", "王凤儿");
	session.setAttribute("sname", "王如花");
	application.setAttribute("aname", "王芙蓉");
%>
<h3>传统方式</h3>
<%= pageContext.getAttribute("pname")%>
<%= request.getAttribute("rname")%>
<%= session.getAttribute("sname")%>
<%= application.getAttribute("aname")%>
<h3>EL的方式</h3>
${ pageScope.pname }   
${ requestScope.rname }
${ sessionScope.sname }
${ applicationScope.aname }
<hr/>
<%
	//pageContext.setAttribute("name", "王守义");
	//request.setAttribute("name", "王凤儿");
	session.setAttribute("name", "王如花");
	application.setAttribute("name", "王芙蓉");
%>
${ name }
<h3>EL获得数组的数据</h3>
<%
	String[] arrs = {"王守义","王如花","王凤儿"};
	pageContext.setAttribute("arrs", arrs);
%>
${ arrs[1] }
<h3>EL获得List集合的数据</h3>
<%
	List<String> list = new ArrayList<String>();
	list.add("aaa");
	list.add("bbb");
	list.add("ccc");
	pageContext.setAttribute("list", list);
%>
${ list[1] }
<h3>获得Map集合的数据</h3>
<%
	Map<String,String> map = new HashMap<String,String>();
	map.put("aaa", "111");
	map.put("bbb", "222");
	map.put("ccc.ddd", "333");
pageContext.setAttribute("map", map);
%>
${ map["ccc.ddd"] }
<h3>EL获得JavaBean中的数据</h3>
<%
	Person person = new Person();
	person.setId(1);
	person.setName("王美丽");
	pageContext.setAttribute("person", person);
%>
${ person.name }
```

## (2) 执行运算

四则运算 (+、-、*、/)、关系 (>、<、!=) 、逻辑 (&&、||)

注意：

- +：只能进行加法运算，字符串形式数字可以进行加法运算
- empty：`${empty 域中的对象名称}`，判断一个容器的长度是否为0(array set list map),还可以判断一个对象是否为空
- 三元运算符：`${a>b?a:b}`

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-12118162.jpg)

**EL 的内置对象(11个)：**

``` xml
pageScope、requestScope、sessionScope、applicationScope
param、paramValues
header、haederValues
initParam
cookie ★
pageContext ★
```

``` xml
EL常用的对象：11个.
${pageScope}
${requestScope}
${sessionScope}
${applicationScope}
${ param }			:相当于request.getParameter();
${ paramValues }	:相当于request.getParameterValues();
${ header }			:获得请求头 一个key对应一个value
${ headerValues }	:获得请求头 一个key对应多个value 
${ initParam }		:获得初始化参数
${ cookie }			:获得Cookie的信息
${pageContext}		:相当于pageContext对象.
```

注意：除了 pagecontext 其余对象获取的全是 map 集合

和参数相关的 el 内置对象：param、paramValues

和请求头相关的 el 内置对象：header、haederValues

和全局初始化参数相关的 el 内置对象：initParam

cookie 内置对象：

``` xml
${cookie} 获取map{key=Cookie}
	例如:创建cookie
		Cookie c=new Cookie("username","tom");
	通过${cookie}获取相当于	
		{username=new Cookie("username","tom")}
	相当于map的key是cookie的键
		map的value是当前cookie

若项获取名称username的cookie的value值(获取tom)
	${cookie.username.value}--javabean导航
注意:
    java中Cookie的api
        getName():获取cookie的名称
        getValue():获取cookie的value值
我们称name和value是cookie的bean属性

使用cookie内置对象:
	${cookie.给cookie起名字.value}

例如:
    获取jsession的值
    ${cookie.JSESSIONID.value}
```

pageContext：获取不是 map 集合，相当于 jsp 的 pageContext 内置对象

- 在 jsp 页面中获取项目名：`${pageContext.request.contextPath}`

##(3) jsp 注释

- html 注释 `<!--  -->`：注释的内容只在页面上看不到，java 代码和 html 源代码都有
- java 注释：只在 java 代码中存在
- jsp 注释 `<%--  --%>`：只在 jsp 页面中存在，翻译成 java 文件之后就没有了。



# 3. jstl(JSP标准标签库)

## (1) JSTL概述

JSTL（JSP Standard Tag Library，JSP标准[标签库](http://baike.baidu.com/view/1002457.htm) ）是一个不断完善的开放源代码的 JSP 标签库，是由 apache 的jakarta 小组来维护的。JSTL 只能运行在支持 JSP1.2 和 Servlet2.3 规范的容器上，如 tomcat 4.x。在 JSP 2.0 中也是作为标准支持的。

JSTL 的版本：JSTL1.0 、JSTL1.1、JSTL1.2

- JSTL1.0 是 EL 还没有被纳入规范的时候使用标签
- JSTL1.1 和 1.2 的版本中 EL 已经被纳入到规范中，JSTL 可以支持 EL 表达式了

作用：用来代替 java 脚本，结合 EL 替换页面中的`<% %>`

jstl 使用步骤：

1. 导入 jar 包 (jstl.jar和standard.jar)

2. 在页面上导入标签库：`<%@taglib prefix="" uri=""%>`

   例如：`<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`

jstl 的分类:

``` xml
核心标签库：包含Web应用的常见工作，比如：循环、表达式赋值、基本输入输出等。
国际化标签库：用来格式化显示数据的工作，比如：对不同区域的日期格式化等。
数据库标签库：可以做访问数据库的工作。
XML标签库：用来访问XML文件的工作，这是JSTL标签库的一个特点。      
函数标签库：用来读取已经定义的某个函数。
```

- core：核心类库 （要掌握）
- fmt：格式化、国际化
- xml：过时了
- sql：过时了
- 函数库：很少使用

core 核心类库：

- `<c:if>` 

- `<c:forEach>`

- `<c:set>`  往域中设置值

- 分支：

  ``` xml
  <c:choose>
      <c:when></c:when>
      <c:otherwise></c:otherwise>
  </c:choose>
  ```

**`<c:if>`  判断：** `<c:if test="${el表达式}">满足的时候输出的内容</c:if>`

例如：

``` jsp
<c:if test="${3>4 }">
	3大于4
</c:if>
<c:if test="${3<=4 }">
	3不大于4
</c:if>
```

**`<c:forEach>` 循环：**

- ①格式1：

  ``` jsp
  <c:forEach begin="从那里开始" end="到那里结束" step="步长" var="给变量起个名字" varStatus="循环状态变量">
      ${i }--${vs.count }--${vs.current }<br>
  </c:forEach>
  			 
  varStatus:用来记录循环的状态
      常用的属性:
      	count:记录次数
      	current:当前遍历的内容
  ```

  例如：

  ``` jsp
  <c:forEach begin="1" end="20" step="2" var="i" varStatus="vs">
      ${i }--${vs.count }--${vs.current }<br>
  </c:forEach>
  ```

- ②格式2：

  ``` jsp
  <c:forEach items="${el获取域中的容器}" var="n">
      ${n }
  </c:forEach>
  ```

  例如：

  ``` jsp
  //遍历list
  <c:forEach items="${list }" var="n">
      ${n }
  </c:forEach>
  
  //遍历map
  <c:forEach items="${map }" var="en">
      ${en.key }-- ${en.value }<br/>
  </c:forEach>
  ```



# 4. 关于el、jstl的补充

**（1）JSTL**

在 JSP 页面中，使用标签库代替传统的 Java 片段语言来实现页面的显示逻辑已经不是新技术了，然而，由自定义标签很容易造成重复定义和非标准的实现。鉴于此，出现了 JSTL（JSP Standard Tag Library），为大多数 JSP 页面逻辑提供了实现的 JSTL 技术，该技术本身就是一个标签库。

Sun 公司 Java 规范标准的 JSTL 由 apache jakarta 组织负责维护。作为开源的标准技术，它一直在不断地完善。 JSTL 的发布包有两个版本：Standard-1.0 Taglib、Standard-1.1 Taglib，它们在使用时是不同的。

- Standard-1.0 Taglib（JSTL1.0）支持 Servlet2.3 和 JSP1.2 规范，Web 应用服务器 Tomcat4 支持这些规范，而它的发布也在 Tomcat 4.1.24 测试通过了。
- Standard-1.1 Taglib（JSTL1.1）支持 Servlet2.4 和 JSP2.0 规范，Web 应用服务器 Tomcat5 支持这些规范，它的发布在 Tomcat 5.0.3 测试通过了。

Sun 发布的标准 `JSTL1.1` 标签库有以下几个标签：

``` xml
核心标签库：包含Web应用的常见工作，比如：循环、表达式赋值、基本输入输出等。
国际化标签库：用来格式化显示数据的工作，比如：对不同区域的日期格式化等。
数据库标签库：可以做访问数据库的工作。
XML标签库：用来访问XML文件的工作，这是JSTL标签库的一个特点。
函数标签库：用来读取已经定义的某个函数。
```

此外，JSTL 还提供了 EL 表达式语言（Expression Language）来进行辅助的工作。

 JSTL 标签库由标签库和EL表达式语言两个部分组成。EL 在 JSTL 1.0 规范中被引入，当时用来作为 Java 表达式来工作，而该表达式必须配合 JSTL 的标签库才能得到需要的结果。

说明：在 JSTL 1.1 规范中，JSP2.0 容器已经能够独立的理解任何 EL 表达式。EL 可以独立出现在 JSP 页面的任何角落。

**（2）EL**

EL 是从 JavaScript 脚本语言得到启发的一种表达式语言，它借鉴了 JavaScript 多类型转换无关性的特点。在使用 EL 从 scope 中得到参数时可以自动转换类型，因此对于类型的限制更加宽松。Web 服务器对于 request 请求参数通常会以 String 类型来发送，在得到时使用的 Java 语言脚本就应该是 `request.getParameter(“XXX”)`，这样的话，对于实际应用还必须进行强制类型转换。而 EL 就将用户从这种类型转换的繁琐工作脱离出来，允许用户直接使用 EL 表达式取得的值，而不用关心它是什么类型。

*参考：[JSTL EL 详解](https://javawind.net/help/html/jstl_el.htm)*



