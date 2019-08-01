## 一、VMware虚拟机安装Ubuntu

### 1.1 介绍

1、VMware 是什么？

威睿（英语：VMware, Inc.）是一家全球著名的软件公司，它提供云计算和硬件虚拟化的软件和服务，并号称是第一个商业化的成功的虚拟化的x86架构。公司成立于1998年，VMware的总部设在加利福尼亚州帕洛阿尔托。2004年，威睿被易安信公司收购控股持有，然后，在2007年8月14日，易安信公司在纽约证券交易所上市公司所出售的15％控股。该公司在符号VMW下交易。——from：[维基百科](https://zh.wikipedia.org/zh-hans/VMware)

2、VMware Workstation 

VMware Workstation是VMware公司销售的商业软件产品之一。该工作站软件包含一个用于英特尔x86相容电脑的虚拟机套装，其允许用户同时创建和运行多个x86虚拟机。每个虚拟机可以运行其安装的操作系统，如（但不限于）Windows、Linux、BSD变生版本。用简单术语来描述就是，VMware Workstation允许一台真实的电脑在一个作业系统中同时开启并运行数个操作系统，其它VMware产品帮助在多个宿主电脑之间管理或移植VMware虚拟机。免费版本为VMware Workstation Player。——from：[维基百科](https://zh.wikipedia.org/zh-hans/VMware)

3、Ubuntu 简介

Ubuntu（友帮拓、优般图、乌班图）是一个以桌面应用为主的开源GNU/Linux操作系统，Ubuntu 是基于Debian GNU/Linux，支持x86、amd64（即x64）和ppc架构，由全球化的专业开发团队（Canonical Ltd）打造的。

其名称来自非洲南部祖鲁语或豪萨语的“ubuntu”一词，类似儒家“仁爱”的思想，意思是“人性”、“我的存在是因为大家的存在”，是非洲传统的一种价值观。

Ubuntu基于Debian发行版和GNOME桌面环境，而从11.04版起，Ubuntu发行版放弃了Gnome桌面环境，改为Unity，与Debian的不同在于它每6个月会发布一个新版本。Ubuntu的目标在于为一般用户提供一个最新的、同时又相当稳定的主要由自由软件构建而成的操作系统。Ubuntu具有庞大的社区力量，用户可以方便地从社区获得帮助。 Ubuntu对GNU/Linux的普及特别是桌面普及作出了巨大贡献，由此使更多人共享开源的成果与精彩。

2013年1月3日，Ubuntu正式发布面向智能手机的移动操作系统。

ubuntu基于linux的免费开源桌面PC操作系统，十分契合英特尔的超极本定位，支持x86、64位和ppc架构。

——*摘自 [百度百科](https://baike.baidu.com/item/Ubuntu)*

### 1.2 安装步骤

1、下载 VMware Workstation、Ubuntu

- VMware Workstations [下载](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)

- Ubuntu [下载](https://www.ubuntu.com/download/desktop#download)

  注意到官网有如下版本：

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-54104674.jpg)

  我们选择第一个 Ubuntu Desktop 下载。关于相关区别参考：

  - [Ubuntu桌面版与服务器版的区别](https://www.linuxidc.com/Linux/2010-11/29740.htm)
  - [Ubuntu的桌面和服务器版到底有什么区别呢？](https://www.zhihu.com/question/20590313)

  大致区别概述：

  > Ubuntu 桌面和服务器版两者 kernel 相同，但是为不同目的服务，所以安装不同的 package
  >
  > - ubuntu server 安装了服务器的标准 packages： web server，email server，file server，samba server，LAMP
  > - ubuntu desktop 安装了办公需要的 packages：deskop environment(Gnome、X11)，web browser，music player，libre office
  >
  > 桌面具有更高的前台响应性能，服务器更倾向于后台服务。还有电源管理，桌面版配置更节能，服务器版配置则启用高性能。

2、安装过程

①本人下载的版本为 VMware Workstation 14 Pro，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-19901436.jpg)



VMware Workstation 安装过程就不多赘述了，和平时安装软件一样，可参考：[虚拟机VMware Workstation Pro v14.0官方下载安装与激活教程](http://www.xitongcheng.com/jiaocheng/xtazjc_article_38952.html)。安装完成之后启动 VMware，需要输入产品密钥，以下是几个可以用的 VMware Workstation 14 永久激活注册码：

``` xml
FF31K-AHZD1-H8ETZ-8WWEZ-WUUVA
CV7T2-6WY5Q-48EWP-ZXY7X-QGUWD
AA702-81D8N-0817Y-75PQT-Q70A4
AC310-0VG0P-M88CQ-YWY5Z-QPRG0
AF70K-DNWEQ-H8DPY-0XN7X-MCAUA
UV1H2-AKWD2-H8EJZ-GGMEE-PCATD
YC592-8VF55-M81AZ-FWW5T-WVRV0
YF5XA-62G0P-48EDP-U4WEG-YPKZF
```

② 本人 Ubuntu 下载的为：`ubuntu-16.04-desktop-amd64.iso`

使用 VMware 虚拟机安装 Ubuntu 详细过程如下。可参考：[VMware Ubuntu安装详细过程](https://blog.csdn.net/u013142781/article/details/50529030)

该软件安装过程我就在此省略，不贴图片了。。。

补充和需要注意的地方：

1. 在网络适配器网络连接设置中，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-72103731.jpg)

有什么区别呢？

- Bridged（桥接模式）：在这种模式下，VMWare 虚拟出来的操作系统就像是局域网中的一台独立的主机，它可以访问网内任何一台机器。你可以手工配置它的 TCP/IP 配置信息（IP、子网掩码等，而且还要和宿主机器处于同一网段），以实现通过局域网的网关或路由器访问互联网；还可以将 IP 地址和 DNS 设置成“自动获取”。如果你想利用 VMWare 在局域网内新建一个虚拟服务器，为局域网用户提供网络服务，就应该选择桥接模式。

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-88779842.jpg)

  > - A1、A2、A、B 四个操作系统可以相互访问
  > - A1、A2 的 IP 为“外网” IP，可以手动设置，也可以自动获取

- NAT（网络地址转换模式）：使用 NAT 模式，就是让虚拟系统借助 NAT 的功能，通过宿主机所在的网络来访问公网。在这种模式下宿主机成为双网卡主机，同时参与现有的宿主局域网和新建的虚拟局域网，但由于加设了一个虚拟的 NAT 服务器，使得虚拟局域网内的虚拟机在对外访问时，使用的则是宿主机的 IP 地址，这样从外部网络来看，只能看到宿主机，完全看不到新建的虚拟局域网。采用 NAT 模式最大的优势是虚拟系统接入互联网非常简单，你不需要进行任何其他的配置，只需要宿主机器能访问互联网即可。如果你想利用 VMWare 安装一个新的虚拟系统，在虚拟系统中不用进行任何手工配置就能直接访问互联网，建议采用 NAT 模式。

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-38371257.jpg)

  > - A1、A2 可以访问 B
  > - B 不可以访问 A1、A2
  > - A1、A2、A 相互访问
  > - A1、A2 的 IP 为局域网 IP，可以手动配置，也可以自动获取

- Host-only（主机模式）：在某些特殊的网络调试环境中，要求将真实环境和虚拟环境隔离开，这时你就可采用 host-only 模式，在这种模式下宿主机上的所有虚拟机是可以相互通信的，但虚拟机和真实的网络是被隔离开的。

  在这种模式下新建了一个由所有虚拟机与宿主机所构成的局域网，但该局域网与宿主机本身所处的现有局域网是相互独立的，如果不做额外路由设置，这两个局域网之间不会连通，因此新建的局域网可以认为是一个单独从属于当前宿主机的私有网络，其成员为当前宿主机和相关的所有虚拟机。

  从网络技术上讲相当于为宿主机增添了一个虚拟网卡，让宿主机变成一台双网卡主机（宿主网卡+虚拟网卡）。同时在宿主机后端加设一个虚拟交换机，让宿主机和所有虚拟机构成另一个虚拟的局域网。由于具备双网卡，宿主机可同时参与两个局域网（现有的宿主局域网+新建的虚拟局域网），只不过缺省情况下两个局域网不连通。

  如果你想利用 VMWare 创建一个与网内其他机器相隔离的虚拟系统，进行某些特殊的网络调试工作，可以选择 host-only 模式。

  > PS：三种模式中，虚拟机的 IP 都可以设置成自动获取，一旦自动获取后不会改变（除非手动配置），也就是下次重启后的 IP 与上次相同。

### 1.3 ubuntu常用操作

ubuntu 使用指南：https://help.ubuntu.com/

- 改变ubuntu 16.04 菜单栏位置到底部，终端下输入命令：`gsettings set com.canonical.Unity.Launcher launcher-position Bottom`；恢复到原来的左侧命令：`gsettings set com.canonical.Unity.Launcher launcher-position Left`

ubuntu 常用命令介绍下：

①权限有关

- sudo：意思就是 super-user do，让当前用户暂时以管理员的身份 root 来执行这条命令，如`sudo apt install openssh-client`，表示当前命令是root身份执行的。

  > sudo 和 su 最大区别：`sudo` 命令需要输入当前用户的密码，`su` 命令需要输入 root 用户的密码。很明显，就安全而言，`sudo` 命令更好。例如，考虑到需要 root 访问权限的多用户使用的计算机。在这种情况下，使用 `su` 意味着需要与其他用户共享 root 用户密码，这显然不是一种好习惯。

- su：用来改变当前用户的，如 `su root`，就是将当前用户切换为 root，用了 `su root` 之后，下面所有的命令就可以不用打 sudo了，因为当前用户已经是管理员 root 了。

  > 主要作用是让你可以在已登录的会话中切换到另外一个用户，换句话说，这个工具可以让你在不登出当前用户的情况下登录为另外一个用户。
  >
  > 注：root 身份命令行前面为`#`，普通用户为`$`。

- `sudo su`：使用 `sudo` 运行命令，你只需要输入当前用户的密码。所以，一旦完成操作，`su` 命令将会以 root 用户身份运行，这意味着它不会再要求输入任何密码。

②修改密码

- `passwd 当前用户名`：修改用户密码，会提示修改密码，密码位数必须大于等于6位，如果小于6位的话，会提示“必须选择更长的密码”，如果一定要设置位6位以下的密码的话，要加上 su 权限，可以用 `sudo passwd 当前用户名`。


- `sudo passwd`或`sudo passwd root`：设置 root 密码。

VMware 虚拟机下安装的 ubuntu 的一些常用快捷键：

- Ctrl+Alt+Enter：全屏
- 取消独占模式：Ctrl+Alt


ubuntu 常用的快捷键：

①桌面

- Alt + F1：聚焦到桌面左侧任务导航栏，可按上下键导航。
- Alt + F2：运行命令
- Alt + F4：关闭窗口
- Alt + TAB：切换程序窗口
- Alt + 空格：打开窗口菜单
- Print：桌面截图
- SUPER：打开 Dash 面板，可搜索或浏览项目，默认有个搜索框，按“下”方向键进入浏览区域（SUPER 键指Win 键或苹果电脑的 Command 键）
- 在 Dash 面板中按 Ctrl + Tab：切换到下一个子面板（可搜索不同类型项目，如程序、文件、音乐）
- SUPER + A：搜索或浏览程序（Application）
- SUPER + F：搜索或浏览文件（File）
- SUPER + M：搜索或浏览音乐文件（Music）

②终端

- Ctrl+ Alt + T：打开终端
- Tab：自动补全命令或文件名
- Ctrl+ Shift+ V: 粘贴（Linux中不需要复制的动作，文本被选择就自动被复制）
- Ctrl+ Shift+ T: 新建标签页
- Ctrl+ D: 关闭标签页
- Ctrl+ L: 清除屏幕
- Ctrl+ R + 文本: 在输入历史中搜索
- Ctrl+ A: 移动到行首
- Ctrl+ E: 移动到行末
- Ctrl+ C: 终止当前任务
- Ctrl+ Z：把当前任务放到后台运行（相当于运行命令时后面加&）


### 1.4 导出导入系统

使用 VMware Workstation 导出安装的系统，下次就算换了电脑也可导入到别的 VMware 中使用了。

#### 1.4.1 导出

先选中需要导出的系统，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-14-66667755.jpg)

再依次点击「文件」--> 「导出为 OVF」，如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-14-61984388.jpg)

选择需要导出到的目录、自定义文件名，然后就是等待了...

最后可以看到导出的文件：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-14-92316285.jpg)

