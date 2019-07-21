
## 前言

Spring 第二天学习大纲：

``` xml
1. 高级参数绑定
   1. 数组类型的参数绑定
   2. List 类型的绑定
2. @RequestMapping 注解的使用
3. Controller 方法返回值
4. SpringMVC 中异常处理
5. 图片上传处理
6. Json 数据交互
7. SpringMVC 实现 Restful
8. 拦截器
```



## 一、高级参数绑定

### 1.1 绑定数组

需求：在商品列表页面选中多个商品，然后删除。

需求分析：此功能要求商品列表页面中的每个商品前有一个 checkbook，选中多个商品后点击删除按钮把商品 id 传递给 Controller，根据商品 id 删除商品信息。

JSP 中实现：

``` jsp
<c:forEach items="${itemList }" var="item">
    <tr>
        <!-- name属性名称要等于vo中的接收的属性名 -->
        <td><input name="ids" value="${item.id}" type="checkbox"></td>
        <td>${item.name }</td>
        <td>${item.price }</td>
        <td><fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/></td>
        <td>${item.detail }</td>
        <td><a href="${pageContext.request.contextPath }/itemEdit.action?id=${item.id}">修改</a></td>
    </tr>
</c:forEach>
```

QueryVo.java 如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-57733118.jpg)



Controller 方法中可以用 String[] 接收，或者 pojo 的 String[] 属性接收。两种方式任选其一即可。定义如下：

``` java
@RequestMapping("/delAll")
public String delAll(QueryVo vo) throws Exception{
    //如果批量删除,一堆input复选框,那么可以提交数组.(只有input复选框被选中的时候才能提交)
    System.out.println(vo);
    return "";
}
```

### 1.2 将表单的数据绑定到List

需求：实现商品数据的批量修改。

需求分析：要想实现商品数据的批量修改，需要在商品列表中可以对商品信息进行修改，并且可以批量提交修改后的商品数据。

接收商品列表的 POJO：List 中存放对象，并将定义的 List 放在包装类中，使用包装 pojo 对象接收。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-74131294.jpg)

页面定义如下：

``` jsp
<c:forEach items="${itemList }" var="item" varStatus="status">
    <tr>
        <td>
            <input type="checkbox" name="ids" value="${item.id }"/>
            <input type="hidden" name="itemsList[${status.index }].id" value="${item.id }"/>
        </td>
        <td><input type="text" name="itemsList[${status.index }].name" value="${item.name }"/></td>
        <td><input type="text" name="itemsList[${status.index }].price" value="${item.price }"/></td>
        <td><input type="text" name="itemsList[${status.index }].createtime" 
                   value="<fmt:formatDate value="${item.createtime}" pattern="yyyy-MM-dd HH:mm:ss"/>"/></td>
        <td><input type="text" name="itemsList[${status.index }].detail" value="${item.detail }"/></td>

        <td><a href="${pageContext.request.contextPath }/itemEdit.action?id=${item.id}">修改</a></td>
    </tr>
</c:forEach>
```

varStatus 属性常用参数总结下：

> ${status.index}     输出行号，从0开始。
>
> ${status.count}     输出行号，从1开始。
>
> ${status.current}  当前这次迭代的（集合中的）项
>
> ${status.first}  判断当前项是否为集合中的第一项，返回值为 true 或 false
>
> ${status.last}  判断当前项是否为集合中的最后一项，返回值为 true 或 false
>
> begin、end、step 分别表示：起始序号，结束序号，跳跃步伐。

批量更新测试：

``` java
@RequestMapping("/updateAll")
public String updateAll(QueryVo vo) throws Exception{
    System.out.println(vo);
    return "";
}
```

> 注意：接收 List 类型的数据必须是 pojo 的属性，方法的形参为 List 类型无法正确接收到数据。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-61134991.jpg)

补充：Controller 方法接收的参数的变量名称必须要等于页面上 input 框的 name 属性值。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-59843346.jpg)

Controller 方法：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-99305323.jpg)



