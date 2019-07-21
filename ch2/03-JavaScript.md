
# 1. JavaScript介绍

JavaScript：一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，

内置支持类型。它的解释器被称为 JavaScript 引擎，为浏览器的一部分，广泛用于客户端的脚本语言。

组成部分：

- **ECMAScript：** JS 基础语法(规定 关键字 运算符 语句 函数等等...)
- **BOM：** 浏览器对象模型
- **DOM：** 文档对象模型

作用：修改 html 页面的内容；修改 html 的样式；完成表单的验证。

注意：JS 可以在页面上直接写，也可以单独出去；JS 的文件的后缀名 `.js`

JS 和 HTML 的整合：

- 方式1：在页面上直接写，将 js 代码放在 `<script></script>` 标签中,一般放在head 标签中

  ``` js
  <script type="text/javascript">
      funtion fuc1(){
      alert(111);
      }
  </script>
  ```

- 方式2：独立的 js 文件，通过 script 标签的 src 属性导入。例如：`<script type="text/javascript" src="test.js" ></script>`



# 2. JavaScript基础

JavaScript 中的变量声明：  

- var 变量名=初始化值;
- var 变量名; 变量名=初始化值;

注：va r可以省略，建议不要省略；一行要以分号结尾，最后一个分号可以省略,建议不要省略。

JavaScript  的数据类型：

原始类型（5种）

- Null
- String
- Number
- Boolean
- Undefined

通过 typeof 运算符可以判断一个值或者变量是否属于原始类型，若属于原始类型，还可以判断出属于那种原始类型。`typeof 变量|值;`

若变量为 null，使用 typeof 弹出的值 object

使用 typeof 的返回值：

- undefined：如果变量是 Undefined 类型的 
- boolean：如果变量是 Boolean 类型的 
- number：如果变量是 Number 类型的 
- string：如果变量是 String 类型的 
- object：如果变量是一种引用类型或 Null 类型的 

引用类型

运算符：

- 关系运算符：
    - 两边值都是字符，比较 ascii 码
    - 两边都是数字，和 java 一样
    - 一边是数字，一遍是字符串形式的数字，例如：`3>"2"` 可以比较，将字符串形式的数字转换成数字在进行比较。
    - 一边是数字，一遍是普通字符串，例如：`3>"hello"` 可以比较，值永远是 false

- 等性运算符：`==` 只判断值是否相同；`===` 不仅判断是否相同,还要判断类型是否相同
  ``` js
  "66"==66  true
  "666"===666 false
  ```

JavaScript 的语句：if、while、for 等和 java 一样，switch 和 java 一样（区别：switch 后面跟字符串，还可以跟变量）

JavaScript 操作：