这里提下，如上图看到的有个 `Ubuntu_64位2-file1.iso` 文件，其实是来自（如下图）：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190725200331.png)

**建议在导出的时候删除掉（导入的时候不会受此影响）。**删除掉后，再进行导出，就只会有这三个 `.ovf`、`.vmdk`、`.mf` 格式文件。

#### 1.4.2 导入

VMware Workstation 中选择「打开虚拟机」，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-14-19153652.jpg)

然后选择后缀 `.ovf` 文件就可以成功导入系统了。

> 补充：OVF 是一种虚拟机打包和分发格式，具有独立于平台、高效、可扩展且开放的特点。OVF 格式提供了完整的虚拟机规范，包括所需虚拟磁盘和所需虚拟硬件配置的完整列表。虚拟硬件配置包括 CPU、内存、网络和存储。管理员无需干预或只需进行极少干预，即可快速置备 OVF 格式的虚拟机。

若是导入碰到提示“导入失败”，提示类似如下这样：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190725210801.png)

不用在意，点击“重试”即可。

我在实践过程，发现导入进来之后，点击虚拟机系统，都有**“升级此虚拟机”**字样，这里可以忽略，或者你也可以打开系统的 vmx 配置文件，修改 virtualhw.version 的值为 14。关于什么是 vmx 文件，下面有讲到。

