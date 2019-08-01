计算机（右键）-属性-高级系统设置-环境变量

1. 新建系统变量 ： JAVA_HOME

C:\Program Files (x86)\Java\jdk1.6.0_10(你的JDK安装路径)

2. 修改系统变量 ：PATH

%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin 或者直接%JAVA_HOME%\bin

3. 新建系统变量：CLASSPATH

.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\dt.jar (要加.表示当前路径) 

4. 检验是否配置成功：

   cmd---
   java -version 、
   javac、
   java  

**java环境变量作用，总结网上的通俗理解版：**

path：是一个环境变量，就像一个程序中你声明的一个变量有用户级和系统级，可以理解为全局和局部变量。变量当然是拿来用的，这样系统就可以根据path=value的value来找到相应的可执行文件，就像你在运行里输入cmd、msconfig、notepad等一样，现在你可以输入path里包含的命令如：javac。
classpath：是告诉一些工具软件如Tomcat之类的，你的JDK在哪，或是你新包含的什么jar包。简单来说它就是类的路径，在编译运行java程序时，如果有调用到其他类的时候，在classpath中寻找需要的类。
总的来说“path”的作用就是用来添加dos中使用的"javac"、“ -version”等命令。
而“classpath”则是为了能在dos中调用jdk中的类。 

---



Windows 下 java 用到的环境变量主要有3个，JAVA_HOME、CLASSPATH、PATH。 

- JAVA_HOME指向的是 jdk 的安装路径，如C:\JDK_1.4.2，在这路径下你应该能够找到bin、lib等目录。值得一提的是，JDK的安装路径可以选择任意磁盘目录，不过建议你放的目录层次浅一点，如果你放的目录很深，比如`C:\XXXXXX\xxxxx\XXXX\xxxx\XXXX\xxxx\XXXX\xxx…… `

  设置方法： 
  `JAVA_HOME=C:\JDK_1.4.2` 

- PATH环境变量原来Windows里面就有，你只需修改一下，使他指向JDK的bin目录，这样你在控制台下面编译、执行程序时就不需要再键入一大串路径了。设置方法是保留原来的PATH的内容，并在其中加上%JAVA_HOME%\bin (注，如果你对DOS批处理不了解，你可能不明白%%引起来的内容是什么意思；其实这里是引用上一步设定好的环境变量JAVA_HOME，你写成C:\JDK_1.4.2也是可以的；你可以打开一个控制台窗口，输入echo %JAVA_HOME%来看一下你的设置结果) ： 

  `PATH=%JAVA_HOME%\bin;%PATH%` 
  同样，%PATH%是引用以前你设置的PATH环境变量，你照抄以前的值就行了。 

- CLASSPATH环境变量我放在最后面，是因为以后你出现的莫名其妙的怪问题80%以上都可能是由于CLASSPATH设置不对引起的，所以要加倍小心才行。 

  `CLASSPATH=.;%JAVA_HOME%\lib\tools.jar`
  首先要注意的是最前面的"`.;`"，如果你看不清，我给你念念——句点分号。这个是告诉JDK，搜索CLASS时先查找当前目录的CLASS文件——为什么这样搞，这是由于LINUX的安全机制引起的，LINUX用户很明白，WINDOWS用户就很难理解(因为WINDOWS默认的搜索顺序是先搜索当前目录的，再搜索系统目录的，再搜索PATH环境变量设定的) ，所以如果喜欢盘根究底的朋友不妨研究一下LINUX。 

  为什么CLASSPATH后面指定了tools.jar这个具体文件？不指定行不行？显然不行，行的话我还能这么罗索嘛！:) 这个是由java语言的import机制和jar机制决定的，你可以查资料解决。 

  具体的设定方法: win2k\xp用户右键点击我的电脑->属性->高级->环境变量，修改下面系统变量那个框里的值就行了。 
  win9x用户修改autoexec.bat文件，在其末尾加入: 
  `set JAVA_HOME=C:\JDK_1.4.2`
  `set PATH=%JAVA_HOME%\bin;%PATH%` 
  `set CLASSPATH=.;%JAVA_HOME%\lib\tools.jar` 
  就可以了。