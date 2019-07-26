
# 1. 会话技术

**会话技术： ** 当用户打开浏览器的时候，访问不同的资源,知道用户将浏览器关闭，可以认为这是一次会话。

作用：因为 http 协议是一个无状态的协议，它记录不论上次访问的内容。用户在访问过程中难免会产生一些数据，通过会话技术就可以将起保存起来。

例如：用户登录、验证码、购物车、访问记录......

分类：

- cookie：浏览器端会话技术
- session：服务器端会话技术

## (1) cookie

### cookie介绍

cookie 是由服务器生成，通过 response 将 cookie 写回浏览器(set-cookie)，保留在浏览器上，下一次访问，浏览器根据一定的规则携带不同的 cookie(通过request的头 cookie)，我们服务器就可以接受 cookie。

``` xml
cookie的api:
	new Cookie(String key,String value)
	setMaxAge(int 秒数);持久化
		若int为0;删除此cookie(前提必须路径一致)
	setPath(String path):设置cookie的路径
		若访问的链接的路径中包含cookie的path,则携带
写回浏览器:
	response.addCookie(Cookie c)
获取cookie:
	Cookie[] request.getCookies()
cookie的常用方法:
    getName():获取cookie的key(名称)
    getValue:获取指定cookie的值
```

查看浏览器的 cookie：在 chrome 浏览器中，f12 打开调试工具，找到 Console 栏（或快捷键 `Ctrl+Shift+j`打开 Console 栏），输入`document.cookie() `查看。

### 案例1-网站访问

``` xml
1.创建一个serlvet RemServlet 路径:/rem
2.在servlet中:
	获取指定cookie 例如:名称为 lastTime
		request.getCookies()
	判断cookie是否为空
		若为空:提示信息 第一次访问
		若不为空:
            获取此cookie的value
            展示信息:你上次访问时间是....

		将这次访问时间记录,写回浏览器
```

代码：

``` java
/**
 * 记录上次访问时间
 */
public class RemServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //0.设置编码
        response.setContentType("text/html;charset=utf-8");
        PrintWriter w = response.getWriter();

        //1.获取指定名称的cookie
        Cookie c=getCookieByName("lastTime",request.getCookies());

        //2.判断cookie是否为空
        if(c == null){
            //cookie为空 提示 第一次访问
            w.print("您是第一次访问!");
        }else{
            //cookie不为空  获取value 展示 上一次访问的时间
            String value = c.getValue();// lastTime=12312324234
            long time = Long.parseLong(value);
            Date date = new Date(time);
            w.print("您上次访问时间:"+date.toLocaleString());
        }

        //3.将当前访问时间记录
        //3.1创建cookie
        c=new Cookie("lastTime",new Date().getTime()+"");

        //持久化cookie
        c.setMaxAge(3600);
        //设置路径
        c.setPath(request.getContextPath()+"/");//  /day11/

        //3.2写回浏览器
        response.addCookie(c);
    }

    /**
	 * 通过名称在cookie数组获取指定的cookie
	 * @param name cookie名称
	 * @param cookies  cookie数组
	 * @return
	 */
    private Cookie getCookieByName(String name, Cookie[] cookies) {
        if(cookies!=null){
            for (Cookie c : cookies) {
                //通过名称获取
                if(name.equals(c.getName())){
                    //返回
                    return c;
                }
            }
        }
        return null;
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

}
```

cookie 总结：

``` xml
常用方法:
	setMaxAge(int 秒):设置cookie在浏览器端存活时间  以秒为单位
		若设置成 0:删除该cookie(前提必须路径一致)
	setPath(String path):设置cookie的路径.
        当我们访问的路径中包含此cookie的path,则携带
        默认路径: 
        	访问serlvet的路径,从"/项目名称"开始,到最后一个"/"结束
        例如:
        	访问的serlvet路径: /day11/a/b/hello
        	默认路径为: /day11/a/b
        手动设置路径: 以"/项目名"开始,以"/"结尾;
```

### 案例2-记录用户浏览历史

需求：当用户访问一个商品的时候，需要将该商品保留在浏览记录中。

步骤分析：

``` markdown
1.先将product_list.htm转成jsp
2.点击一个商品,展示该商品的信息,将该商品id记录到cookie  (GetProductById)
		获取之前的浏览记录 例如名称:ids
		判断cookie是否为空
			若为空 将当前商品的id起个名称 ids 放入cookie中  ids=1
			若不为空,获取值 例如:ids=2-1  当前访问的id=1  使用"-"分割商品id
				判断之前记录中有无该商品
					若有:
						将当前的id放入前面  结果 ids=1-2
					若没有:
						继续判断长度是否>=3
							若>=3,移除最后一个,将当前的id放入最前面
							若<3,直接将当前的id放入最前面.
			
			若 ids=3-2-1 现在访问1 结果 ids=1-3-2
			若 ids=4-3-2 现在访问1 结果 ids=1-4-3
3.再次回到product_list.jsp页面,需要将之前访问商品展示在浏览记录中
		获取ids  例如:ids=2-3-1
		切割
```

### 扩展：删除浏览记录

技术分析：cookie.setMaxAge(0)

步骤分析：

``` xml
1.在浏览器记录中添加一个超链接 
	<a href="/day1101/clearHistroy">清空</a>
2.创建servlet clearHistroy
	创建一个cookie 
		名称路径保持一致
		setMaxAge(0)
	写回浏览器
3.页面跳转
	重定向 product_list.jsp
```

注意：