##### 导入 macOS 出现的问题：Attempting to start up from

我有在 VMware pro 14.0 虚拟机下安装 macOS 10.13，参考：[VMware 15 安装 MAC OS 10.13 原版（详细图文教程） - CSDN博客](<https://blog.csdn.net/qq_40147863/article/details/84797618>)，文章也有提到安装可能会遇到的问题，这里不多讲了。

这里得提下，在导入 macOS 到虚拟机遇到的问题。

**问题1：**

在导入 macOS 后，会报：

``` 
VMware Workstation 不可恢复错误: (vcpu-0)
```

这样的错误。**下面是解决办法：**

1. 打开选择的系统的那个目录，找到 wmx 为后缀名的文件，用编辑器打开；

2. 找到 smc.present = “TRUE” 那一行，下面添加一行，内容为：

   ```
   smc.version = 0
   ```

这里提下 vmx 文件：

> vmx 文件是 vmware 安装虚拟系统后，生成的虚拟机配置文件。可以用普通的文本编辑器编辑，其中保存着虚拟机的各项软硬件配置信息，诸如硬盘，光盘，网卡，虚拟机名等配置信息。

**问题2：**

解决问题1 后，点击启动系统，然后报了如下类似错误：

```
Attempting to start up from
EFI VMware Virtual SATA CDROM Drive (1.0) … unsuccessful.

```

网上找到了该问题解决方式，很多都有提到该解决方式：

```
1. 在虚拟机的安装目录里找到vmx文件
2. 删掉里面的 firmware="efi"
3. 保存重启虚拟机即可正常安装
```

但实测，按照如上方法，并没解决问题。后通过自己的倒腾，找到了解决办法。操作很简单：

1. 找到导入进来 macOS 后对应的 vmx 文件。

2. 然后修改 guestOS 值为 darwin17-64：

   ```
   guestOS = "darwin17-64"
   ```

即可解决该问题。我猜测是因为在开始安装 macOS 系统，使用了 darwin.iso 文件缘故。

## 二、SSH远程连接Ubuntu

VMware 虚拟机下安装完 Ubuntu 系统就可以开始使用了！

关于什么是 SSH，参考阮一峰老师文章，写的非常清晰了：[SSH原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)。

这里还有一篇写的很清楚，推荐阅读：[最佳实践：使用SSH连接Linux服务器](https://www.jianshu.com/p/59c4fc2684be)。

简单说，SSH（Secure Shell，安全外壳协议） 是一种网络协议，用于计算机之间的加密登录。存在多种实现，既有商业实现，也有开源实现。建议好好理解下 SSH 协议原理，HTTPS 协议也有相关知识点，日后有时间我来写篇关于 HTTPS 的认识吧。

> 注：使用 Git 在 GitHub 进行多人合作代码管理等操作之前，一般也会配置 SSH 公钥私钥，这样每次提交代码就不用输入账户和密码。

### 2.1 口令方式登录

1、前言

在新安装的 Ubuntu 系统后，默认是不支持 ssh 登录的，但是 ssh 登录时我们平时经常用到的，所以先安装 ssh 服务。查看当前 ubuntu 是否安装了 ssh-server 服务：`dpkg -l | grep ssh`（默认只安装了 ssh-client 服务）

> 解释下：SSH 分客户端`openssh-client`和服务端`openssh-server`。如果你只是想登录别的机器，那么只需要安装`openssh-client`，命令：`sudo apt install openssh-client`；如果要使本机开放 SSH 服务，也就是让别人可以连接你的机器，那么就需要安装`openssh-server`，命令：`sudo apt install openssh-server`。

1. 在 ubuntu 下执行命令：`sudo apt-get install openssh-server openssh-client`，等待安装完成；

   > 可以把 SSH 客户端和服务端都一并都给安装，省得到时候怕出什么问题。

2. 再查看 ssh 是否在运行，执行命令`sudo ps -e |grep ssh`，如果没有任何显示，是没有运行 ssh 服务的，反之，出现 sshd 字样，表示 ssh 服务已运行。如果 ssh 没有运行，使用命令`sudo service ssh start` 启动。

   涉及的相关一些操作记录在此：

   - 启动 ssh 服务

     ``` xml
     #启动方式1
     /etc/init.d/ssh start
     #启动方式2
     service ssh start
     #启动方式3 适用于  Ubuntu Linux 16.04 LTS或以上的系统
     systemctl start ssh
     ```

   - 关闭 ssh 服务

     ``` xml
     #关闭方式1
     /etc/init.d/ssh stop
     #关闭方式2
     service ssh stop
     #关闭方式3 适用于  Ubuntu Linux 16.04 LTS或以上的系统
     systemctl stop ssh
     ```

   - 重新启动 ssh 服务

     ``` xml
     #启动方式1
     /etc/init.d/ssh restart
     #启动方式2
     service ssh restart
     #启动方式3 适用于  Ubuntu Linux 16.04 LTS或以上的系统
     systemctl start restart
     ```

   - 查看 ssh 服务状态

     ``` xml
     #启动方式1
     /etc/init.d/ssh restart
     #启动方式2
     service ssh restart
     #启动方式3 适用于  Ubuntu Linux 16.04 LTS或以上的系统
     systemctl start restart
     ```

     > 至此，ssh 已经安装并运行起来，可以使用 ssh 客户端连接了。但是，可能还会连接不上怎么办？看下一步。

3. 修改配置文件：/etc/ssh/sshd_config（该配置文件详解参考：[SSH远程登录配置文件sshd_config详解](https://blog.csdn.net/Field_Yang/article/details/51568861)）

   1. 使用命名打开配置文件：`gedit /etc/ssh/sshd_config`；
   2. 将配置文件中的 `PermitRootLogin without-password` 一行注释掉；
   3. 添加新的一行内容：`PermitRootLogin yes`；
   4. 保存配置文件，重启 ssh 服务：`/etc/init.d/ssh restart`或`service ssh restart`。

   补充：

   > ①ssh-server 配置文件位于 `/etc/ssh/sshd_config`，在这里可以定义 SSH 的服务端口，默认端口是 22，你可以自己定义成其他端口号如 222。
   >
   > ②如果不进行第三步修改配置文件，普通用户还是能以 ssh 口令方式登录，但是 root 身份登录不了；修改完成之后重启 ssh 则便 root 身份也能登录了。

2、登录

Ubuntu 上安装了 ssh 服务，Windows 下可以使用 xshell 或者 putty 等软件连接这台 ubuntu 了。（但是每次都得输入用户名和密码，即使 xshell 可以存储用户名和密码但是登陆速度很慢。所以可以使用公共密钥的登陆方式来提高速度和安全性，下一节会讲到。）

倘若本机曾安装过 Git，也可以，因为 Git 自带 ssh 程序，正好可以使用 ssh 登录 ubuntu，命令格式：

``` xml
$ssh -p[端口号] [user]@[ip]
```

演示：（SSH 的默认端口是 22，也可不用输入端口，直接 `ssh jaybo@192.168.109.128`）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-73100778.jpg)

当然如果是第一次登录，还会一些提示，大概意思是：

> 无法确认 host 主机的真实性，只知道它的公钥指纹，问你还想继续连接吗？

接受即可。然后会要求输入密码，如果密码正确，就可以登录了。

### 2.2 公钥方式登录

如上口令方式登录每次都要输入密码，挺麻烦的，也不是很安全，此时可以添加公钥认证来免去输入密码的麻烦并提高安全性。参考：

- [Ubuntu 远程登陆服务器 ssh的安装和配置](https://www.jianshu.com/p/029b14e2d0af)
- [windows如何使用ssh登录ubuntu](https://blog.csdn.net/xugen12/article/details/51918284)

大致步骤如下：

1. 在 Ubuntu 下输入该命令：`ssh-keygen`（当然该命令还有一些参数，可自行网上搜索了解）

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-47978603.jpg)

   如上图，其过程会提示输入 ssh 的登录密码，直接 Enter 键密码留空即可，如此可在使用私钥认证的时候免去输入密码的麻烦。默认密钥的位置在`~/.ssh`目录中。

   `cd .ssh`切换到该目录可以看到生成的私钥`id_rsa`和公钥`id_rsa.pub`：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-31290436.jpg)

   记住无论如何都不要暴露自己的私钥`id_rsa`

2. 这里将公钥改名为 `authorized_keys`：

   ``` xml
   #使用 mv 命令可以达到改名的作用
   mv id_rsa.pub authorized_keys  
   ```

3. 修改配置文件`/etc/ssh/sshd_config`：`sudo vi /etc/ssh/sshd_config  `

   ``` xml
   ......  
   # AuthorizedKeysFile  %h/.ssh/authorized_keys  
   .....  
   ```

   将 AuthorizedKeysFile 前的 # 号去掉即可。

4. 到 Windows 平台，启动 PuTTYgen，导入先前生成的私钥 `id_rsa` 转换成 Putty 所识别的格式（`*.ppk`），得到文件 `id_rsa.ppk`。

   PuTTY 下载地址：http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

   > 关于 PuTTY：PuTTY 是一款集成虚拟终端、系统控制台和网络文件传输为一体的自由及开放源代码的程序，其包含了如下工具：（安装完成之后即可使用如下些工具，当然也可以找包含如下工具的绿色版使用）
   >
   > ``` xml
   > Putty (Telnet和SSH客户端工具)
   > PSCP (Scp客户端,命令行下通过SSH拷贝文件)
   > PSFTP (Sftp命令行客户端,类似于FTP文件传输)
   > Puttytel (Telnet客户端)
   > Plink (命令行工具,远程执行服务器上的命令)
   > PuttyGen (生成DSA和RSA密钥)
   > Pageant (Putty、PSCP、Plink的认证代理)
   >
   > 使用说明：Putty在Windows环境下直接可用。如putty.exe可直接用.  pscp、psftp需要在cmd环境下用。
   > ```

   打开 PuTTYgen：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-87041863.jpg)

   选择 Load 将 Linux 下的 `id_rsa` 文件导入，将 Linux 环境下的账户密码填入：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-61742165.jpg)

   > 注：这里有一步操作，如何将 ubuntu 下的文件私钥 `id_rsa` 拷贝到 Windows 下。具体方法下一小节会讲到。这里采用了 pscp 命令行方式：（如下为拷贝到了 D 盘根目录，其实若安装过 VMware Tools，则可以直接采用“拖动”文件方式拷贝）
   >
   > ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-8333940.jpg)

   选择 Save private key，保存为 `id_rsa.ppk`。

