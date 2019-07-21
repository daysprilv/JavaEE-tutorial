
# 1. JQuery 基础

JQuery：JavaScript 的一个类库，对常用的方法和对象进行封装，方便我们使用。

JQuery 是单独的 js 文件，通过 script 标签的 src 属性导入即可。

获取一个 jquery 对象，格式：`$("选择器")` 或者 `jQuery("选择器")`

**(1) 对象转换**

dom 对象和 jquery 对象之间的转换：

- dom 对象 –> jquery 对象：`$(dom对象)`
- jquery 对象 –> dom对象：① `jquery对象[index]`、② `jquery对象.get(index)`

**(2) 页面加载** 

JS 中：`window.onload=function(){}`  在一个页面中只能使用一次

JQuery 在一个页面中可以使用多次：

- 方式① `$(function(){...})`、
- 方式② `$(document).ready(function(){});`

`$(document).ready()` 和 `window.onload` 等几个的区别？

①JS 中，常见：` window.onload` 如：

``` js
window.onload = function(){ 
  alert(“text1”); 
}; 
```
等同于 JQuery 中
```
$(window).load(function(){
    alert("text1");
});
```
他们都是用于当页面的所有元素，包括外部引用文件，图片等都加载完毕时运行函数内的 alert 函数。load 方法只能执行一次，如果在 js 文件里写了多个，只能执行最后一个。

②另外，由于在 `$(document).ready()` 方法内注册的事件，只要 DOM 就绪就会被执行，因此可能此时元素的关联文件未下载完，例如与图片有关的HTML下载完毕，并且已经解析为 DOM 树了，但很有可能图片还未加载完毕，所以例如图片的高度和宽度这样的属性此时不一定有效。

要解决这个问题，可以使用 JQuery 中另一个关于页面加载的方法 load() 方法。load() 方法会在元素的 onload 事件中绑定一个处理函数。如果处理函数绑定在元素上，则会在元素的内容加载完毕后触发。如：$(window).load(function(){ }) 等价于 window.onload = function(){ }…

③在 JQuery 中，可以写 `$(document).ready(function(){ })` 等同于（简化写法为）：`$(function(){ })`  他们都是用于当页面的标准 DOM 元素被解析成 DOM 树后就执行内代码。这个函数是可以在 js 文件里多次编写的，对于多人共同编写的js 就有很大的优势，因为所有行为函数都会执行到。而 `$(document).ready()` 函数在 HMTL 结构加载完后就可以执行，不需要等大型文件加载或者不存在的连接等耗时工作完成才执行，效率高。

*详细参考：[$(document).ready() 和 window.onload 的区别](https://blog.csdn.net/u012195214/article/details/73691424)* 

**(3) 派发事件**

- `$("选择器").click(function(){...});` 相当于原生 JS 中 `dom对象.onclick=function(){....}`

- 掌握：focus、blur、submit、change、click

jquery 中效果：

- 隐藏或展示：show(毫秒数)、hide(毫秒数)
- 滑入或滑出：slideDown(毫秒数)：向下滑入、slideUp(毫秒数):向上滑出
- 淡入或淡出：fadeIn(int)：淡入、fadeOut(int)：淡出



# 2. 选择器

基本选择器：

- `$("#id值")`  
- `$(".class值")`  
- `$("标签名")  `

了解：`$("*")`

理解：获取多个选择器 用逗号隔开 `$("#id值,.class值")`

层次选择器：

- `a b`：a 的所有 b 后代
- `a>b`：a 的所有 b 孩子
- `a+b`：a 的下一个兄弟(大弟弟)
- `a~b`：a 的所有弟弟

基本过滤选择器：

- `:frist` 第一个
- `:last` 最后一个
- `:odd`  索引奇数
- `:even` 索引偶数
- `:eq(index)` 指定索引
- `:gt(index)` >
- `:lt(index)` <

内容过滤：`:has("选择器")` 包含指定选择器的元素

可见过滤：

- `:hidden`   在页面不展示元素 一般指 `input type="hidden"` 和 样式中 `display:none`
- `:visible` 

属性过滤器：

- [属性名]
- [属性名="值"]

表单过滤：`:input`  所有的表单子标签  input select textarea button

表单对象属性过滤选择器：

- `:enabled`：可用的
- `:disabled`：不可用
- `:checked`：选中的(针对于单选框和复选框的)
-  `:selected`：选中的(针对于下拉选)



# 3. 属性与CSS的操作

**(1) 对属性的操作**

attr()：设置或获取元素的属性（jquery1.6版本之前）

- 方式1：获取 `attr("属性名称")`

- 方式2：设置一个属性 `attr("属性名称","值");`

- 方式3：设置多个属性

   ``` js
   attr({
       "属性1":"值1",
       "属性2":"值2"
   })
   ```

*注：`prop()`和 attr 用法一致（jquery1.6 版本之后）*

移除属性：`removeAttr("属性名称")`

`addClass("指定的样式值"); `  相当于 `attr("class","指定的样式值");`

`removeClass("指定的样式值");` 移除指定的样式

**(2) 对 CSS 操作**

操作元素的 style 属性。

`css()`： 获取或设置 css 样式

- 方式1：获取 `css("属性名")`

- 方式2：设置一个属性 `css("属性名","值")`

- 方式3：设置多个

   ``` css
   css({
       "属性1":"值1",
       "属性2":"值2"
   });
   ```

获取元素的尺寸：width()、height()

**(3) 文档处理**

内部插入：

- a.append(c)：将c插入到a的内部(标签体)后面
- a.prepend(c)：将c插入到a的内部的前面
- appendTo
- prependTo

外部插入：

- a.after(c)：将 c 放到 a 的后面
- a.before(c)：将 c 放到 a 的前面
- a.insertAfter(c)
- a.insertBefore(c)

删除：

- empty()：清空元素
- remove()：删除元素

其他：val  html  text

- val：设置或者获取 values 属性

- html：获取或者设置标签体内容

- text：获取或设置标签体内容，但只是标签文本内容( 不带样式，**有区别于 html** )

  例如：

  `<div id="d1"><a href="http://www.baidu.com/">百度</a></div>`

  `alert($("#d1").text());` 弹出的内容为“百度”

  `alert*($("#d1").text());` 弹出的内容为 `<a href="http://www.baidu.com/">百度</a>`

遍历数组：

- 方式1：`jquery对象.each(function(){});`
- 方式2：`$.each(jquery对象,function(){});`

在 jquery 中创建元素：`$("<标签名>").prop(属性).html(内容)`
