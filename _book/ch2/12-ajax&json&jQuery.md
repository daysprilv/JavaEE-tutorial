### 1、ajax

![](http://p35l3ejfq.bkt.clouddn.com/18-6-2/62246928.jpg)

> 异步JavaScript和XML，AJAX 是一种用于创建快速动态网页的技术。通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用 AJAX）如果需要更新内容，必须重载整个网页页面。
>
> AJAX使用的是老的技术，新的思想。完成了RIA的应用：Rich Internet Application.
>
> - 传统方式的开发：所有的数据提交到服务器端进行处理(胖服务器)
> - AJAX的方式开发：有一部分的代码写在客户端。

ajax 作用：

> 完成页面局部刷新而不影响用户的体验。
>
> - 用户名是否已经存在的校验，例如
>
>   ![](http://p35l3ejfq.bkt.clouddn.com/18-6-2/11527495.jpg)
>
>   在注册页面中，当输入了用户名之后，光标离开文本框，显示用户名是否已经存在。用户名是否已经存在，需要到后台的数据库进行查询的.
>
> - 百度信息输入的提示
>
> - ....



#### 1.1 原生 ajax

开发步骤：

``` xml
1.创建一个核心对象 XMLHttpRequest
	- IE将XMLHttpRequest封装到一个ObjectXActive插件中.
	- Firefox直接可以创建XMLHttpRequest.
2.编写一个回调函数
3.编写请求方式和请求的路径(open操作)
4.发送请求(send操作)
```

ajax 的 api 详解：

``` xhtml
常用属性:
    onreadystatechange:检测readyState状态改变的时候
    readyState:ajax核心对象的状态
        0:核心对象创建
        1:调用了open方法
        2:调用了send方法
        3:部分响应已经生成(没有意思)
        4:响应已经完成(使用的是这个状态)
    status:状态码
        if(xmlhttp.readyState==4 && xmlhttp.status==200){
        }
    responseText:响应回来的文本
常用方法:
    open("请求方式","请求路径"[,"是否异步"]):设置请求的方式和请求的路径
    send(["参数"]):发送请求 参数是请求方式为post的时候的参数
    setRequestHeader("content-type","form表单enctype属性"):设置post请求的参数的类型 必须放在send方法之前.
```

**readyState：** 

![](http://p35l3ejfq.bkt.clouddn.com/18-6-2/2549562.jpg)

代码例子1：

``` js
function btnClick(){
		//1.创建核心对象
		xmlhttp=null;
		if (window.XMLHttpRequest)
		  {// code for Firefox, Opera, IE7, etc.
		  	xmlhttp=new XMLHttpRequest();
		  }
		else if (window.ActiveXObject)
		  {// code for IE6, IE5
		 	 xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
		  }
		//2.编写回调函数
		xmlhttp.onreadystatechange=function(){
			//alert(xmlhttp.readyState);
			if(xmlhttp.readyState==4 && xmlhttp.status==200){
				//alert('ok');
				//接受服务器回送过来的数据
				alert(xmlhttp.responseText);
			}
		}	
		//3.open 设置请求的方式和请求路径
		xmlhttp.open("get","${pageContext.request.contextPath}/ajax1");
		//4.send 发送请求
		xmlhttp.send();
	}
```

##### 1.1.1 Ajax 的GET 入门

``` js
创建XMLHttpRequest
function createXMLHttpRequest() {
	var xmlHttp;
	try { // Firefox, Opera 8.0+, Safari
		xmlHttp = new XMLHttpRequest();
	} catch (e) {
		try {// Internet Explorer
			xmlHttp = new ActiveXObject("Msxml2.XMLHTTP");
		} catch (e) {
			try {
				xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
			} catch (e) {
			}
		}
	}

	return xmlHttp;
}

AJAX的代码：
function loadData() {
	// 1.创建异步XMLHttpRequest对象
	var xhr = createXMLHttpRequest();
	// 2.设置状态改变触发一个函数
	xhr.onreadystatechange = function(){
		// 回调函数.
		if(xhr.readyState == 4){// 请求发送完成
			if(xhr.status == 200){// 响应也正确
				var data = xhr.responseText;
				document.getElementById("d1").innerHTML=data;
			}
		}
	}
	// 3.打开一个连接：
	xhr.open("GET","/WEB15/ServletDemo1",true);

	// 4.发送请求
	xhr.send(null);
}
```



##### 1.1.2 Ajax 的POST 入门

``` js
function loadData(){
	// 1.创建异步对象
	var xhr = createXMLHttpRequest();
	// 2.设置状态改变触发的函数
	xhr.onreadystatechange = function(){
		if(xhr.readyState == 4){
			if(xhr.status == 200){
				document.getElementById("d1").innerHTML=xhr.responseText;
			}
		}
	}
	// 3.打开连接
	xhr.open("POST","/WEB15/ServletDemo2",true);
	xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
	// 4.发送数据
	xhr.send("name=李四&password=456");
}
```



### 1.2 jQuery 的 ajax

> 由于传统 ajax 开发代码比较麻烦，而且还有浏览器兼容的问题。传统的 ajax 的使用在企业中是很少的。使用AJAX的一些框架完成。

四种方式：

``` xml
jquery对象.load(url,params,function(数据){});
////////////////////////////////////////////////////////////////////
$.get(url,params,function(数据){},type);
	发送get请求的ajax
        url:请求的路径
        params:请求的参数 参数为key\value的形式 key=value  {"":"","":""}
        fn:回调函数 参数就是服务器发送回来的数据
        type:返回内容格式，xml, html, script, json, text, _default。    以后用"json"
/////////////////////////////////////////////////////////////////////
$.post(url,params,function(数据){},type);
发送post请求的ajax

若结果为json格式,  打印返回值的时候是一个对象 
    例如若json为 {"result":"success","msg":"成功"}
    获取msg 只需要	参数.msg
///////////////////////////////////////////////////////////////////////
$.ajax([选项]);
	选项的可选值:
        url:请求路径
        type:请求方法
        data:发送到服务器的数据
        success:fn 成功以后的回调
        error:fn 异常之后的回调
        dataType:返回内容格式 经常使用json
        async:设置是否是异步请求
	例如:
        $.ajax({
            url:"/day15/demo1",
            type:"post",
            data:"username=tom",
            success:function(d){
            alert(d.msg);
            },
            error:function(){},
            dataType:"json"
        });	
```

使用 jQuery 的 load 方法：

``` js
$(function(){
	// 给按钮1绑定一个click事件：
	$("#bt1").click(function(){
		$("#d1").load("/WEB15/ServletDemo4",{"name":"张三","password":"123"},function(data){
			if(data == 1){
				$(this).html("张三");
			}else{
				$(this).html("其他");
			}
		});
       });
});
```

使用 jQuery 的 get 方法：

``` js
$(function(){
	$("#bt2").click(function(){
		$.get("/WEB15/ServletDemo4",{"name":"李四","password":"345"},function(data){
			$("#d2").html(data);
		});
	});
});
```

使用 jQuery 的 post 方法：

``` js
$(function(){
	$("#bt3").click(function(){
		$.post("/WEB15/ServletDemo4",{"name":"王五","password":"456"},function(data){
			$("#d3").html(data);
		});
	});
});
```

使用 jQuery 的 ajax 方法：

``` xhtml
$(function(){
	$("#bt4").click(function(){
		$.ajax({
			type:"post",
			url:"/WEB15/ServletDemo4",
			data:"name=aaa&password=123",
			success:function(data){
				$("#d4").html(data);
			}	
		});
	});
});
```

////////////////////////////////////////////////////////

代码例子：

``` js
$(function(){
	// 给用户名的文本框绑定一个事件：
	$("#username").blur(function(){
		// 获得文本框的值:document.getELementById().value;
		var username = $(this).val();
		// 使用jquery的ajax的操作异步发送请求.
		$.post("/WEB15/ServletDemo3",{"username":username},function(data){
			if(data==1){
				// 用户名可以使用
				$("#s1").html("<font color='green'>用户名可以使用</font>");
				$("#regBut").prop("disabled",false);
			}else if(data==2){
				// 用户名已经存在
				$("#s1").html("<font color='red'>用户名已经被占用</font>");
				$("#regBut").prop("disabled",true);
			}
		});
	});
});

```

### 1.3 json

json 的概念：

> **JSON**（**J**ava**S**cript **O**bject **N**otation）是一种由[道格拉斯·克罗克福特](https://zh.wikipedia.org/wiki/%E9%81%93%E6%A0%BC%E6%8B%89%E6%96%AF%C2%B7%E5%85%8B%E7%BE%85%E5%85%8B%E7%A6%8F%E7%89%B9)构想和设计、轻量级的[数据交换语言](https://zh.wikipedia.org/w/index.php?title=%E8%B3%87%E6%96%99%E4%BA%A4%E6%8F%9B%E8%AA%9E%E8%A8%80&action=edit&redlink=1)，该语言以易于让人阅读的文字为基础，用来传输由属性值或者序列性的值组成的数据对象。尽管JSON是[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)的一个子集，但JSON是独立于语言的[文本格式](https://zh.wikipedia.org/wiki/%E6%96%87%E6%9C%AC%E6%96%87%E4%BB%B6)，并且采用了类似于[C语言](https://zh.wikipedia.org/wiki/C%E8%AA%9E%E8%A8%80)家族的一些习惯。
>
> JSON 数据格式与语言无关，脱胎于 [JavaScript](https://zh.wikipedia.org/wiki/JavaScript)，但目前很多[编程语言](https://zh.wikipedia.org/wiki/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80)都支持 JSON 格式数据的生成和[解析](https://zh.wikipedia.org/wiki/%E8%AF%AD%E6%B3%95%E5%88%86%E6%9E%90%E5%99%A8)。JSON 的官方 [MIME 类型](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E5%AA%92%E4%BD%93%E7%B1%BB%E5%9E%8B)是 `application/json`，文件扩展名是 `.json`。

JSON用于描述资料结构，有两种结构存在：

- 对象（object）：一个对象包含一系列非排序的名称／值对(pair)，一个对象以**{**开始，并以**}**结束。每个名称／值对之间使用**:**分区。格式：`{"key":value,"key1":value}`
- 数组 (array)：一个数组是一个值(value)的集合，一个数组以**[**开始，并以**]**结束。数组成员之间使用**,**分区。格式：`[e1,e2]`

具体的格式如下：

- 名称／值（pair）：名称和值之间使用**：**隔开，一般的形式是：


```js
{name:value}
```

一个名称是一个字符串； 一个值(value)可以是一个字符串(string)，一个数值(number)，一个对象(object)，一个布尔值(bool)，一个有序列表(array)，或者一个null值。

- 字符串：以**""**括起来的一串字符。
- 数值：一系列0-9的数字组合，可以为负数或者小数。还可以用**e**或者**E**表示为指数形式。
- 布尔值：表示为true或者false。
- 值的有序列表（array）：一个或者多个值用**,**分区后，使用**[**，**]**括起来就形成了这样的列表，形如：

```js
[value, value]
```

///////////////////////////////////////////

举例：

``` json
{
     "firstName": "John",
     "lastName": "Smith",
     "sex": "male",
     "age": 25,
     "address": 
     {
         "streetAddress": "21 2nd Street",
         "city": "New York",
         "state": "NY",
         "postalCode": "10021"
     },
     "phoneNumber": 
     [
         {
           "type": "home",
           "number": "212 555-1234"
         },
         {
           "type": "fax",
           "number": "646 555-4567"
         }
     ]
 }
```

////////////////////////////////////////////////////////////////////

**将对象转换成 JSON：**（使用 jsonlib 将 java中对象或集合转成 json）

1. 导入 jar 包

2. 使用 API

   ``` xml
   JSONArray.formObject(数组或者list);		//将数组或List集合转成JSON的.
   JSONObject.formObject(bean或者map);	 //将对象或Map集合转成JSON的.
   ```

/////////////////////////////////////////////////////////////////////

**读取 JSON：**

由于JSON是[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)的子集，所以一般都会使用`eval()`作为读取数据的方式，如果是针对可靠的数据来源，在不支持原生JSON解析的浏览器上面这是最快速的方法。然而由于eval方法同样可以执行任意的JavaScript代码，因此当数据来源不可靠时则可能产生安全问题。如下面的例子，直接用eval执行时会跳转：

```js
var json= eval("{message:(function (){ window.location='http://zh.wikipedia.org/wiki/JSON#.E5.AE.89.E5.85.A8.E6.80.A7.E5.95.8F.E9.A1.8C'; })()}");
```

其中一种防止不安全代码出现的解决办法，是通过浏览器原生支持的JSON.parse（str）方法读取JSON数据，目前已经得到大部分主流浏览器的支持（IE8+，Firefox 3.5+，Chrome4+/Safari4+，Opera10+），在不支持原生JSON对象的浏览器上面可以使用`parseJSON`方法进行读取，`parseJSON`采用解析器验证读入的代码是否真的是JSON代码，这样就更安全。但由于这是用模拟的方式读取，速度上会比`eval()`慢。

### 1.4 jQuery 复习

``` xml
获取jquery对象:
	$("选择器") jQuery("选择器");
jquery对象>>dom对象
	方式1:
		jquery对象.get(index);
		
	方式2:
		jquery对象[index]
dom对象>>jquery对象
	$(dom对象)

页面载入
	$(function(){})

派发事件
	jquery对象.事件(function(){...});

选择器:
	#id值  .class值  标签名  [属性]  [属性='值']
	a b:后代    a>b:孩子  a+b:大弟弟  a~b:所有弟弟
	:first :last :odd :even :eq|gt|lt(index)
	:hidden
	:checked  :selected
属性和css:
	prop|attr
	css
	
文本 标签体
	val()
	html() text()

文档处理
	内部插入
		append prepend 
	外部插入
		after before
	删除
		remove
效果:
	隐藏|显示
		show() hide()
	淡入淡出
		fadeIn() fadeOut()
	滑入滑出
		slideDown() slideUp()

遍历
	jquery对象.each(function(){
	});
```