5. Windows 下启动 putty 软件，进行如下配置：

   - Session - Logging - Hostname：填上你的 ubuntu 的 IP 地址
   - Windows －Translation：在下拉菜单里选上 `UTF-8`，这里不设置，登录后将会出现中文乱码
   - Connection - Data - Auto login username：填上你登录 ubuntu 时用的用户名。
   - Connection - SSH - Auth - Private key file for authentication：选上 `id_rsa.ppk` 文件
   - 保存 Session 配置

6. 打开配置的 ssh，成功登录。

注：如上过程，其实也可在 Windows 下生成公钥私钥，然后把公钥上传到 ubuntu 上。比如安装了 Git，可以直接在 Git Bash 下`ssh-keygen`生成公钥私钥。

关于远程连接 CentOS 系统可参考：

- [菜鸟Linux系列：[4]SSH免密码登陆远程服务器](https://jingyan.baidu.com/article/2fb0ba4043124a00f2ec5f0f.html)

### 2.3 文件传输

如何在 Linux 和 Windows 、Linux 和 Linux 相互之间传输文件呢？这也是经常碰到的一件事情。

#### 2.3.1 Linux和Windows之间传输

传输的前提是 Linux 机器要先安装好 ssh。前文已讲到过如何安装 ssh，就不赘述了。

安装完成之后，启动 `openssh-server`，解释下：

> 如果你的机器没有启动 openssh-server，那么别人就连接不了你的机器，也没法从你的机器拷贝文件，但是你可以连别的开启了 openssh-server 的机器，也可以拷贝文件，因为你本机的 openssh-client 安装后就默认打开。

因为本人安装过 PuTTY，自带 pscp，该工具可以命令行下通过 SSH 拷贝文件。如果是只下载了单个 pscp 工具，可以将其放入 windows 的 system32 文件夹下，这样在 dos 命令窗口中就能直接调用使用了。

要传送文件的时候，假设你要将 Windows 下的 `d:/test.txt` 传到 IP 为 `192.168.109.128` 的 Linux 机器的 `/home/user1 `文件夹下，假设 Linux 机器的端口是 22，用户名是：user1，那么从 Windows 下打开 cmd：

1. Windows 向 Linux 拷贝文件：`pscp -P 22 d:\test.txt user1@192.168.109.128:/home/user1`

   这里 `-P` 指定端口，@ 符号的前面是用户名，后面是 Linux 机器的地址。输完这个命令会提示输入密码，输入即可，下同。

2. Linux 向 Windows 拷贝文件，在 Windows 的 cmd 中输入：`pscp -P 22 user1@192.168.109.128:/home/user1/test.txt d:/`

   如上是将 Linux 机器下的 `/home/user1/test.txt` 文件拷贝到 Windows 机器的 D 盘下。

   如果要拷贝文件夹，则多加一个 `-r` 参数即可：`pscp -r -P 22 user1@192.168.109.128:/home/user1/test d:/`

#### 2.3.2 Linux和Linux之间传输

同样采用 scp 命令。

假设从另外一台 Linux 机器拷贝文件到本机，另外一台 Linux 机器的地址是 `192.168.109.127`，用户名是user2，端口是 22，要拷的目录是 `/home/user2/test.txt`，那么命令行输入：`scp -P 22 user2@192.168.109.127：/home/user1/test.txt .`

最后一个 `.` 符号表示拷贝到本机的当前目录，也就是命令行当前所处的目录，当然也可以指定一个存放目录。

如果要拷贝文件夹，则多加一个 `-r` 参数即可。

**可能遇到的问题：**

①scp 远程拷贝文件时提示错误： 

``` xml
Warning: Permanently added ‘[10.100.200.11]:22’ (ECDSA) to the list of known hosts. 
Permission denied (publickey). 
```

解决：可能是你的机器重启但是没开 ssh 服务，重启 ssh 服务即可：`/etc/init.d/ssh restart`。

参考：[linux和windows、linux和linux传文件](https://blog.csdn.net/u014380165/article/details/78210260)



### 补充：SSH远程连接CentOS

在虚拟机（Vmware Workstation）下，安装了 CentOS，现在想通过 SSH 工具连接虚拟机中的 CentOS7。

1. 首先，要确保 CentOS 安装了  `openssh-server`，在终端中输入：`yum list installed | grep openssh-server`

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190730110741.png)

   此处显示已经安装了  `openssh-server`，如果没任何输出显示表示没有安装  `openssh-server`，通过输入  `yum install openssh-server` 来进行安装 `openssh-server`。

2. 找到了  **/etc/ssh/**  目录下的 sshd 服务配置文件 sshd_config，用 vim 编辑器打开，将文件中，关于监听端口、监听地址前的 # 号去除，然后开启允许远程登录，最后，开启使用用户名密码来作为连接验证，保存文件，退出。
   
3. 开启  sshd  服务，输入 `sudo service sshd start`。检查  sshd  服务是否已经开启，输入 `ps -e | grep sshd`。或者输入 `netstat -an | grep 22`  检查  22 号端口是否开启监听。

这里提下查看 CentOS 的 IP 地址的问题：

我们输入 ip 查询命名 ip addr  也可以输入 ifconfig 查看 ip，但此命令会出现3个条目，centos 的 ip 地址是 ens33 条目中的 inet 值。

发现 ens33 没有 inet 这个属性，那么就没法通过 ip 地址连接虚拟机。

接着来查看 ens33 网卡的配置：`vi /etc/sysconfig/network-scripts/ifcfg-ens33`，注意 vi 后面加空格。从配置清单中可以发现 CentOS 7 默认是不启动网卡的（ONBOOT=no），把这一项改为 YES（ONBOOT=yes），然后按 Esc 退出  再出入命令 :wq  再按 Enter 即可。然后重启网络服务：`sudo service network restart`。

然后再输入  ip addr 命令，可以看到 inet 属性显示了虚拟机 CentOS 的 ip 地址。



## 三、windows远程控制ubuntu桌面

在使用 VMware 虚拟机安装完 ubuntu 后，我们总得折腾点什么以更方便使用 ubuntu 系统。

### 3.1 VMware Tools

#### 3.1.1 简介

VMware Tools 是用于增强虚拟机的客户机操作系统性能并改进虚拟机管理的实用程序套件。如果不在客户机操作系统中安装 VMware Tools，客户机性能将缺少重要的功能。安装 VMware Tools 会避免出现以下问题或改进性能：

- 视频分辨率低
- 色深不足
- 网速显示错误
- 鼠标移动受限
- 不能复制、粘贴和拖放文件
- 没有声音
- 提供创建客户机操作系统静默快照的能力
- 将客户机操作系统中的时间与主机上的时间保持同步

VMware Tools 安装后，可以让我们在 Windows 下更好的管理虚拟机。

 虚拟机VMware tools的作用：

1. 更新虚拟机中的显卡驱动, 使虚拟机中的 XWindows 可以运行在 SVGA 模式下。在客户操作系统中安装 Mware Tools 非常重要。如果不安装 VMware Tools，虚拟机中的图形环境被限制为 VGA 模式图形(640x480，16色)。使用 VMware Tools，SVGA 驱动程序被安装，VMware Workstation 支持最高 32 位显示和高显示分辨率，显著提升总体的图形性能。
2. 在主机和客户机之间时间同步。  注意: 只有当你在客户操作系统中设置时钟为一个比在主机中设置的时间更早的时间时，才可以在客户和主机操作系统之间同步时间。 
3. 支持同一个分区的真实启动和从虚拟机中启动, 自动修改相应的设置文件。 
4. 自动捕获和释放鼠标光标。未安装 VMware Tools 的时候只能用 Ctrl+Alt 来释放鼠标，安装 VMware Tools 后可以实现虚拟机和主机图形用户界面之间平滑移动鼠标光标。 
5. 在主机和客户机之间或者从一台虚拟机到另一台虚拟机进行复制和粘贴操作。
6. 改善网络性能。 

#### 3.1.2 安装

参考：[VMware Tools （ubuntu系统）安装详细过程与使用](https://blog.csdn.net/u013142781/article/details/50539574)

步骤如下大致如下：

1、点击 VMware Workstation 软件菜单「虚拟机」--> 「安装 VMware Tools」；

2、弹出对话框，点击是；

3、此时，会发现虚拟机设备下多了 VMware Tools 这一项，点击它，其里面有一个 `VMwareTools-10.1.15-6627299.tar` 文件：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-55778972.jpg)

4、然后把 `VMwareTools-10.1.15-6627299.tar` 文件提取到某个目录下，比如桌面新建的 myfile文件夹下；

5、提取完成后会发现桌面的 myfile 里面多了一个 vmware-tools-distrib 文件夹；

6、使用快捷键 Ctrl+Alt+T 打开终端，然后切换到 vmware-tools-distrib 该文件夹：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-16749489.jpg)

7、然后输入命令：`sudo ./vmware-install.pl -d`，开始安装 VMware Tools了，根据其提示输入 yes/no，直到出现 Enjoy，表示安装成功。

> 注：`-d` 假定希望接受默认设置。如果不使用 `-d`，请按提示接受默认值或提供您自己的答案。

8、重启虚拟机后会发现菜单栏 ：虚拟机 --> 安装 VMware Tools 变成了“重新安装”字眼，这也表明 VMware Tools已经安装成功了。

9、成功安装完 VMware Tools 后，虚拟机与主机可以通过“拖拽”来互传文件了。

### 3.2 VNC远程控制

改节主要讲解如何使用 VNC 实现 Windows 远程控制 Ubuntu 16.04 桌面。

#### 3.2.1 VNC简介

1、 什么是 VNC？

VNC (Virtual Network Console)，即虚拟网络控制台，它是一款基于 Unix 和 Linux 操作系统的优秀远程控制工具软件，由著名的 AT&T 的欧洲研究实验室开发，远程控制能力强大，高效实用，并且免费开源。

VNC 基本上是由两部分组成：一部分是客户端的应用程序（vncviewer）；另外一部分是服务器端的应用程序（vncserver）。

在任何安装了客户端的应用程序（vncviewer）的计算机都能十分方便地与安装了服务器端的应用程序（vncserver）的计算机相互连接。

2、VNC运行的工作流程

1. VNC 客户端通过浏览器或 VNC Viewer 连接至 VNC Server。
2. VNC Server 传送一对话窗口至客户端，要求输入连接密码，以及存取的 VNC Server 显示装置。
3. 在客户端输入联机密码后，VNC Server 验证客户端是否具有存取权限。
4. 若是客户端通过 VNC Server 的验证，客户端即要求 VNC Server 显示桌面环境。
5. VNC Server 通过 X Protocol 要求 X Server 将画面显示控制权交由 VNC Server 负责。
6. VNC Server 将来由 X Server 的桌面环境利用 VNC 通信协议送至客户端，并且允许客户端控制 VNC Server 的桌面环境及输入装置。

#### 3.2.2 安装

1、首先设置 Ubuntu 允许进行远程控制。打开搜索，直接搜索「桌面共享」程序，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-164579.jpg)

打开之后，将【允许其他人查看您的桌面】这一项勾上，然后在安全那项，勾选【要求远程用户输入此密码】，并设置远程密码。并且取消勾选【必须为对本机器的每次访问进行确定】（这样做，是为了被远程的时候不需要再确认，否则每次远程都要人为确认才能被远程，会很繁琐）如图所示：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-22175592.jpg)

2、Ctrl+Alt+T 快捷键打开终端，安装 vncserver 的基础服务，输入命令：`sudo apt-get install xrdp vnc4server xbase-clients`，因本人我已经安装过了，所以出现如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-20778877.jpg)

3、然后我们需要取消掉请求加密的功能，否则缺少这一步是无法远程上的，这个时候我们需要安装 dconf-editor 工具进行配置，输入以下命令：`sudo apt-get install dconf-editor`；

4、安装完成之后，我们需要打开 dconf-editor 工具，在桌面搜索 dconf-editor 打开，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-59330676.jpg)

打开，然后依次展开 `org->gnome->desktop->remote-access`，然后取消 “requlre-encryption”的勾选即可。如图所示：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-27148039.jpg)

到此，前期准备工作已经完成，后面直接通过 VNC 工具或者 Windows 自带的 mstsc（远程桌面控制）进行访问就行。

#### 3.2.3 远程控制

获取当前的 IP 地址，在 ubuntu 终端下输入：`ifconfig` 即可得到，本人虚拟机下的 ubuntu 的 IP 地址为：`192.168.109.128`

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-64377565.jpg)

**注：在远程访问之前最好先相互之间 ping 下 ip 地址，看是否网络连通。** 如上地址为 局域网 ip 地址，外网就 ping 不通了，继而不能远程控制了。

**方法一：通过 VNC Viewer 客户端进行访问**

到 VNC 官网下载最新的版本，根据自己实际情况，选择相对应的版本，下载地址：[直达链接](https://www.realvnc.com/en/connect/download/viewer/)。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-97948451.jpg)

然后就可以成功连接上。

**方法二：通过 Windows 自带远程桌面控制(mstsc)进行访问（推荐）**

比较推荐使用该方法，直接使用 Windows 自带的远程桌面控制工具进行远程访问，这样就可以不用再下载 VNC Viewer 客户端，直接打开自带远程桌面控制。

Windows 下按 Win 键，然后搜索「远程桌面连接」，打开程序，输入 ubuntu 的 IP 地址，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-65463306.jpg)



点击连接后，选择模式【vnc-any】，然后输入ip 地址和密码进行登录（其中端口号默认为5900，保持不变就行）如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-66262278.jpg)

点击 OK 就可以顺利连接了，如图（下图为缩小图）：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-13-53830959.jpg)