但也可以使用 `@RequestParam(" ")`方式，这样就不需要 input 框的 name 属性名称必须等于 pojo 的属性名称。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-15468710.jpg)



## 二、@RequestMapping注解的使用

通过 RequestMapping 注解可以定义不同的处理器映射规则。

☛ URL路径映射：

> `@RequestMapping(value="/item")或@RequestMapping("/item)`
>
> value 的值是数组，可以将多个 url 映射到同一个方法。

☛ 窄化请求映射：

> 在 class 上添加`@RequestMapping(url)`指定通用请求前缀， 限制此类下的所有方法请求 url 必须以请求前缀开头，通过此方法对 url 进行分类管理。如下：
>
> @RequestMapping 放在类名上边，设置请求前缀：
>
> ``` java
> @Controller
> //窄化请求映射:为防止你和你的队友在conroller方法起名的时候重名,所以相当于在url中多加了一层目录,防止重名
> //例如:当前list的访问路径   localhost:8081/ssm0523-1/items/list.action
> @RequestMapping("/items")
> public class ItemsController {
>
> }
> ```
>
> 方法名上边设置请求映射url：
>
> @RequestMapping放在[方法名上边]()，如下：
>
> ``` java
> @Controller
> @RequestMapping("/items")
> public class ItemsController {
>     @RequestMapping("/list")
>     public ModelAndView itemsList() throws Exception{
>
>     }
> }
> ```
>
> 访问地址：`.../items/list`

☛ 请求方法限定：

1. 限定 GET 方法：`@RequestMapping(method = RequestMethod.GET)`，如果通过POST 访问则报错：

   > HTTP Status 405 - Request method 'POST' not supported

   例如：`@RequestMapping(value="/editItem",method=RequestMethod.GET)`

2. 限定 POST 方法：`@RequestMapping(method = RequestMethod.POST)`，如果通过 GET 访问则报错：

   > HTTP Status 405 - Request method 'GET' not supported

3. GET 和 POST 都可以：`@RequestMapping(method={RequestMethod.GET,RequestMethod.POST})`

   


## 三、Controller方法返回值

☛ **返回 ModeAndView：**

> Controller 方法中定义 ModelAndView 对象并返回，对象中可添加 model 数据、指定 view。

☛ **返回 void：**

在 controller 方法形参上可以定义 request 和 response，使用 request 或 response 指定响应结果：

1、使用 request 转向页面，如下：

``` java
//指定返回的页面(如果controller方法返回值为void,则不走springMvc组件,所以要写页面的完整路径名称)
request.getRequestDispatcher("/WEB-INF/jsp/success.jsp").forward(request, response);
```

2、也可以通过 response 页面重定向：

``` java
//重定向:浏览器中url发生改变,request域中的数据不可以带到重定向后的方法中
response.sendRedirect("url")
```

3、也可以通过 response 指定响应结果，例如响应 json 数据如下：

``` java
response.setCharacterEncoding("utf-8");
response.setContentType("application/json;charset=utf-8");
response.getWriter().write("json串");
```

☛ **返回字符串：**

- 逻辑视图名：Controller 方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。

  ``` java
  //指定逻辑视图名，经过视图解析器解析为jsp物理路径：/WEB-INF/jsp/items/editItem.jsp
  return "items/editItem";
  ```

- redirect 重定向：Contrller 方法返回结果重定向到一个 url 地址，如下，商品修改提交后重定向到商品查询方法，参数无法带到商品查询方法中。

  ``` java
  //重定向到queryItem.action地址,request无法带过去
  return "redirect:queryItem.action"; //在springMvc中凡是以redirect:字符串开头的都为重定向
  ```

  redirect 方式相当于 `response.sendRedirect()`，转发后浏览器的地址栏变为转发后的地址，因为转发即执行了一个新的 request 和 response。由于新发起一个 request 原来的参数在转发时就不能传递到下一个 url，如果要传参数可以`/items/queryItem.action`后边加参数的形式，如：`/items/queryItem?...&…..`

