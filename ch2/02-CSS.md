
# 1. CSS介绍

层叠样式表，作用：渲染页面，提高工作效率。

格式：`选择器{属性:值;属性1:值1;}`

后缀名：`.css`  独立的 css(样式)文件

和 HTML 元素的整合 ：

- 方式1：内联样式表，通过标签的 style 属性设置样式，例如：`<div style="background-color: #FF0000;"></div>`

- 方式2：内部样式表，在当前页面中使用的样式，例如

  ``` css
  <style>
      #divId2{
          background-color: #0f0;
      }
  </style>
  ```

- 方式3：外部样式表，有独立的 css 文件，通过 head 标签的 link 子标签导入，例如：

  ```css
  <link rel="stylesheet" href="css/1.css" type="text/css"/>
  ```

  


# 2. 选择器

- **id 选择器**

  要求： html 元素必须有 id 属性且有值   `<xxx id="id1"></xxx>` ；css 中通过"#"引入,后面加上id的值  `#id1{...}`

- **类选择器：** 

  要求：html 元素必须有 class 属性且有值 `<xxx class="cls1"/>`；css 中通过"."引入,后面加上class的值  `.cls1{...}`

- **元素选择器：**

  直接用元素(标签)名即可   `xx{...}`  例如：`div{ }`

- **属性选择器：**

  html 元素有一个属性且有值 `<xx att="val1">`，css 中通过元素名[属性="值"]使用   `xx[att="val1"]{...}` 例如：`xxx[nihao="wohenhao"]{....}`

- **后代选择器：**

  `选择器 后代选择器{...}`：在满足第一个选择器的条件下找后代的选择器，给满足条件的元素添加样式。

- **锚伪类选择器：**（了解）

   ``` xml
   a:link {color: #FF0000}	/* 未访问的链接 /
   a:visited {color: #00FF00}	/ 已访问的链接 /
   a:hover {color: #FF00FF}	/ 鼠标移动到链接上 /
   a:active {color: #0000FF}	/ 选定的链接 */
   ```

选择器使用小结：

- id 选择器：一个元素(标签)
- 类选择器：一类元素 
- 元素选择器：一种元素
- 属性选择器：元素选择器的特殊用法

使用的时候注意：

- 若多个样式作用于一个元素的时候：
  - 不同的样式,会叠加
  - 相同的样式,最近原则
- 若多个选择器作用于一个元素的时候：
  - 越特殊优先级越高，id 优先级最高



# 3. 样式

字体：

- font-family：设置字体(隶书) 设置字体家族
- font-size:：设置字体大小
- font-style：设置字体风格

文本：改变文本的颜色、字符间距，对齐文本，装饰文本，对文本进行缩进

- color：文本颜色
- line-height：设置行高
- text-decoration：向文本添加修饰。 none underline
- text-align：对齐文本

列表：

- list-style-type：设置列表项的类型，例如：a 1  实心圆 
- list-style-image：设置图片最为列表项类型 使用的时候使用 url函数  `url("/i/arrow.gif");`

背景：

- background-color：设置背景颜色
- background-image：设置图片作为背景 url

尺寸：

- width：宽度
- height：高度

浮动：

- float：可选值 left、right

分类：

- clear：设置元素的两边是否有其他的浮动元素，值为 `both` 两边都不允许有浮动元素
- display：设置是否及如何显示元素。
  - none 此元素不会被显示。 
  - block 此元素将显示为块级元素，此元素前后会带有换行符。 
  - inline 默认，此元素会被显示为内联元素，元素前后没有换行符。



# 4. 框模型（理解）

一个元素外面有 padding(内边距)、border(边框)、margin(外边距)

- padding：元素和边框的距离
- margin：元素最外层的空白

在网上找了两幅图片：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219232641.png)

![img](https://img-blog.csdn.net/20180430120005726?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTIxOTUyMTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上面这三个属性都有简写的属性，若设置大小的时候，四个值顺序为：**上右下左**

如：`padding:10px 10px 10px 10px`

- 若只写一个的话：代表四个边使用同一个值  `padding:10px`
- 若只写两个个的话：代表四个边使用同一个值 `padding:10px 20px`
- 若只写三个个的话：代表四个边使用同一个值 `padding:10px 20px 30p`

border(边框)：可以设置颜色 风格

简写属性：`border:宽度 风格 颜色;` ，例如：`border:5px solid red;`

说明：solid：实线，dashed：虚线，double：双实线





