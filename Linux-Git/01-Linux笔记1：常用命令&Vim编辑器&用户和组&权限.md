## 一、Linux的概述

**1、学习 Linux 之前先了解 Unix** 

Unix 是一个强大的**多用户、多任务操作系统**。于 1969 年在 AT&T 的贝尔实验室开发。Unix 的商标权由国际开放标准组织（The Open Group）所拥有。Unix 操作系统是商业版，需要收费，价格比 Microsoft Windows 正版要贵一些。关于多用户、多任务操作系统，参考网上相关资料：[操作系统概述](http://zy.swust.net.cn/00/1/jsjwhjc/content/chapter2/j1-1.htm)

**2、Linux 的概述**

Linux 是基于 Unix 的。Linux 是一种自由和开放源码的操作系统，存在着许多不同的 Linux 版本，但它们都使用了 Linux 内核，我们称其为 Linux 发行版。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-50201545.jpg)

Linux 可安装在各种计算机硬件设备中，比如手机、平板电脑、路由器、台式计算机。

Linux 诞生于 1991 年 10 月 5 日。是由芬兰赫尔辛基大学学生 Linus Torvalds 和后来加入的众多爱好者共同开发完成。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-16830482.jpg)![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-18019021.jpg)

下图为 Linus Torvalds 在 GitHub 上的账户，地址：https://github.com/torvalds。  Linux 代码也一直开源在 GitHub 上，下图第一个 repository 就是，地址：https://github.com/torvalds/linux

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-48951896.jpg)

3、Linux 的历史

Linux 最初是由芬兰赫尔辛基大学学生 Linus Torvalds 由于自己不满意教学中使用的 Minix 操作系统， 所以在 1990 年底由于个人爱好设计出了 Linux 系统核心。后来发布于芬兰最大的 FTP 服务器上，用户可以免费下载，所以它的周边的程序越来越多，Linux 本身也逐渐发展壮大起来，之后 Linux 在不到三年的时间里成为了一个功能完善，稳定可靠的操作系统。

4、Linux 系统的应用

- 服务器系统

  Web 应用服务器、数据库服务器、接口服务器、DNS、FTP 等等； 

- 嵌入式系统

  路由器、防火墙、手机、PDA、IP 分享器、交换器、家电用品的微电脑控制器等等，

- 高性能运算、计算密集型应用

  Linux 有强大的运算能力。

- 桌面应用系统

- 移动手持系统

5、Linux 的版本

Linux 的版本分为两种：内核版本和发行版本。

- 内核版本是指在 Linus 领导下的内核小组开发维护的系统内核的版本号 ；
- 发行版本是一些组织和公司根据自己发行版的不同而自定的。

6、Linux 的主流版本

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-66657058.jpg)

*以下摘自《鸟哥的Linux私房菜-基本篇》第四版：*