- forward 转发：Controller 方法执行后继续执行另一个 Controller 方法，如下商品修改提交后转向到商品修改页面，修改商品的 id 参数可以带到商品修改方法中。

  ``` java
  //结果转发到editItem.action，request可以带过去
  return "forward:itemEdit.action"; //spirngMvc中请求转发:返回的字符串以forward:开头的都是请求转发
  ```

  forward 方式相当于`request.getRequestDispatcher().forward(request,response)`，转发后浏览器地址栏还是原来的地址。转发并没有执行新的 request 和 response，而是和转发前的请求共用一个 request 和 response，所以转发前请求的参数在转发后仍然可以读取到。

  > 注：① `forward:itemEdit.action`表示相对路径，相对路径就是相对于当前目录，当前为类上面指定的 items 目录，在当前目录下可以使用相对路径随意跳转到某个方法中。
  >
  > ②`forward:/itemEdit.action`路径中以斜杠开头的为绝对路径，绝对路径从项目名后面开始算。

  


## 四、异常处理器

1、异常处理思路

SpringMVC 在处理请求过程中出现异常信息交由异常处理器进行处理，自定义异常处理器可以实现一个系统的异常处理逻辑。

系统中异常包括两类：预期异常和运行时异常 RuntimeException，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。

系统的 dao、service、controller 出现都通过 throwsException 向上抛出，最后由 SpringMVC 前端控制器交由异常处理器进行异常处理。

2、自定义异常类

为了区别不同的异常通常根据异常类型自定义异常类，这里我们创建一个自定义系统异常，如果 controller、service、dao 抛出此类异常说明是系统预期处理的异常信息。

``` java
//自定义异常类,用来处理自定义异常
public class CustomException extends Exception {
    private static final long serialVersionUID = -5212079010855161498L;

    public CustomException(String message){
        super(message);
        this.message = message;
    }
    //异常信息
    private String message;
    public String getMessage() {
        return message;
    }
    public void setMessage(String message) {
        this.message = message;
    }
}
```

3、自定义异常处理器

``` java
public class CustomExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response, Object handler, Exception ex) {
        ex.printStackTrace();
        CustomException customException = null;

        //如果抛出的是系统自定义异常则直接转换
        if(ex instanceof CustomException){
            customException = (CustomException)ex;
        }else{
            //如果抛出的不是系统自定义异常则重新构造一个系统错误异常。
            customException = new CustomException("系统错误，请与系统管理员联系！");
        }

        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("message", customException.getMessage());
        modelAndView.setViewName("error");
        return modelAndView;
    }
}
```

4、错误页面

``` jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%> 
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>错误页面</title>

    </head>
    <body>
        您的操作出现错误如下：<br/>
        ${message }
    </body>
</html>
```

5、异常处理器配置

在 SpringMvc.xml 中添加：

``` xml
<!-- 异常处理器 -->
<bean id="handlerExceptionResolver" class="cn.itheima.exception.CustomExceptionResolver"/>
```

6、异常测试

修改商品信息，id 输入错误提示商品信息不存在。

修改 controller 方法 itemEdit，调用 service 查询商品信息，如果商品信息为空则抛出异常：

``` java
// 调用service查询商品信息
Items item = itemService.findItemById(id);
if(item == null){
    throw new CustomException("商品信息不存在!");
}
```



## 五、上传图片

> 一般上传图片都不会上传到本地机器上，而是上传到一个图片服务器上，一般互联网企业都是要布置集群的，而这个图片服务器就专门用来放图片。
>
> 我们现在没有单独的服务器，但我们可以通过创建虚拟的图片服务器演示，怎么来创建呢？---> 利用 tomcat。

1、配置虚拟目录

在 tomcat 上配置图片虚拟目录，在 tomcat 下 conf/server.xml 中添加：

``` xml
<Context docBase="F:\develop\upload\temp" path="/pic" reloadable="false"/>
```

访问 `http://localhost:8080/pic` 即可访问 F:\develop\upload\temp 文件夹下的图片。

也可以通过 eclipse 配置：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-57798767.jpg)



2、在上传图片的表单页面我们都会添加这个 `enctype="multipart/form-data"`

