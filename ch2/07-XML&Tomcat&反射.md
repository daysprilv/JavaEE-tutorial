
# 1. XML(可扩展的标记语言)

关于 XML 知识，要求掌握：

- 可以编写简单的 xml 文件
- 可以按照指定的约束文件编写 xml 文件

## (1) xml的介绍

XML 全称为 Extensible Markup Language，意思是可扩展的标记语言。XML 语法上和 HTML 比较相似，但 HTML 中的元素是固定的，而 XML 的标签是可以由用户自定义的。

作用：存储数据。（一般用于配置文件）

书写规范：

1. 区分大小写
2. 应该有一个根标签
3. 标签必须关闭 `<xx> </xx>` 、`<xx/>`
4. 属性必须用引号引起来：`<xx att="value"/>`
5. 标签体中的空格或者换行或者制表符等内容都是作为数据内容存在的： `<xx>aa</xx>`、`<xx>    aa   </xx>` 是不一样的
6. 特殊字符必须转义：`<`、`>`、`&`

满足上面规范的文件我们称之为是一个格式良好的 xml 文件；可以通过浏览器浏览。后缀名为：`.xml`

## (2) xml组成部分

- 声明

  - 作用：告诉别我是一个 xml 文件

  - 格式：`<?xml ..... ?>`

    例如：

    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml version='1.0' encoding='utf-8' standalone="yes|no"?>
    ```

    要求：必须在 xml 文件的第一行；必须顶格写。

- 元素（标签）

  - 格式：`<xx></xx>`、`<xx/>`

    要求：

    1. 必须关闭
    2. 标签名不能 xml、Xml、XML 等等开头
    3. 标签名中不能出现" "或者":"等特殊字符

- 属性

  - 格式：`<xx 属性名="属性值"/>`  要求：属性必须用引号引起来

- 注释：和 html 的注释方式一样 `<!-- 注释内容 -->`

- CDATA：xml 文件的特殊字符必须转义，通过 cdata 可以保证数据原样输出。

  - 格式：

    ``` xml
    <![CDATA[
        原样输出的内容
    ]]>
    ```

## (3) xml解析

解析方式：

1. sax：（特点）逐行解析，只能查询。
2. dom：（特点）一次性将文档加载到内容中，形成一个 dom 树。可以对 dom 树 curd 操作。

解析技术：

- JAXP：sun公司提供支持DOM和SAX开发包
- JDom：dom4j兄弟
- jsoup：一种处理HTML特定解析开发包
- dom4j (★)：比较常用的解析开发包，hibernate底层采用。

### domj4

**dom4j** 技术进行查询操作，使用步骤：

1. 导入 jar 包

2. 创建一个核心对象 SAXReader  `new SAXReader();`

3. 将 xml 文档加载到内存中形成一棵树  `Document doc=reader.read(文件)`

4. 获取根节点：`Element root=doc.getRootElement();`

5. 通过根节点就可以获取其他节点(文本节点，属性节点，元素节点)

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-9-54918257.jpg)

   获取所有的子元素：`List<Element> list=root.elements()`

   获取元素的指定属性内容：`String value=root.attributeValue("属性名");`

   获取子标签标签体：遍历 list 获取到每一个子元素，`String text=ele.elementText("子标签名称")`

``` java
public static void main(String[] args) throws Exception {
    //创建核心对象
    SAXReader reader = new SAXReader();	
    //获取dom树
    Document doc = reader.read("D:\\eclipsewk\\28\\day08\\xml\\web.xml");		
    //获取根节点
    Element root=doc.getRootElement();
    //获取其他节点
    List<Element> list = root.elements();
    //遍历集合
    for (Element ele : list) {
        //获取servlet-name的标签体
        String text = ele.elementText("servlet-name");
        //System.out.println(text);
        //获取url-pattern标签体
        //System.out.println(ele.elementText("url-pattern"));
    }
    //获取root的version属性值
    String value = root.attributeValue("version");
    System.out.println(value);
}
```

### xpatch

**xpath** 解析技术（扩展了解）：依赖于 dom4j

使用步骤：

1. 导入 jar 包(dom4j 和 jaxen-1.1-beta-6.jar)
2. 加载 xml 文件到内存中
3. 使用 api    `selectNode("表达式");`、`selectSingleNode("表达式");`

表达式的写法：

``` xml
/ 从根节点选取 
// 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置 
例如一个标签下有一个id属性且有值  id=2;
	//元素名[@属性名='属性值']
	//元素名[@id='2']