> 由于发展 Linux distributions 的社群与公司实在太多了，例如在台湾有名的 Red Hat, SuSE，Ubuntu，Fedora，Debian 等等，所以很多人都很担心，如此一来每个 distribution 是否都不相同呢？ 这就不需要
> 担心了，因为每个 Linux distributions 使用的 kernel 都是 http://www.kernel.org 所释出的，而他们所选择的软件，几乎都是目前很知名的软件，重复性相当的高， 例如网页服务器的 Apache，电子邮件服务器的 Postfix/sendmail，文件服务器的 Samba 等等。
>
> 此外，为了让所有的 Linux distributions 开发不致于差异太大，且让这些开发商在开发的时候有所依据，还有 Linux Standard Base (LSB) 等标准来规范开发者，以及目录架构的 File system Hierarchy Standard (FHS) 标准规范！ 唯一差别的，可能就是该开发者自家所开发出来的管理工具，以及套件管理的模式吧！ 所以说，基本上，每个 Linux distributions 除了架构的严谨度与选择的套件内容外， 其实差异并不太大啦！大家可以选择自己喜好的 distribution 来安装即可！
>
> 事实上鸟哥认为 distributions 主要分为两大系统，一种是使用 **RPM 方式安装软件的系统**，包括 Red Hat，Fedora，SuSE 等都是这类； 一种则是使用 Debian 的 **dpkg 方式安装软件的系统**，包括 Debian，Ubuntu，B2D 等等。若是加上商业公司或社群单位的分类，那么我们可以简单的用下表来做个解释喔！
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-44945686.jpg)
>
> Tips：到底是要买商业版还是社群版的 Linux distribution 呢？如果是要装在个人计算机上面做为桌面计算机用的，建议使用社群版， 包括 Fedora，Ubuntu，OpenSuSE 等等。如果是用在服务器上面的，建议使用商业版本，包括 Red Hat，SuSE 等。 这是因为社群版通常开发者会加入最新的软件，这些软件可能会有一些 bug 存在。至于商业版则是经过一段时间的磨合后， 才将稳定的软件放进去。举例来说，Fedora 兜出来的软件套件经过一段时间的维护后，等到该软件稳定到不容易发生错误后， Red Hat 才将该软件放到他们最新的释出版本中。所以，Fedora 的软件比较经常改版，Red Hat 的软件就较少更版。



## 二、Linux的安装

### 2.1 安装

**方式一：虚拟机安装** 

1. 什么是虚拟机？

   虚拟机：一台虚拟的电脑。

   虚拟机软件：（通过虚拟机软件可以在自己的电脑上安装很多个系统）

   - VmWare：收费的
   - VirtualBox：Oracle 公司产品，免费的