> 从页面上传后，都是是以流的形式存在，所以我们需要解析上传的内容。因此需要导入相关 jar 包。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-36371663.jpg)
>
> PS：CommonsMultipartResolver 解析器依赖 commons-fileupload 和 commons-io。

3、配置解析器，在 SpringMvc.xml 文件中添加：

``` xml
<!-- 文件上传 -->
<bean id="multipartResolver"
      class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 设置上传文件的最大尺寸为5MB -->
    <property name="maxUploadSize">
        <value>5242880</value>
    </property>
</bean>
```

4、图片上传，controller：

``` java
public String update(MultipartFile pictureFile,Items items, Model model, HttpServletRequest request) throws Exception{
    //1. 获取图片完整名称
    String fileStr = pictureFile.getOriginalFilename();
    //2. 使用随机生成的字符串+源图片扩展名组成新的图片名称,防止图片重名
    String newfileName = UUID.randomUUID().toString() + fileStr.substring(fileStr.lastIndexOf("."));
    //3. 将图片保存到硬盘
    pictureFile.transferTo(new File("F:\\develop\\upload\\temp" + newfileName));
    //4.将图片名称保存到数据库
    items.setPic(newfileName);
    itmesService.updateItems(items);

    ......

```

页面：

``` jsp
<!-- 上传图片是需要指定属性 enctype="multipart/form-data" -->
<form id="itemForm" action="${pageContext.request.contextPath }/item/editItemSubmit.action" method="post" enctype="multipart/form-data">
    <input type="hidden" name="pic" value="${item.pic }" />

```

file 的 name 与 controller 形参一致：

``` jsp
<tr>
    <td>商品图片</td>
    <td><c:if test="${item.pic !=null}">
        <img src="/pic/${item.pic}" width=100 height=100 />
        <br />
        </c:if> <input type="file" name="pictureFile" /></td>
</tr>
```

步骤小结：

``` xml
1)在tomcat中配置虚拟图片服务器
2)导入fileupload的jar包
3)在springMvc.xml中配置上传组件
4)在页面上编写上传域,更改form标签的类型
5)在controller方法中可以使用MultiPartFile接口接收上传的图片
6)将文件名保存到数据库,将图片保存到磁盘中
```



## 六、json数据交互

☛ @RequestBody

> `@RequestBody`注解用于读取 http 请求的内容（字符串），通过 SpringMVC 提供的 HttpMessageConverter 接口将读到的内容转换为 json、xml 等格式的数据并绑定到 controller 方法的参数上。
>
> 本例子的应用：@RequestBody 注解实现接收 http 请求的 json 数据，将 json 数据转换为 java 对象
>
> ``` java
> List.action?id=1&name=zhangsan&age=12
> ```

☛ @ResponseBody

> 作用：该注解用于将 Controller 的方法返回的对象，通过 HttpMessageConverter 接口转换为指定格式的数据如：json,xml 等，通过 response 响应给客户端。
>
> 本例子的应用：@ResponseBody 注解实现将 controller 方法返回对象转换为 json 响应给客户端
>
> ``` java
> fdaf
> ```

☛ 案例：请求 json，响应 json 实现

① 环境准备：SpringMVC 默认用 MappingJacksonHttpMessageConverter 对 json 数据进行转换，需要加入 jackson 的包，如下：![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-34458630.jpg)



② 配置 json 转换器。在 SpringMVC 配置文件中的注解适配器中加入messageConverters

``` xml
<!--注解适配器 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list>
            <bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"></bean>
        </list>
    </property>
</bean>
```

但是注意：如果使用了`<mvc:annotation-driven />`则不用定义上边的内容。![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-68666969.jpg)

③ controller 编写

``` java
//导入jackson的jar包在 controller的方法中可以使用@RequestBody,让spirngMvc将json格式字符串自动转换成java中的pojo
//页面json的key要等于java中pojo的属性名称
//controller方法返回pojo类型的对象并且用@ResponseBody注解,springMvc会自动将pojo对象转换成json格式字符串
@RequestMapping("/sendJson")
@ResponseBody
public Items json(@RequestBody Items items) throws Exception{
    System.out.println(items);
    return items;
}
```

