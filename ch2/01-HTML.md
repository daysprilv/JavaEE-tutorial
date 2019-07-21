
# 1. HTML介绍

（1）超文本标记语言

- 作用：展示。

- 超文本：超越了一般的文本，描述文本的字体、颜色、图片。

- 标签：标记。

- 书写规则：
  - 文件的后缀名：`.html`（建议）或者`.htm`；
  - 标签必须用`<>`引起来，已经已经定义好的标签
  - 属性：
    - 格式：key=“value”，建议属性值用引号引起来
    - 不区分大小写

  注意：

  - 最好将所有的内容放在一个标签中 `<html></html>`
  - 有开始标签和结束标签的标签称之为围堵标签
  - 没有结束的标签的称之为空标签，如`<br/>`
  - 开始标签和结束标签之间的内容称之为标签体.
  - `<!--注释内容-->` HTML 页面中的注释
  - 标签必须正常嵌套
  - 标签最好关闭



# 2. HTML标签

## (1) 文件标签

`html` 标签：声明当前网页为 html 页面。
子标签：

``` html
<head></head>
<body></body>
```
- `head`：用来存放当前页面的重要信息,一般不展示在html页面上，其常见的子标签：`<title></title>`——>为网页的标题
- `body`：要展示的数据放在 body 中。

## (2) 标题标签

`<hn><hn>`：n 取值：1~6，h1 最大，h6 最小，自动换行，且留白，默认加粗。

常用属性：

- `align`：对齐方式。left 左，right  右，center 居中，eg： `<h2 align="center"></h2>`

## (3) 字体标签

- `h1~h6`：标题
- `font`：字体，常用属性： face 字体，size 尺寸，color 颜色
- `b`、`strong `：文字加粗，eg：`<b>哈哈</b>`
- `i`：文字斜体，如：`<i>斜了</i>`

## (4) Div标签

`div` 标签：定义 HTML 文档中的一个分隔区块或者一个区域部分。可以把文档分割为独立的、不同的部分。它可以用作严格的组织工具，并且不使用任何格式与其关联。`<div>` 是一个块级元素。这意味着它的内容自动地开始一个新行。实际上，换行是 `<div>` 固有的唯一格式表现。

``` html
<div style="background-color:lightblue">
  <h3>This is a heading</h3>
  <p>This is a paragraph.</p>
</div>
```


## (5) 其他标签

**水平线：** `<hr/>`

**换行：** `<br/>`

**颜色的取值：** （RGB）

- 方式1：#xxxxxx  x为16进制，如：`#acfdff`
- 方式2：英文单词，如：`red`

**图片：** 

`<img src="图片的路径" alt="替代文字" width="" height=""/>`

路径的写法：

- 相对路径：./ 或者 什么都不写  当前目录；../ 上一级目录
- 绝对路径(常用)：`http://www.baidu.com/xx`

**超链接：**

`<a href="跳转的路径" target="在什么地方打开"></a>`

**表格标签：**

``` html
<table border="" width="" height="" bgcolor="" align="表格对齐方式">
    <tr align="内容对齐方式">
        <th></th>  	<!--表头单元格-->
        <td></td> 	<!--普通单元格-->
    </tr>
</table>
```
`td` 重要的属性： colspan 列合并，rowspan 行合并

**列表：**

- `<ol></ol>`

- `<ul></ul>`

  子标签：列表项  `<li></li>`

**form 表单标签：**

常用属性：action 提交的路径，method 提交的方式（get 和 post）

常用子标签：`input`、`select`、`textarea`

`input` 标签：

常用属性有：

- type（分 3 组）： （text 文本框、password 密码、radio 单选框、checkbox 多选框、file 文件框）；（submit 提交、reset 重置、button 普通按钮）；（hidden 隐藏域、image 图片提交）
- name：要想将信息提交到服务器必须提供name属性；将单选框和复选框设置成一组
- value：text、password  设置默认值；radio、checkbox 设置选中后提交的值；submit、reset、button 给按钮起个显示的名字。

`select`：下拉选

``` html
<select name="">
    <option value="">显示的名字</option>
</select>
```

 `textarea`：文本域
``` html
<textarea cols="" rows="" name="">默认值</textarea>
```

注意：

- 给单选框多选框设置默认值，设置属性： `checked="checked"`

- 给下拉选设置默认值，设置属性： `selected="selected"`

- 提交的路径（get）：`http://ssdfsdfsfd?key=vaule&key=value`  如：`http://localhost/day/login.jsp?username=tom&password=123`

**框架（了解）**

`frameset`：规定框架结构，框架集。

常用属性：rows 水平切割，cols 垂直切割

子标签：`frame`：具体实现

如：水平切割 3 份

``` html
<frameset rows="15%,*,10%">
    <frame>
    <frame>
    <frame>
</frameset>
```

- `frame`：常用属性：src 具体展示的网页；name 给当前的 frame 取个名字，方便超链接使用。

**span 标签：**

`span` 标签：`<span>` 标签被用来组合文档中的行内元素。

**meta：** 

`meta`：元信息， `<meta charset="UTF-8">` 指定浏览器用什么编码打开此页面

## (6) 转义字符

转义字符：三部分构成 `&实体;`

| 显示 |  描述  | 实体名称 |
| ---- | :----: | -------: |
| >    | 大于号 |   `&gt;` |
| <    | 小于号 |   `&lt;` |
| &    |  和号  |  `&amp;` |
|      |  空格  | `&nbsp;` |

*更多：[HTML转义字符](http://tool.oschina.net/commons?type=2)*