2. 我们使用 VmWare 虚拟机安装 Linux，相关参考资料：

   - [安装VMware Workstation](https://jingyan.baidu.com/article/17bd8e52204b8e85ab2bb8c2.html)
   - [Linux环境搭建-在虚拟机中安装Centos7.0](https://www.cnblogs.com/lynn-li/p/6077944.html)

**方式二：实体硬盘** 

直接对硬盘分出一个新区用于安装 Linux 系统。网上博客很多，参考：

- [Windows 7下硬盘安装Ubuntu 14.04图文教程](https://www.linuxidc.com/Linux/2014-04/100369.htm)
- ......

### 2.2 Linux目录结构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-82226277.jpg)

``` xml
home:家,用户的家
	普通用户的家目录文件在home下 例如:一个用户tom 在home就会存在tom的目录
root:超级管理员root的家
etc:存放配置文件
usr:存放共享的资源
```

| 目录        | 说明                                                         |
| :---------- | :----------------------------------------------------------- |
| /bin        | 存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里。 |
| /etc        | 存放系统管理和配置文件                                       |
| /home       | 存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示 |
| /usr        | 用于存放系统应用程序，比较重要的目录/usr/local 本地系统管理员软件安装目录（安装系统级的应用）。这是最庞大的目录，要用到的应用程序和文件几乎都在这个目录。 |
| /opt        | 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里。 |
| /proc       | 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息。 |
| /root       | 超级用户（系统管理员）的主目录（特权阶级）                   |
| /sbin       | 存放二进制可执行文件，只有root才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如ifconfig等。 |
| /dev        | 用户存放设备文件。                                           |
| /mnt        | 系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统。 |
| /boot       | 存放用于系统引导时使用的各种文件                             |
| /lib        | 存放跟文件系统中的程序运行所需要的共享库及内核模块。共享库又叫动态链接共享库，作用类似windows里的.dll文件，存放了根文件系统程序运行所需的共享文件。 |
| /tmp        | 用于存放各种临时文件，是公用的临时文件存储点。               |
| /var        | 用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等。）等。 |
| /lost+found | 这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里 |

Linux 目录和 Windows 目录有着很大的不同，Linux 目录类似一个树，最顶层是其根目录，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-1597126.jpg)

``` xml
/bin 二进制可执行命令
/dev 设备特殊文件
/etc 系统管理和配置文件
/etc/rc.d 启动的配置文件和脚本
/home 用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示
/lib 标准程序设计库，又叫动态链接共享库，作用类似windows里的.dll文件
/sbin 超级管理命令，这里存放的是系统管理员使用的管理程序
/tmp 公共的临时文件存储点
/root 系统管理员的主目录
/mnt 系统提供这个目录是让用户临时挂载其他的文件系统
/lost+found这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里
/proc 虚拟的目录，是系统内存的映射。可直接访问这个目录来获取系统信息。
/var 某些大文件的溢出区，比方说各种服务的日志文件
/usr 最庞大的目录，要用到的应用程序和文件几乎都在这个目录，其中包含：
/usr/x11R6 存放x window的目录
/usr/bin 众多的应用程序
/usr/sbin 超级用户的一些管理程序
/usr/doc linux文档
/usr/include linux下开发和编译应用程序所需要的头文件
/usr/lib 常用的动态链接库和软件包的配置文件
/usr/man 帮助文档
/usr/src 源代码，linux内核的源代码就放在/usr/src/linux里
/usr/local/bin 本地增加的命令
/usr/local/lib 本地增加的库根文件系统
```



## 三、Linux的常用命令

### 3.1 切换目录

- `cd app`：切换到 app 目录
  - `cd 目录/目录`
- `cd ..`：切换到上一层目录
- `cd /`：切换到系统根目录
- `cd ~`：切换到用户主目录
- `cd -`：切换到上一个所在目录

### 3.2 创建目录和移除目录

mkdir（make directory）命令可用来创建子目录：

- `mkdir app`：在当前目录下创建 app 目录
- `mkdir –p app2/test`：级联创建 app2 以及 test 目录

rmdir（remove directory）命令可用来删除空的子目录：

- `rmdir app`：删除 app 目录

### 3.3 列出文件列表

ls（list）是一个非常有用的命令，用来显示当前目录下的内容，配合参数的使用，能以不同的方式显示目录内容。格式：`ls[参数] [路径或文件名]`

- `ls`：展示的能看见的文件（和目录）的名称
- `ls -a`：展示所有的文件和目录，文件前面有`.`代表的是隐藏文件
- `ls -l`：显示文件的详细信息，简写的方式: `ll`
- `ll -h`：友好的显示

### 3.4 浏览文件

- `cat` 命令用于显示文件的所有内容。格式：`cat[参数] <文件名>`，如`cat yum.conf`

- `more` 命令一般用于要显示的内容会超过一个画面长度的情况，按空格键显示下一个画面，按回车键下一行，按 q 键退出查看，如 `more yum.conf`

- `less` 命令用法和 `more` 类似，不同的是 `less` 可以通过 PgUp、PgDn 键来控制，如`less yun.conf`

- `tail` 命令是在实际使用过程中使用非常多的一个命令，它的功能是：用于显示文件后几行的内容

  - `tail -10 /etc/passwd`：查看后 10 行数据
  - `tail -f catalina.log`：动态查看日志，`ctrl+c` 结束查看


### 3.5 文件操作

①创建文件

- `touch 文件名`：创建一个空白的文件

②复制和移动

cp（copy） 命令：可以将文件从一处复制到另一处。一般在使用 cp 命令时将一个文件复制成另一个文件或复制到某目录时，需要指定源文件名与目标文件名或目录。

- `cp a.txt b.txt`：将 a.txt 复制为 b.txt 文件
- `cp a.txt ../`：将 a.txt 文件复制到上一层目录中

mv（move）命令： 移动或者重命名

- `mv a.txt ../`：将 a.txt 文件移动到上一层目录中
- `mv a.txt b.txt`：将 a.txt 文件重命名为 b.txt

③删除文件或目录

- `rm a.txt`：删除 a.txt 文件，删除需要用户确认 y/s
- `rm -f a.txt`：不询问，直接删除 a.txt
- `rm -r 目录`：带询问的递归删除
- `rm -rf  a `：不带询问的递归删除 a 目录
- `rm -rf *`：不带询问的递归删除所有文件
- `rm -rf /* `：**自杀**

④打包或解压

`tar` 命令位于 /bin 目录下，它能够将用户所指定的文件或目录打包成一个文件，但不做压缩。一般 Linux 上常用的压缩方式是选用 tar 将许多文件打包成一个文件，再以 gzip 压缩命令压缩成 `xxx.tar.gz`（或称为 xxx.tgz）的文件。

常用的参数有：

``` xml
-c：创建一个新tar文件 
-v：显示运行过程的信息 
-f：指定文件名 
-z：调用gzip压缩命令进行压缩 
-t：查看压缩文件的内容 
-x：解开tar文件
```

常用的组合：

``` xml
-cvf :打包一个文件或者目录
-zcvf:打包并压缩一个文件或者目录 压缩的格式:gzip
-xvf:解压或者打开一个tar文件
```

格式：`tar 参数 文件名 要打包|解压的文件目录`

例如：

- 将当前目录下的所有文件打包成 `test1.tar`：`tar -cvf test1.tar ./*`
- 将当前目录下的所有文件打包并压缩成 `test2.tar.gz`：`tar -zcvf test2.tar.gz ./*`
- 将 `test1.tar` 解压到当前目录：`tar -xvf test1.tar `
- 将 `test1.tar` 解压到 b 目录：`tar -xvf test1.tar -C b`

### 3.6 其他常用命令

①`gerp` 命令：查找符合条件的字符串。

用法：`grep [选项]... PATTERN [FILE]...`

- `grep lang anaconda-ks.cfg`：在文件中查找 lang
- `grep lang anaconda-ks.cfg –color`：高亮显示

②`pwd ` 命令：显示当前所在目录

③`wget` 命令：下载资料

用法： `wget 资源路径`，如：`wget http://nginx.org/download/nginx-1.9.12.tar.gz`

④`man` 命令：查看帮助，如查看 grep 命令用法：`man grep`



## 四、Vi和Vim编辑器

### 4.1 Vim编辑器

#### 4.1.1 Vim介绍

在 Linux 下一般使用 Vi 或 Vim 编辑器来编辑文件。Vi 和 Vim 既可以查看文件也可以编辑文件。

Q：Vi 和 Vim 有什么区别？

A：它们都是多模式编辑器，不同的是 Vim 是 Vi 的升级版本，它不仅兼容 Vi 的所有指令，而且还有一些新的特性在里面。

Vim 的这些优势主要体现在以下几个方面：

1. 多级撤消

   > 在 Vi 里，按 u 只能撤消上次命令，而在 Vim 里可以无限制的撤消。

2. 易用性

   > Vi 只能运行于 Unix 中，而 Vim 不仅可以运行于 Unix、Windows、Mac 等多操作平台。

3. 语法加亮

   > Vim 可以用不同的颜色来加亮你的代码。

4. 可视化操作

   > 就是说 Vim 不仅可以在终端运行，也可以运行于 X Window、Mac OS、 Windows。

5. 对 Vi 的完全兼容

   > 某些情况下，你可以把 Vim 当成 Vi 来使用。

总结： Vi 和 Vim 都是 Linux 中的编辑器，不同的是 Vim 比较高级，可以视为 Vi 的升级版本，Vi 使用于文本编辑，但是 Vim 更适用于 coding。

三种模式：命令行、插入、底行模式。

- 切换到命令行模式：按 Esc 键；

- 切换到插入模式：按 i 、o、a 键；

  ``` xml
  i：在当前位置生前插入
  I：在当前行首插入
  a：在当前位置后插入
  A：在当前行尾插入
  o：在当前行之后插入一行
  O：在当前行之前插入一行
  ```

- 切换到底行模式：按 `:`（冒号）；

例如：

``` xml
- 打开文件：vim 文件名
- 退出：先按 esc，再 :q
- 修改文件：输入 i 进入插入模式
- 保存并退出：先按 esc，再 :wq
- 不保存退出：先按 esc，再 :q!
```

#### 4.1.1 Vim命令合集

1、命令历史

``` xml
以:和/开头的命令都有历史纪录，可以首先键入:或/然后按上下箭头来选择某个历史命令。
```

2、启动 vim

``` xml
在命令行窗口中输入以下命令即可:
vim 直接启动vim
vim filename 打开vim并创建名为filename的文件
```

3、文件命令

- 打开单个文件：`vim file`

- 同时打开多个文件：`vim file1 file2 file3 ...`

- 在 vim 窗口中打开一个新文件：`:open file`

- 在新窗口中打开文件：`:split file`

- 切换到下一个文件：`:bn`

- 切换到上一个文件：`:bp`

- 查看当前打开的文件列表，当前正在编辑的文件会用 [] 括起来：`:args`

- 打开远程文件，比如 ftp 或者 share folder：

  ``` xml
  :e ftp://192.168.10.76/abc.txt
  :e \\qadrive\test\1.txt
  ```

4、vim 模式

- 正常模式（按 Esc 或 `Ctrl+[` 进入） 左下角显示文件名或为空
- 插入模式（按 i 键进入） 左下角显示--INSERT--
- 可视模式（不知道如何进入） 左下角显示--VISUAL--

5、导航命令

``` xml
% 括号匹配
```

6、插入命令

``` xml
i 在当前位置生前插入
I 在当前行首插入
a 在当前位置后插入
A 在当前行尾插入
o 在当前行之后插入一行
O 在当前行之前插入一行
```

7、查找命令

``` xml
/text　　查找text，按n健查找下一个，按N健查找前一个。
?text　　查找text，反向查找，按n健查找下一个，按N健查找前一个。
vim中有一些特殊字符在查找时需要转义　　.*[]^%/?~$

:set ignorecase　　忽略大小写的查找
:set noignorecase　　不忽略大小写的查找
查找很长的词，如果一个词很长，键入麻烦，可以将光标移动到该词上，按*或#键即可以该单词进行搜索，相当于/搜索。而#命令相当于?搜索。

:set hlsearch　　高亮搜索结果，所有结果都高亮显示，而不是只显示一个匹配。
:set nohlsearch　　关闭高亮搜索显示
:nohlsearch　　关闭当前的高亮显示，如果再次搜索或者按下n或N键，则会再次高亮。
:set incsearch　　逐步搜索模式，对当前键入的字符进行搜索而不必等待键入完成。
:set wrapscan　　重新搜索，在搜索到文件头或尾时，返回继续搜索，默认开启。
```

8、替换命令

``` xml
ra		将当前字符替换为a，当期字符即光标所在字符。
s/old/new/		用old替换new，替换当前行的第一个匹配
s/old/new/g		用old替换new，替换当前行的所有匹配
%s/old/new/		用old替换new，替换所有行的第一个匹配
%s/old/new/g  	用old替换new，替换整个文件的所有匹配
:10,20 s/^/		/g 在第10行知第20行每行前面加四个空格，用于缩进。
ddp 	交换光标所在行和其下紧邻的一行。
```

9、移动命令

``` xml
h 左移一个字符
l 右移一个字符，这个命令很少用，一般用w代替。
k 上移一个字符
j 下移一个字符
以上四个命令可以配合数字使用，比如20j就是向下移动20行，5h就是向左移动5个字符，在Vim中，很多命令都可以配合数字使用，比如删除10个字符10x，在当前位置后插入3个！，3a！<Esc>，这里的Esc是必须的，否则命令不生效。
    
w 向前移动一个单词（光标停在单词首部），如果已到行尾，则转至下一行行首。此命令快，可以代替l命令。
b 向后移动一个单词 2b 向后移动2个单词
e，同w，只不过是光标停在单词尾部
ge，同b，光标停在单词尾部。
^ 移动到本行第一个非空白字符上。
0（数字0）移动到本行第一个字符上，
<HOME> 移动到本行第一个字符。同0健。
$ 移动到行尾 3$ 移动到下面3行的行尾
gg 移动到文件头。 = [[
G（shift + g） 移动到文件尾。 = ]]
f（find）命令也可以用于移动，fx将找到光标后第一个为x的字符，3fd将找到第三个为d的字符。
F 同f，反向查找。
跳到指定行，冒号+行号，回车，比如跳到240行就是 :240回车。另一个方法是行号+G，比如230G跳到230行
    
Ctrl + e 向下滚动一行
Ctrl + y 向上滚动一行
Ctrl + d 向下滚动半屏
Ctrl + u 向上滚动半屏
Ctrl + f 向下滚动一屏
Ctrl + b 向上滚动一屏
```

10、撤销和重做

``` xml
u 撤销（Undo）
U 撤销对整行的操作
Ctrl + r 重做（Redo），即撤销的撤销。
```

11、删除命令

``` xml
x 删除当前字符
3x 删除当前光标开始向后三个字符
X 删除当前字符的前一个字符。X=dh
dl 删除当前字符， dl=x
dh 删除前一个字符
dd 删除当前行
dj 删除上一行
dk 删除下一行
10d 删除当前行开始的10行。
D 删除当前字符至行尾。D=d$
d$ 删除当前字符之后的所有字符（本行）
kdgg 删除当前行之前所有行（不包括当前行）
jdG（jd shift + g）   删除当前行之后所有行（不包括当前行）
:1,10d 删除1-10行
:11,$d 删除11行及以后所有的行
:1,$d 删除所有行
J(shift + j)　　删除两行之间的空行，实际上是合并两行。
```

12、拷贝和粘贴

``` xml
yy 拷贝当前行
nyy 拷贝当前后开始的n行，比如2yy拷贝当前行及其下一行。
p  在当前光标后粘贴,如果之前使用了yy命令来复制一行，那么就在当前行的下一行粘贴。
shift+p 在当前行前粘贴
:1,10 co 20 将1-10行插入到第20行之后。
:1,$ co $ 将整个文件复制一份并添加到文件尾部。
正常模式下按v（逐字）或V（逐行）进入可视模式，然后用jklh命令移动即可选择某些行或字符，再按y即可复制

ddp交换当前行和其下一行
xp交换当前字符和其后一个字符
```

13、剪切命令

``` xml
正常模式下按v（逐字）或V（逐行）进入可视模式，然后用jklh命令移动即可选择某些行或字符，再按d即可剪切
ndd 剪切当前行之后的n行。利用p命令可以对剪切的内容进行粘贴
:1,10d 将1-10行剪切。利用p命令可将剪切后的内容进行粘贴。
:1, 10 m 20 将第1-10行移动到第20行之后。
```

14、退出命令

``` xml
:wq 保存并退出
ZZ 保存并退出
:q! 强制退出并忽略所有更改
:e! 放弃所有修改，并打开原来文件。
```

15、窗口命令

``` xml
:split或new 打开一个新窗口，光标停在顶层的窗口上
:split file或:new file 用新窗口打开文件
split打开的窗口都是横向的，使用vsplit可以纵向打开窗口。
Ctrl+ww 移动到下一个窗口
Ctrl+wj 移动到下方的窗口
Ctrl+wk 移动到上方的窗口

//关闭窗口
:close 最后一个窗口不能使用此命令，可以防止意外退出vim。
:q 如果是最后一个被关闭的窗口，那么将退出vim。
ZZ 保存并退出。

//关闭所有窗口，只保留当前窗口
:only

//录制宏
按q键加任意字母开始录制，再按q键结束录制（这意味着vim中的宏不可嵌套），使用的时候@加宏名，比如qa。。。q录制名为a的宏，@a使用这个宏。
```

16、执行 shell 命令

``` xml
:!command
:!ls 列出当前目录下文件
:!perl -c script.pl 检查perl脚本语法，可以不用退出vim，非常方便。
:!perl script.pl 执行perl脚本，可以不用退出vim，非常方便。
:suspend或Ctrl - Z 挂起vim，回到shell，按fg可以返回vim。
```

17、注释命令

``` xml
perl程序中#开始的行为注释，所以要注释某些行，只需在行首加入#
3,5 s/^/#/g 注释第3-5行
3,5 s/^#//g 解除3-5行的注释
1,$ s/^/#/g 注释整个文档。
:%s/^/#/g 注释整个文档，此法更快。
```

18、帮助命令

``` xml
:help or F1 显示整个帮助
:help xxx 显示xxx的帮助，比如 :help i, :help CTRL-[（即Ctrl+[的帮助）。
:help 'number' Vim选项的帮助用单引号括起
:help <Esc> 特殊键的帮助用<>扩起
:help -t Vim启动参数的帮助用-
：help i_<Esc> 插入模式下Esc的帮助，某个模式下的帮助用模式_主题的模式
帮助文件中位于||之间的内容是超链接，可以用Ctrl+]进入链接，Ctrl+o（Ctrl + t）返回
```

19、其他非编辑命令

``` xml
. 重复前一次命令
:set ruler?　　查看是否设置了ruler，在.vimrc中，使用set命令设制的选项都可以通过这个命令查看
:scriptnames　　查看vim脚本文件的位置，比如.vimrc文件，语法文件及plugin等。
:set list 显示非打印字符，如tab，空格，行尾等。如果tab无法显示，请确定用set lcs=tab:>-命令设置了.vimrc文件，并确保你的文件中的确有tab，如果开启了expendtab，那么tab将被扩展为空格。

//Vim教程
1.在Unix系统上
	$ vimtutor
2.在Windows系统上
	:help tutor

:syntax 列出已经定义的语法项
:syntax clear 清除已定义的语法规则
:syntax case match 大小写敏感，int和Int将视为不同的语法元素
:syntax case ignore 大小写无关，int和Int将视为相同的语法元素，并使用同样的配色方案
```

*来源：[Vim命令合集](https://www.cnblogs.com/softwaretesting/archive/2011/07/12/2104435.html)* 

### 4.2 重定向>和>>

- `>`：重定向输出，覆盖原有内容；

- `>>`：重定向输出，又追加功能；

  示例：

  ``` xml
  cat /etc/passwd > a.txt  将输出定向到a.txt中
  cat /etc/passwd >> a.txt  输出并且追加

  ifconfig > ifconfig.txt
  ```

### 4.3 管道

管道是 Linux 命令中重要的一个概念，其作用是将一个命令的输出用作另一个命令的输入。

示例：

``` xml
ls --help | more  分页查询帮助信息
ps –ef | grep java  查询名称中包含java的进程

ifconfig | more
cat index.html | more
ps –ef | grep aio
```

### 4.4 &&命令执行控制

命令之间使用 `&&` 连接，实现逻辑与的功能。 只有在 `&&` 左边的命令返回真（命令返回值 `$? == 0`），`&&` 右边的命令才会被执行。只要有一个命令返回假（命令返回值`$? == 1`），后面的命令就不会被执行。示例：`mkdir test && cd test`

### 4.5 网络通信命令

- `ifconfig`：显示网络设备
- `ifconfig eth0 up`：启用 eth0 网卡
- `ifconfig eth0 down`：停用 eth0 网卡
- `ping`：探测网络是否通畅，如 `ping 192.168.0.1`
- `netstat`：查看网络端口
  - `netstat -an | grep 3306`：查询 3306 端口占用情况

### 4.6 系统管理命令 

- `date`：显示当前系统时间
  - `date -s “2014-01-01 10:10:10“`：设置系统时间
- `df -h`：友好显示磁盘信息
- `free -m`：以 mb 单位显示内存组昂头
- `top`：显示，管理执行中的程序
- `clear`：清屏幕
- `ps`：正在运行的某个进程的状态
  - `ps -ef`：查看所有进程
  - `ps –ef | grep ssh`：查找某一进程
- `kill`：杀掉某一进程
  - `kill 2826`：杀掉 2826 编号进程
  - `kill -9 2826`：强制杀死进程
- `du`：显示目录或文件大小
  - `du -h`：显示当前目录的大小
- `who`：显示目前登入系统的用户信息
- `hostname`：查看当前主机名，修改 `vi /etc/sysconfig/network`
- `uname`：显示系统信息
  - `uname -a`：显示本机详细信息，依次为：内核名称（类别），主机名，内核版本号，内核版本，内核编译日期，硬件名，处理器类型，硬件平台类型，操作系统名称




## 五、Linux的用户和组

### 5.1 用户的管理

- `useradd`：添加一个用户
  - `useradd test`：添加 test 用户
  - `useradd test -d /home/t1`：指定用户 home 目录
- `passwd`：设置、修改密码
  - `passwd test`：为 test 用户设置密码
- 切换用户：
  - `ssh -l 用户名 -p 22 主机`，例如：`ssh -l tom -p 22 192.168.17.131`
  - `su - 用户名`：
- `userdel`：删除一个用户
  - `userdel test`：删除 test 用户（不会删除 home 目录）
  - `userdel -r test`：删除用户以及 home 目录

### 5.2 组管理

当在创建一个新用户 user 时，若没有指定他所属于的组，就建立一个和该用户同名的私有组。创建用户时也可以指定所在组。

- `groupadd`：创建组
  - `groupadd public`：创建一个名为 public 的组
  - `useradd u1 –g public`：创建用户指定组
- `groupdel`：删除组，如果该组有用户成员，必须先删除用户才能删除组
  - `groupdel public`：删除一个名为 public 的组

### 5.3 id、su命令

- `id` 命令：查看一个用户的 UID 和 GID

  用法：`id [选项]... [用户名]`

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-66211664.jpg)

- `su`命令：切换用户。

  用法：`su [选项]... [-] [用户 [参数]... ]`

  示例：`su - u1` 切换到 u1 用户，并且将环境也切换到 u1用户的环境（推荐使用）



## 六、Linux的权限命令

### 6.1 文件权限

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-41580363.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-5-74206597.jpg)

### 6.2 Linux的三种文件类型

- 普通文件： 包括文本文件、数据文件、可执行的二进制程序文件等。 
- 目录文件： Linux 系统把目录看成是一种特殊的文件，利用它构成文件系统的树型结构。  
- 设备文件： Linux 系统把每一个设备都看成是一个文件

### 6.3 文件类型标识

- 普通文件：包括文本文件、数据文件、可执行的二进制程序文件等。标识为 `-`
- 目录文件：Linux 系统把目录看成是一种特殊的文件，利用它构成文件系统的树型结构。
- 符号链接：标识为 `l`
- `*` 进入 etc 可以查看，相当于快捷方式
- 字符设备文件：标识为 `c`
- 块设备文件：标识为 `s`
- 套接字：标识为 `s`
- 命名管道：标识为 `p`

### 6.4 文件权限管理

- `chmod`：变更文件或目录的权限
  - `chmod 755 a.txt`
  - `chmod u=rwx,g=rx,o=rx a.txt`
  - `chmod 000 a.txt`：虽然设置为没有任何权限，但对于超级管理员 root 来说，还是有权限的，比如写入
  - `chmod 777 a.txt`：所有都用户都有权限
- `chown`：变更文件或目录改文件所属用户和组
  - `chown u1:public a.txt`：变更当前的目录或文件的所属用户和组
  - `chown -R u1:public dir`：变更目录中的所有的子目录及文件的所属用户和组




更详细学习资料推荐：

- *[《鸟哥Linux私房菜-基础篇》](http://oqo3t056d.bkt.clouddn.com/vbird-linux-basic-4e.pdf)*
- *视频：[史上最牛的Linux视频教程—兄弟连](https://www.bilibili.com/video/av18156598/?p=1)*

