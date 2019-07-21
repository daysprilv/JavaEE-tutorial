
# 1. Ajax

AJAX 不是 JavaScript 的规范，它只是一个哥们“发明”的缩写：Asynchronous JavaScript and XML，意思就是用 JavaScript 执行异步网络请求。

如果仔细观察一个 Form 的提交，你就会发现，一旦用户点击“Submit”按钮，表单开始提交，浏览器就会刷新页面，然后在新页面里告诉你操作是成功了还是失败了。如果不幸由于网络太慢或者其他原因，就会得到一个 404 页面。

这就是 Web 的运作原理：一次 HTTP 请求对应一个页面。

如果要让用户留在当前页面中，同时发出新的 HTTP 请求，就必须用J avaScript 发送这个新请求，接收到数据后，再用 JavaScript 更新页面，这样一来，用户就感觉自己仍然停留在当前页面，但是数据却可以不断地更新。 

- AJAX 不是新的编程语言，而是一种使用现有标准的新方法。
- AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。
- AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

- AJAX使用的是老的技术，新的思想。完成了 RIA 的应用：Rich Internet Application.
  - 传统方式的开发：所有的数据提交到服务器端进行处理(胖服务器)
  - AJAX的方式开发：有一部分的代码写在客户端。

Ajax 作用：完成页面局部刷新而不影响用户的体验。

- 用户名是否已经存在的校验，例如

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-6-2-11527495.jpg)

在注册页面中，当输入了用户名之后，光标离开文本框，显示用户名是否已经存在。用户名是否已经存在，需要到后台的数据库进行查询的.

- 百度信息输入的提示
- ....



# 2. 原生Ajax

## (1) 开发步骤

开发步骤：

``` xml
1.创建一个核心对象 XMLHttpRequest
	- IE将XMLHttpRequest封装到一个ObjectXActive插件中.
	- Firefox直接可以创建XMLHttpRequest.
2.编写一个回调函数
3.编写请求方式和请求的路径(open操作)
4.发送请求(send操作)
```

Ajax 的 api 详解：

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

| 状态            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| 0（未初始化）   | 对象已建立，但是尚未初始化（尚未调用open方法）               |
| 1（初始化）     | 对象已建立，尚未调用 send 方法                               |
| 2（发送数据）   | send方法已调用，但是当前的状态及http头未知                   |
| 3（数据传送中） | 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分 |
| 4（完成）       | 数据接收完毕，此时可以通过通过responseBody和responseText获取完整的回应数据 |

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

## (2) Ajax的GET入门

``` js
//创建XMLHttpRequest
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

//AJAX的代码：
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

## (3) Ajax的POST入门

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



# 2. jQuery的Ajax

由于传统 ajax 开发代码比较麻烦，而且还有浏览器兼容的问题。传统的 ajax 的使用在企业中是很少的。使用AJAX的一些框架完成。

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

# 3. JSON(JavaScript Object Notation)

JSON（JavaScript Object Notation）是一种由道格拉斯·克罗克福特构想和设计、轻量级的数据交换语言，该语言以易于让人阅读的文字为基础，用来传输由属性值或者序列性的值组成的数据对象。尽管 JSON 是 JavaScript 的一个子集，但JSON是独立于语言的文本格式，并且采用了类似于 C 语言家族的一些习惯。

JSON 数据格式与语言无关，脱胎于 JavaScript，但目前很多编程语言都支持 JSON 格式数据的生成和解析。JSON 的官方MIME 类型是 `application/json`，文件扩展名是 `.json`。

JSON 用于描述资料结构，有两种结构存在：

- 对象（object）：一个对象包含一系列非排序的名称／值对(pair)，一个对象以 `{` 开始，并以 `}` 结束。每个名称／值对之间使用**:**分区。格式：`{"key":value,"key1":value}`
- 数组（array）：一个数组是一个值(value)的集合，一个数组以 `[` 开始，并以 `]` 结束。数组成员之间使用 `,` 分区。格式：`[e1,e2]`

具体的格式如下：

- 名称／值（pair）：名称和值之间使用 `：` 隔开，一般的形式是：`{name:value}`

  一个名称是一个字符串； 一个值(value)可以是一个字符串(string)，一个数值(number)，一个对象(object)，一个布尔值(bool)，一个有序列表(array)，或者一个 null 值。

- 字符串：以 `""` 括起来的一串字符。
- 数值：一系列0-9的数字组合，可以为负数或者小数。还可以用**e**或者**E**表示为指数形式。
- 布尔值：表示为 true 或者 false。
- 值的有序列表（array）：一个或者多个值用 `,` 分区后，使用 `[`， `]` 括起来就形成了这样的列表，形如：`[value, value]`

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

**将对象转换成 JSON：**（使用 jsonlib 将 java中对象或集合转成 json）

1. 导入 jar 包

2. 使用 API

   ``` xml
   JSONArray.formObject(数组或者list);		//将数组或List集合转成JSON的.
   JSONObject.formObject(bean或者map);	 //将对象或Map集合转成JSON的.
   ```

**读取 JSON：**

由于 JSON 是 JavaScript 的子集，所以一般都会使用 `eval()` 作为读取数据的方式，如果是针对可靠的数据来源，在不支持原生 JSON 解析的浏览器上面这是最快速的方法。然而由于 eval 方法同样可以执行任意的 JavaScript 代码，因此当数据来源不可靠时则可能产生安全问题。如下面的例子，直接用 eval 执行时会跳转：

```js
var json= eval("{message:(function (){ window.location='http://zh.wikipedia.org/wiki/JSON#.E5.AE.89.E5.85.A8.E6.80.A7.E5.95.8F.E9.A1.8C'; })()}");
```

其中一种防止不安全代码出现的解决办法，是通过浏览器原生支持的 JSON.parse（str）方法读取 JSON 数据，目前已经得到大部分主流浏览器的支持（IE8+，Firefox 3.5+，Chrome4+/Safari4+，Opera10+），在不支持原生JSON 对象的浏览器上面可以使用 `parseJSON` 方法进行读取，`parseJSON`  采用解析器验证读入的代码是否真的是JSON 代码，这样就更安全。但由于这是用模拟的方式读取，速度上会比 `eval()`慢。



# 4. jQuery 复习

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