④ 页面 js 编写

``` js
<script type="text/javascript">
    function sendJson(){
        //请求json响应json
        $.ajax({
            type:"post",
            url:"${pageContext.request.contextPath }/items/sendJson.action",
            contentType:"application/json;charset=utf-8",
            data:'{"name":"测试商品","price":99.9}',
            success:function(data){
                alert(data);
            }
    });
}
</script>
```

调试结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-74875899.jpg)



## 七、RESTful支持

1、什么是 restful？

> restful 就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格，是对 http 协议的诠释。
>
> 资源定位：互联网所有的事物都是资源，要求 url 中没有动词，只有名词，没有参数
>
> - 例如：`http://blog.csdn.net/beat_the_world/article/details/45621673`
>
> 资源操作：使用 put、delete、post、get 使用不同方法对资源进行操作。分别对应添加、删除、修改、查询。一般使用时还是 post 和 get。put 和 delete 几乎不使用。

2、以案例来学习

需求：RESTful 方式实现商品信息查询，返回 json 数据

① 添加 DispatcherServlet 的 rest 配置，web.xml：

``` xml
<!-- springmvc前端控制器 -->
<servlet>
    <servlet-name>springMvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:SpringMvc.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>springMvc</servlet-name>
    <!-- 
       *.action    代表拦截后缀名为.action结尾的
       / 			拦截所有但是不包括.jsp
       /* 			拦截所有包括.jsp
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

② URL 模板模式映射

`@RequestMapping(value="/ viewItems/{id}")`：`{×××}`占位符，请求的 URL 可以是 “/viewItems/1” 或 “/viewItems/2” ，通过在方法中使用`@PathVariable`获取 {×××} 中的 ××× 变量。

`@PathVariable`：用于将请求URL中的模板变量映射到功能处理方法的参数上。

``` java
/*
	*通过@PathVariable可以接收url中传入的参数
	 *@RequestMapping("/itemEdit/{id}")中接收参数使用大括号中加上变量名称, @PathVariable中的变量名称要和RequestMapping中的变量名称相同
*/
@RequestMapping("/itemEdit/{id}")
public String itemEdit(@PathVariable("id") Integer id, HttpServletRequest reuqest, 
                       Model model) throws Exception{
    Items items = itmesService.findItemsById(id);
    model.addAttribute("item", items);
    return "editItem";
}
```

页面定义：

``` jsp
<td><a href="${pageContext.request.contextPath }/items/itemEdit/${item.id}">修改</a></td>
```

注：重定向，浏览器中 url 发生改变，request 域中的数据不可以带到重定向后的方法中，但是使用 restful 风格方式，如下：

``` java
return "redirect:itemEdit/"+items.getId();
```

这个可以带回去。此方式通过 URL 字符串形式，不算是 request 域的东西。



## 八、拦截器

### 8.1 拦截器学习

Spring Web MVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器进行预处理和后处理。

SpringMVC 拦截器流程图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-11224191.jpg)

① 拦截器的定义，实现 HandlerInterceptor 接口，如下：

``` java
public class Interceptor1 implements HandlerInterceptor {

    //执行时机:controller已经执行,modelAndview已经返回
    //使用场景: 记录操作日志,记录登录用户的ip,时间等.
    @Override
    public void afterCompletion(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, Exception arg3)
        throws Exception {
        System.out.println("======Interceptor1=======afterCompletion========");
    }