- cookie不能跨浏览器
- cookie中不支持中文



## (2) session

### session介绍

服务器端的会话技术。

浏览器访问服务器的时候，服务器会获取 jsessionid：

- 若获取到：要拿着这个 jsessionid 去服务器中（session池）查找有无此 session
  - 若查找到了：拿过来直接使用，将该 session 的 jsessionid 通过响应返回给浏览器
  - 若查找不到：创建一个session，将你的数据保存到这个session中，将当前 session 的 jsessionid 返回给浏览器
- 若获取不到：创建一个session，将你的数据保存到这个 session 中，将当前 session 的 jsessionid 返回给浏览器

对 session 的理解：

> 客户到银行办存蓄卡。客户相当于浏览器，存储卡的卡 ID 相当于 jsessionid，银行相当于服务器，服务器响应给浏览器“存储卡ID”（session 底层依赖 cookie）。

详细参考网上文章：[Session机制、持久化、session="false"属性不创建session、显示创建session及其销毁](https://blog.csdn.net/u013210620/article/details/52318884)

### 案例3-添加到购物车

需求：在商品详情页面有一个添加到购物车，点击则将该商品添加到购物车，点击购物车链接将里面的所有商品展示出来。

技术分析：session

获取一个 session：`request.getSession()`

``` markdown
jsp页面上增加 <%@ page session="false"%> 才会让你自己 getSession(true)或 getSession()时创建session
 
1、request.getSession() 等价于 request.getSession(true)
	这两个方法的作用是相同的，查找请求中是否有关联的jsessionid，如果有则返回这个号码所对应的session对象，如果没有则生成一个新的session对象。所以说，通过此方法是一定可以获得一个session对象。
	
2、request.getSession(false) 查找请求中是否有关联的jsessionid号，如果有则返回这个号码所对应的session对象，如果没有则返回一个null。

3、在JSP页面中有一个session的隐士对象，这个隐士对象是怎么产生的呢？我们大家都知道JSP页面最终要被转换成一个Java源文件，实际上这个隐士对象就是通过request.getSession(true)这个方法获得的，所以总是可以获得session对象的。如果设置了<%@ page session=false%>指令，容器不会调用以上方法，而并不是说以上方法不会返回session对象。
```

**session域对象：**`xxxAttribute`

- 生命周期：

  - 创建：java 程序中第一次使用 `request.getsession()` 方法的时候创建

  - 销毁：

    ``` xml
    ①服务器非正常关闭
    ②session超时
        默认时间超时:30分钟  web.xml有配置 
        手动设置超时:setMaxInactiveInterval(int 秒) 
    ③手动干掉session
    	★session.invalidate()
    ```

其他域对象：

- ServletContext：共享的数据
- HttpServletRequest：一次请求的数据
- HttpSession：私有的数据

步骤分析：

``` xhtml
1.点击添加到购物车的时候,提交到一个servlet add2CartServlet
		需要将商品名称携带过去
2.add2CartServlet中的操作
		获取商品的名称
		将商品添加到购物车 购物车的结构 Map<String 名称,Integer 购买数量>
			将购物车放入session中就可以了
		
		将商品添加到购物车分析:
			获取购物车
			判断购物车是否为空
				若为空:
					第一次添加
					创建一个购物车
					将当前商品put进去.数量:1
					将购物车放入session中
				若不为空:继续判断购物车中是否有该商品
					若有:
						取出count 将数量+1 
						将商品再次放入购物车中
					若没有:
						将当前商品put进去 数量:1
					
		提示信息:你的xx已添加到购物车中
	
	3.点击购物车连接的时候 cart.jsp
		从session获取购物车
			判断购物车是否为空
				若为空:提示信息
				若不为空:遍历购物车即可
```

代码：

``` java
/**
 * 添加到购物车
 */
public class Add2CartServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		//0.设置编码
		response.setContentType("text/html;charset=utf-8");
		PrintWriter w = response.getWriter();
		//1.获取商品的名称
		String name=request.getParameter("name");
		name=new String(name.getBytes("iso8859-1"),"utf-8");
		//2.将商品添加到购物车
		//2.1 从session中获取购物车
		Map<String,Integer> map=(Map<String, Integer>) request.getSession().getAttribute("cart");
		Integer count=null;
		//2.2判断购物车是否为空
		if(map==null){
			//第一次购物  创建购物车 
			map=new HashMap<>();
			//将购物车放入session中g
			request.getSession().setAttribute("cart", map);
			count=1;
		}else{
			//购物车不为空 继续判断购物车中是否有该商品
			count = map.get(name);
			if(count==null){
				//购物车中没有该商品
				count=1;
			}else{
				//购物车中有该商品
				count++;
			}
		}
		//将商品放入购物车中
		map.put(name, count);
		
		//3.提示信息
		w.print("已经将<b>"+name+"</b>添加到购物车中<hr>");
		w.print("<a href='"+request.getContextPath()+"/product_list.jsp'>继续购物</a>&nbsp;&nbsp;&nbsp;&nbsp;");
		w.print("<a href='"+request.getContextPath()+"/cart.jsp'>查看购物车</a>&nbsp;&nbsp;&nbsp;&nbsp;");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}
}
```

### 案例2-扩展清空购物车:

``` xml
思路1:将购物车移除
思路2:将session干掉
步骤分析:
	在cart.jsp上添加一个超链接 清空购物车
		<a href="/day1101/clearCart">清空购物车</a>
	在clearCart中需要调用session.invalidate()
	重定向到购物车页面
```