```

## (4) 反射

### Java中反射应用

①获取对应的 class 对象

- 方式1★： `Class clazz=Class.forName("全限定名");`


- 方式2：`Class clazz=类名.class;`


- 方式3：`Class clazz==对象.getClass();`

②通过 class 对象创建一个实例对象（相当于 new 类()）：  `Object clazz.newInstance();`

③通过 class 对象获取一个方法（public修饰的）：`Method method=clazz.getMethod("方法名",Class .... paramType);` ，paramType 为参数的类型

④让方法执行：`method.invoke(Object 实例对象,Object ... 参数);`

- Object 实例对象：以前调用方法的对象 

- Object ... 参数：该方法运行时需要的参数 

例如：`method.invoke(a,10,30)`

``` java
@Test
//调用有两个参数的add方法
public void f4()throws Exception{
    //获取class对象
    //Class clazz = Class.forName("com.itheima.HelloMyServlet");
    Class clazz=HelloMyServlet.class;
    //通过clazz对象创建一个实例
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    //获取有两个参数的add方法
    Method m = clazz.getMethod("add",int.class,int.class);	
    //执行方法
    m.invoke(a, 10,30);//相当于 a.add(10,30);		
}
```

### 反射例子

HelloMyServlet 类：

``` java
public class HelloMyServlet {
	public void add(){
		System.out.println("空参add方法");
	}
	
	public void add(int i,int j){
		System.out.println("带有连个add方法");
		System.out.println(i+j);
	}
	
	public int add(int i){
		return i+10;
	}
}
```

正常使用：

``` java
@Test
public void f1(){
    //调用helloMyServlet中的方法
    HelloMyServlet a = new HelloMyServlet();
    a.add();
    a.add(10, 20);
}
```

现在可以使用反射：

``` java
@Test
public void f2() throws Exception{
    Class clazz = Class.forName("com.itheima.HelloMyServlet");
    //通过字节码对象创建一个实例对象(相当于调用空参的构造器)
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    a.add();
}

@Test
//调用空参的add方法
public void f3() throws Exception{
    Class clazz = Class.forName("com.itheima.HelloMyServlet");
    //通过字节码对象创建一个实例对象(相当于调用空参的构造器)
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    //获取方法对象
    Method method = clazz.getMethod("add");
    //让方法对象执行   obj调用这个方法的实例,相当于a  ,args是该方法执行时候所需要的参数   a.add()
    method.invoke(a);//相当于 a.add()
}

@Test
//调用有两个参数的add方法
public void f4()throws Exception{
    //获取class对象
    //Class clazz = Class.forName("com.itheima.HelloMyServlet");
    Class clazz=HelloMyServlet.class;
    //通过clazz对象创建一个实例
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    //获取有两个参数的add方法
    Method m = clazz.getMethod("add",int.class,int.class);
    //执行方法
    m.invoke(a, 10,30);//相当于 a.add(10,30);
}
```

## (5) xml 约束

xml 约束作用：规定 xml 中可以出现那些元素及那些属性，以及他们出现的顺序。

- DTD 约束：struts hiebernate等等
- SCHEMA 约束：tomcat spring等等

###DTD 约束

和 xml 的关联(一般都会提供好，复制过来即可，有时候连复制都不需要。)

- 方式1：内部关联。格式：`<!DOCTYPE 根元素名 [dtd语法]>`

- 方式2：外部关联-系统关联

  格式：`<!DOCTYPE 根元素名 SYSTEM "约束文件的位置">`

  例如：`<!DOCTYPE web-app SYSTEM "web-app_2_3.dtd">`

- 方式3：外部关联-公共关联

  格式：`<!DOCTYPE 根元素名 PUBLIC "约束文件的名称" "约束文件的位置">`

dtd 语法（了解）

元素：

``` xml
<!Element 元素名称 数据类型|包含内容>
    数据类型:
    	#PCDATA:普通文本 使用的时候一般用()引起来
    包含内容:
    	该元素下可以出现那些元素 用()引起来
符号:
    *	出现任意次
    ?	出现1次或者0次
    +	出现至少1次
    |	或者
    ()  分组
    ,	顺序
```

属性：

``` xml
格式:
	<!ATTLIST 元素名 属性名 属性类型 属性是否必须出现>
属性类型:
    ID:唯一
    CDATA:普通文本
属性是否必须出现
    REQUIRED:必须出现
    IMPLIED:可以不出现
```

**注意：一个 xml 文档中只能添加一个 DTD 约束。**

dtd 约束例子：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
	模拟servlet2.3规范，如果开发人员需要在xml使用当前DTD约束，必须包括DOCTYPE。
	格式如下：
	<!DOCTYPE web-app SYSTEM "web-app_2_3.dtd">
-->
<!ELEMENT web-app (servlet*,servlet-mapping* , welcome-file-list?) >
<!ELEMENT servlet (servlet-name,description?,(servlet-class|jsp-file))>
<!ELEMENT servlet-mapping (servlet-name,url-pattern) >
<!ELEMENT servlet-name (#PCDATA)>
<!ELEMENT servlet-class (#PCDATA)>
<!ELEMENT url-pattern (#PCDATA)>

<!ELEMENT welcome-file-list (welcome-file+)>
<!ELEMENT welcome-file (#PCDATA)>

<!ATTLIST web-app version CDATA #REQUIRED>
```