    //执行时机:Controller方法已经执行,ModelAndView没有返回
    //使用场景: 可以在此方法中设置全局的数据处理业务
    @Override
    public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, ModelAndView arg3)
        throws Exception {
        System.out.println("======Interceptor1=======postHandle========");

    }

    //返回布尔值:如果返回true放行,返回false则被拦截住
    //执行时机:controller方法没有被执行,ModelAndView没有被返回
    //使用场景: 权限验证
    @Override
    public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2) throws Exception {
        System.out.println("======Interceptor1=======preHandle========");
        return true;
    }
}
```

② 配置拦截器，在 SpringMvc.xml 文件添加：

``` xml
<!-- 配置拦截器 -->
<mvc:interceptors>
    <mvc:interceptor>
        <!-- 拦截请求的路径    要拦截所有必需配置成 /** -->
        <mvc:mapping path="/**"/>
        <!-- 指定拦截器的位置 -->
        <bean class="cn.itheima.interceptor.Interceptor1"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

运行结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-35882261.jpg)

### 8.2 多个拦截器执行顺序

1、定义两个拦截器分别为：HandlerInterceptor1 和 HandlerInterceptor2

``` xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="cn.itheima.interceptor.HandlerInterceptor1"></bean>
    </mvc:interceptor>
     <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="cn.itheima.interceptor.HandlerInterceptor2"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

每个拦截器的 preHandler 方法都返回 true

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-27-66905152.jpg)

运行流程：

``` xml
HandlerInterceptor1..preHandle..
HandlerInterceptor2..preHandle..

HandlerInterceptor2..postHandle..
HandlerInterceptor1..postHandle..

HandlerInterceptor2..afterCompletion..
HandlerInterceptor1..afterCompletion..

```

2、定义两个拦截器分别为：HandlerInterceptor1 和 HandlerInteptor2

HandlerInterceptor1 的 preHandler 方法返回 false，HandlerInterceptor2 返回 true，运行流程如下：

``` xml
HandlerInterceptor1..preHandle..
```

从日志看出第一个拦截器的 preHandler 方法返回 false 后第一个拦截器只执行了 preHandler 方法，其它两个方法没有执行，第二个拦截器的所有方法不执行，且 controller 也不执行了。

HandlerInterceptor1 的 preHandler 方法返回 true，HandlerInterceptor2 返回 false，运行流程如下：

``` xml
HandlerInterceptor1..preHandle..
HandlerInterceptor2..preHandle..
HandlerInterceptor1..afterCompletion..
```

从日志看出第二个拦截器的 preHandler 方法返回 false 后第一个拦截器的 postHandler 没有执行，第二个拦截器的 postHandler 和 afterCompletion 没有执行，且 controller 也不执行了。

总结：

``` xml
preHandle 按拦截器定义顺序调用
postHandler 按拦截器定义逆序调用
afterCompletion 按拦截器定义逆序调用

postHandler 在拦截器链内所有拦截器返成功调用
afterCompletion 只有preHandle返回true才调用
```

### 8.3 拦截器应用

1、处理流程

``` xml
1、有一个登录页面，需要写一个controller访问页面
2、登录页面有一提交表单的动作。需要在controller中处理。
	a)	判断用户名密码是否正确
	b)	如果正确，向session中写入用户信息
	c)	返回登录成功，或者跳转到商品列表
3、拦截器。
	a)	拦截用户请求，判断用户是否登录
	b)	如果用户已经登录。放行
	c)	如果用户未登录，跳转到登录页面。
```

2、用户身份认证

``` java
public class LoginInterceptor implements HandlerInterceptor{
    @Override
    Public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response, Object handler) throws Exception {
        //如果是登录页面则放行
        if(request.getRequestURI().indexOf("login.action")>=0){
            return true;
        }
        HttpSession session = request.getSession();
        //如果用户已登录也放行
        if(session.getAttribute("user")!=null){
            return true;
        }
        //用户没有登录跳转到登录页面
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request, response);
        return false;
    }
}
```

3、用户登录 controller

``` java
//登陆页面
@RequestMapping("/login")
public String login(Model model)throws Exception{
    return "login";
}

//登陆提交
//userid：用户账号，pwd：密码
@RequestMapping("/loginsubmit")
public String loginsubmit(HttpSession session,String userid,String pwd)throws Exception{
    //向session记录用户身份信息
    session.setAttribute("activeUser", userid);
    return "redirect:item/queryItem.action";
}

//退出
@RequestMapping("/logout")
public String logout(HttpSession session)throws Exception{
    //session过期
    session.invalidate();
    return "redirect:item/queryItem.action";
}
```