- 获取元素：`var obj=document.getElementById("id值");`
- 获取元素的value属性：`var val=document.getElementById("id值").value;`
- 设置元素的value属性：`document.getElementById("id值").value="sdfsdf";`
- 获取元素的标签体：`var ht=document.getElementById("id值").innerHTML;
- 设置元素的标签体：`document.getElementById("id值").innerHTML="ssss";`

定义函数：

- 方式1：function 函数名(参数列表){函数体}
- 方式2：var 函数名=function(参数列表){函数体}

  注意：函数声明的时候不用声明返回值类型；参数列表上不要写参数类型；通过return 结束方法和将值返回；函数调用的时候：`函数名(参数)`

事件：

- onclick：单击
- onsubmit：表单提交，加在 form 表单上的 `onsubmit="return 函数名()"` ，函数返回值为 boolean 类型
- onload：页面加载成功或者元素加载成功

事件和函数绑定：

- 方式1：通过元素的事件属性，`<xxx onxxx="函数名(参数列表)">`，若参数为 this 是将当前的 dom 对象传递给了函数

- 方式2：给元素派发事件（相当于给元素绑定事件）

  ``` js
  document.getElementById("id值").onxxx=function(){...};
  document.getElementById("id值").onxxx=函数名;
  ```
  注意：内存中应该存在该元素才可以派发事件，怎么理解？因为网页是从前往后解析，如果把派发事件写在页面元素的前面，而 HTML 页面其实还没加载完毕，在这之前获取元素是获取不到，即不能派发事件。

   做法：	
  1. 将 js 代码放在 html 页面的最下面
  2. 在页面加载成功之后在运行 js 代码  onload 事件.
      ​

# 3. BOM(浏览器对象模型)

所有的浏览器都有 5 个对象。

- window：窗口
- location：定位信息 (地址栏)
- history：历史

**Windows 对象详解** 

如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象。

若是 window 对象的属性和方法,调用的时候可以省略 window

- 常用的属性：通过 window 可以获取其他的四个对象

  - window.location 等价于 location
  - window.history 等价于 history
  - ...

- 常用的方法：

  - 消息框：

    `alert("....")` 警告框 

    `confirm("你确定要删除吗?")`确定框，返回值：boolean

    `prompt("请输入您的姓名")` 输入框，返回值：你输入的内容

  - 定时器：

    设置定时器：setInterval(code, 毫秒数):周期执行；setTimeout(code,毫秒数)：延迟    多长事件后 只执行一次，例如：`setInterval(showAd,4000);`，`serInterval("showAd()",4000);`

    清除定时器：`clearInterval(id):`，`clearTimeout(id):`

  - 打开和关闭：`open(url)` 打开；`close()` 关闭

**location 对象：定位信息，地址栏** 

常用属性：

- href 获取或者设置当前页面的 url(定位信息)
- location.href; 获取 url
- location.href="..."; 设置 url，相当于 a 标签，超链接

**history 对象：操作历史** 

- back();  后退
- forward():  向前
- go(int)
  - go(-1) 相当于 back()
  - go(1) 相当于 forward()



# 4. DOM(文档对象模型)

就是我们的 HTML 代码加载到内存中会形成一颗 document 树。

节点：

- 文档节点 document
- 元素节点 element
- 属性节点 attribute
- 文本节点 text

获取一个元素节点：通过 document 获取

操作元素的属性：`dom对象.属性`

操作元素的标签体：`dom对象.innerHTML`



# 5. 其他及注意

(1) 在方法中 `(function())` this 指代的是当前的元素（当前的 dom 对象）
例如：

``` html 
<input type="text" name="username" id="username" onblur="loseFocus(this.value)">
方法:
function loseFocus(obj){
	alert(obj);
}
```

(2) 事件总结：

常见事件：

- 焦点事件：`onfocus`、`onblur`
- 表单事件：`onsubmit`、`onchange` 改变
- 页面加载事件：`onload`
- 鼠标事件（掌握）：`onclick`
- 鼠标事件（了解）：`ondblclick` 双击、`onmousedown` 按下、`onmouserup` 弹起、`onmousemove` 移动、`onmouserover` 悬停、`onmouserout` 移出
- 鼠标事件（理解）：`onkeydown` 按下、`onkeyup` 弹起、`onkeypress` 按住

(3) 阻止默认事件的发生；阻止事件传播

(4) 引用型类似总结：

- 原始类型中的 String Number Boolean 都是伪对象，可以调用相应的方法
- Array：数组
- String：

  ``` html
  语法:
  	new String(值|变量);//返回一个对象
  	String(值|变量);//返回原始类型
  常用方法:
  	substring(start,end):[start,end)
  	substr(start,size):从索引为指定的值开始向后截取几个
  	
  	charAt(index):返回在指定位置的字符。
  	indexOf(""):返回字符串所在索引
  	
  	replace():替换
  	split():切割
  			
  常用属性:length	
  ```

- Boolean：

  ``` html
  语法：
  	new Boolean(值|变量);
  	Boolean(值|变量);
  	非0数字 非空字符串 非空对象 转成true
  ```

- Number：语法

  ``` html
  语法:
  	new Number(值|变量);
  	Number(值|变量);
  注意:

  	null====>0 
  	fale====>0 true====>1
  	字符串的数字=====>对应的数字
  	其他的NaN
  ```

- Date：

  ``` html
  new Date();
  常用方法:
  	toLocalString()
  ```

- RegExp:正则表达式

  ``` html
  语法:
  	直接量语法  /正则表达式/参数
  	直接量语法  /正则表达式/
  			
  	new RegExp("正则表达式")
  	new RegExp("正则表达式","参数") 
  	参数:
  		i:忽略大小写
  		g:全局
  	常用方法:
  		test() :返回值为boolean
  ```

- Math：

  ``` html
  Math.常量|方法
  Math.PI
  Math.random()  [0,1)
  ```
- 全局：

  ``` html
  decodeURI() 解码某个编码的 URI。 
  encodeURI() 把字符串编码为 URI。

  Number():强制转换成数字
  String():转成字符串

  parseInt():尝试转换成整数
  parseFloat():尝试转换成小数

  eval() 计算 JavaScript 字符串，并把它作为脚本代码来执行。 
  ```