### SCHEMA约束

一个 xml 文档中可以添加多个 schema 约束。

xml 和 schema 的关联，格式：

``` xml
<根标签 xmlns="..." ...>
<根标签 xmlns:别名="..." ...>
```

名称空间：关联约束文件；规定元素是来源于那个约束文件的。

例如：

``` html
一个约束文件中规定 table(表格)  表格有属性 row和col
还有一个约束文件规定 table(桌子) 桌子有属性 width和height

在同一个xml中万一我把两个约束文件都导入了,
	在xml中我写一个table,这个table有什么属性????
我们为了避免这种情况的发生,可以给其中的一个约束起个别名
使用的时候若是没有加别名那就代表是来自于没有别名的约束文件
    例如 table(表格) 给他起个别名  xmlns:a="..."
    在案例中使用 a:table 代表的是表格
    若在案例中直接使用 table 代表的是桌子
在一个xml文件中只能有一个不起别名;
```

**注：schema 约束本身也是 xml 文件。**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-9-62475749.jpg)



# 2. tomcat(应用服务器)

## (1) tomcat 介绍

常见的 web 服务器：

| 服务器名称 | 厂商 | 特点 |
| :------------ |:--------------| :-----|
| weblogic | oracle | 大型的收费的支持javaee所有规范的服务器 |
| webspere | IBM | 大型的收费的支持javaee所有规范的服务器 |
| tomcat | apache组织 | 中小型的免费的支持servlet和jsp规范的服务器 |

> 关于 JavaEE 规范了解下：[【Java】javaEE的13个技术规范](https://blog.csdn.net/zh15732621679/article/details/53966404)

tomcat 下载：

- `tar` 、`.tar.gz：` 提供给 Linux 系统
- `.zip`、 `.exe`：供给 Windows 系统

安装：解压 `apache-tomcat-7.0.52.zip` 即可

启动：tomcat/bin 目录下，双击 `startup.bat`，打开浏览器访问 `http://localhost:8080`

退出：①点 x；②ctrl+c；③双击 shutdown.bat

常见问题：

1. 启动的时候一闪而过 --> 正确配置：JAVA_HOME

2. 端口冲突问题 -->

   修改 tomcat 的端口号，打开 `tomcat/conf/server.xml`

   大概 70 行左右，有如下代码：

   ``` xml
   <Connector port="8080" protocol="HTTP/1.1"
       connectionTimeout="20000"
       redirectPort="8443" />
   ```

   修改 port 后面的值就可以了。

   **注意：1024 以下的端口号留给系统用的。**80 端口是默认给 http 协议用的，我们可以改为使用这个端口号，这样就可以不用输入端口号了。

3. 有可能出现的问题（在环境变量中配置CATALINA_HOME）--> 删除

## (2) tomcat目录和web项目目录

tomcat 目录：

``` html
bin:存放的可执行程序
conf:配置文件
lib:存放的是tomcat和项目运行时需要的jar包
logs:日志 注意:catalina
temp:临时文件
★★webapps:存放项目的目录
★work:存放jsp文件在运行时产生的java和class文件
```

web 项目的目录结构：

``` html
|
|---- html css js image等目录或者文件
|
|---- WEB-INF(特点:通过浏览器直接访问不到 目录)
|	 	|
|	 	|--- lib(项目的第三方jar包)
|	 	|--- classes(存放的是我们自定义的java文件生成的.class文件)
|	 	|--- web.xml(当前项目的核心配置文件)
|	 	|
```

以上是 web2.5版本标准的目录结构，若版本为 3.0 目录下则没有 web.xml。

访问路径：`http://主机:端口号/项目名称/资源路径`，以项目 myweb 为例：`http://localhost:80/myweb/1.html`

##(3) 常用的项目发布方式(虚拟目录映射)

①方式1：将项目放到 `tomcat/webapps` 下

②方式2(了解)：修改`tomcat/conf/server.xml`，大概130行，在 host 标签下添加如下代码`<Context path="/项目名" docBase="项目的磁盘目录"/>`  例如：`<Context path="/my" docBase="G:\myweb"/>`

③方式3(了解)：在 `tomcat/conf/` 引擎目录/主机目录下新建一个 xml 文件，文件的名称就是项目名，文件的内容如下：`<Context docBase="G:\myweb"/>`