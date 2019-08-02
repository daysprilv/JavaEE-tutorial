
## 一、前言

### 1.1 大型互联网架构演变历程

#### 1.1.1 淘宝技术

淘宝的核心技术（国内乃至国际的 Top，这还是2011年的数据）

- 拥有全国最大的分布式 Hadoop 集群（云梯，2000左右节点，24000核 CPU，48000GB 内存，40PB 存储容量）
- 全国分布 80+CDN 节点，能够自动找寻最近的节点提供服务，支持流量超过800Gbps
- 不逊于百度的搜索引擎，对数十亿商品进行搜索，全球最大的电商平台
- 顶尖的负载均衡系统，顶尖的分布式系统，顶尖的互联网思想，功能多样运行极其稳定
- 丰富的生态产业以及先进的数据挖掘技术
- ……很多很多

#### 1.1.2 淘宝技术演变

摘自《淘宝技术这十年》

> - 马总在2003年4月7日秘密叫来阿里巴巴的十位员工，来到杭州一个隐秘的毛坯房，要求他们在一个月左右的时间内做出一个 C2C 网站。结果当然还是直接买的快，一个基于 LAMP 架构的网站，原名是PHPAuction，老美开发的一个拍卖网站。当然必须要做修改才能用。
> - 2003年底，淘宝注册用户23万，PV 31万/day，半年成交额3371万。
> - 很显然 MySQL 无法撑得起如此大的访问量，数据库瓶颈出现了。幸好阿里的 DBA 队伍足够强大，他们使用 Oracle 替代了 MySQL。Oracle 那时就已经有了强大的并发性访问设计——连接池，从连接池取连接的耗费比单独建立连接少很多。但是 PHP 当时并没有官方提供支持语言连接池特性，于是多隆前辈用Google（不会是Baidu）搜到了一个开源的 SQL Relay，于是数据库软件方面的瓶颈暂时解决了。
> - 随之而来的是面临硬件性能瓶颈，阿里买了 EMC 的 SAN 存储设备，加上 Oracle 高性能 RAC，硬件容量也暂时没问题了。
> - 因为 SQL Relay 的问题实在过于严重，2004年于是淘宝终于做出了跨时代的决策——使用Java重写网站。
> - 淘宝请了 Sun 的高级工程师来帮忙做 Java 架构。那么他们是如何做到修改编程语言而不改变网站使用呢——模块化替换，今天写好了 A 模块，另开一个新域名，将连接指向该模块，同时别的模块不变，等到全部模块完成的时候，原域名放弃。Sun 公司坚持使用 EJB 作为控制层，加上使用 iBatis 作为持久层，一个可扩展且高效的 Java EE 应用诞生了。
> - 送走 Sun 的大牛们之后，阿里的数据存储又遇到了瓶颈，于是忍痛买了一台 IBM 小型机，也就有了IOE（IBM + Oracle + EMC）这样的传说。
> - 2004年底，淘宝注册用户400万，PV 4000万/day，全网成交额10个亿。
> - 2005年 Spring 诞生了，早闻 Spring 框架在 Web 应用不可或缺，而在淘宝网，Spring 也达到了 Rod Johnson 设计它的目的——替代 EJB。
> - 2005年底，淘宝注册用户1390万，PV 8931万/day，商品数目1663万个。
> - 考虑到未来的发展，这样的设施架构只是勉强可以应付现在的要求。于是，CDN 技术派上用场了，一开始使用商用的 ChinaCache，后来使用章文嵩博士搭建低耗能 CDN 网络，淘宝网的性能越来越好了。
> - 2006年底，淘宝注册用户3000万，PV 15000万/day，商品数目5000万，全网成交额169亿元。
> - 淘宝在2007年之前，使用 NetApp 的商用存储系统，但是仍然不够应付迅速增长的趋势。同年 Google 公布了 GFS 的设计思想，参照它的思想，淘宝也开发了自己的文件系统——TFS 每个用户在 TFS 上拥有1GB的图片存储空间，这些都得益于 TFS 集群的文件存储系统以及大量的图片服务器。淘宝使用实时生成缩率图，全局负载均衡以及一级和二级缓存来保证图片的访问优化与高效访问。
> - 淘宝的服务器软件使用 Tengine，一个被优化过的 nginx 模块。
> - 淘宝分离出了 UIC（User Information Center），供所有模块调用。多隆前辈再次为其编写出了 TDBM，完全是基于内存的数据缓存（参考了memcached）。再然后，淘宝将 TBstore 和 TDBM 合并，写出了 Tair，一个基于 Key-Value 的分布式缓存数据系统。然后升级了自己的 iSearch 系统。
> - 2007年底，淘宝注册用户5000万，PV 25000万/day，商品数目1个亿，全网成交额433亿元。
> - ......
> - Dubbo 是阿里巴巴内部的 SOA 服务化治理方案的核心框架，每天为2000+ 个服务提供3000000000+ 次访问量支持，并被广泛应用于阿里巴巴集团的各成员站点。Dubbo 自2011年开源后，已被许多非阿里系公司使用。

#### 1.2.3 技术发展总结

1、单节点架构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-36179227.jpg)

2、集群架构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-14144335.jpg)

3、集群+分布式架构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-40748000.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-25056529.jpg)

### 1.2 代理概述

#### 1.2.1 正向代理(Forward Proxy)

一般情况下，如果没有特别说明，代理技术默认说的是正向代理技术。关于正向代理的概念如下：

正向代理(forward)是一个位于客户端【用户A】和原始服务器(origin server)【服务器B】之间的服务器【代理服务器Z】，为了从原始服务器取得内容，用户 A 向代理服务器 Z 发送一个请求并指定目标（服务器B），然后代理服务器 Z 向服务器 B 转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-87699555.jpg)

从上面的概念中，可以看出，所谓的**正向代理**就是**代理服务器替代访问方【用户A】去访问目标服务器【服务器B】。**

这就是正向代理的意义所在。而为什么要用代理服务器去代替访问方【用户A】去访问服务器 B 呢？这就要从代理服务器使用的意义说起。

使用正向代理服务器作用主要有以下几点：

1. 访问本无法访问的服务器 B，如图

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-18019077.jpg)

   我们抛除复杂的网络路由情节来看上图，假设图中路由器从左到右命名为 R1、R2。假设最初用户 A 要访问服务器 B 需要经过 R1 和 R2 路由器这样一个路由节点，如果路由器 R1 或者路由器 R2 发生故障，那么就无法访问服务器 B 了。但是如果用户 A 让代理服务器 Z 去代替自己访问服务器 B，由于代理服务器 Z 没有在路由器 R1 或 R2 节点中，而是通过其它的路由节点访问服务器 B，那么用户 A 就可以得到服务器 B 的数据了。现实中的例子就是 FQ。不过自从 VPN 技术被广泛应用外，FQ 不但使用了传统的正向代理技术，有的还使用了VPN 技术。

2. 加速访问服务器 B

   这种说法目前不像以前那么流行了，主要是带宽流量的飞速发展。早期的正向代理中，很多人使用正向代理就是提速。还是如上图，假设用户 A 到服务器 B，经过 R1 路由器和 R2 路由器，而 R1 到 R2 路由器的链路是一个低带宽链路。而用户 A 到代理服务器 Z，从代理服务器 Z 到服务器 B 都是高带宽链路。那么很显然就可以加速访问服务器 B 了。

3. Cache 作用

   Cache（缓存）技术和代理服务技术是紧密联系的（不光是正向代理，反向代理也使用了Cache（缓存）技术。还如上图所示，如果在用户 A 访问服务器 B 某数据 D 之前，已经有人通过代理服务器 Z 访问过服务器 B 上得数据 D，那么代理服务器 Z 会把数据 D 保存一段时间，如果有人正好取该数据 D，那么代理服务器 Z 不再访问服务器 B，而把缓存的数据 D 直接发给用户 A。这一技术在 Cache 中术语就叫 Cache 命中。如果有更多的像用户 A 的用户来访问代理服务器 Z，那么这些用户都可以直接从代理服务器 Z 中取得数据 D，而不用千里迢迢的去服务器 B 下载数据了。

4. 客户端访问授权

   这方面的内容现今使用的还是比较多的，例如一些公司采用 ISA SERVER 做为正向代理服务器来授权用户是否有权限访问互联网，如下图

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-18239409.jpg)

   用防火墙作为网关，用来过滤外网对其的访问。假设用户 A 和用户 B 都设置了代理服务器，用户 A 允许访问互联网，而用户 B 不允许访问互联网（这个在代理服务器 Z 上做限制）这样用户 A 因为授权，可以通过代理服务器访问到服务器 B，而用户 B 因为没有被代理服务器 Z 授权，所以访问服务器 B 时，数据包会被直接丢 弃。

5. 隐藏访问者的行踪

   如下图，我们可以看出服务器 B 并不知道访问自己的实际是用户 A，因为代理服务器 Z 代替用户 A 去直接与服务器 B 进行交互。如果代理服务器 Z 被用户 A 完全控制（或不完全控制），会惯以「肉鸡」术语称呼。 

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-48918035.jpg)


总结一下：正向代理是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标（原始服务器），然后代理向原始服务器转交请求并将获得的内 容返回给客户端。客户端必须设置正向代理服务器，当然前提是要知道正向代理服务器的 IP 地址，还有代理程序的端口。

#### 1.2.2 反向代理(Reverse Proxy)

反向代理正好与正向代理相反，对于客户端而言代理服务器就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端。 使用反向代理服务器的作用如下：

1. 保护和隐藏原始资源服务器，如下图：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-35788242.jpg)

   用户 A 始终认为它访问的是原始服务器 B 而不是代理服务器 Z，但实用际上反向代理服务器接受用户 A 的应答，从原始资源服务器 B 中取得用户 A 的需求资源，然后发送给用户 A。由于防火墙的作用，只允许代理服务器 Z 访问原始资源服务器 B。尽管在这个虚拟的环境下，防火墙和反向代理的共同作用保护了原始资源服务器 B，但用户 A 并不知情。

2. 负载均衡，如下图：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-992873.jpg)

   当反向代理服务器不止一个的时候，我们甚至可以把它们做成集群，当更多的用户访问资源服务器 B 的时候，让不同的代理服务器 Z（x）去应答不同的用户，然后发送不同用户需要的资源。

   当然反向代理服务器像正向代理服务器一样拥有 Cache 的作用，它可以缓存原始资源服务器 B 的资源，而不是每次都要向原始资源服务器 B 请求数据，特别是一些静态的数据，比如图片和文件，如果这些反向代理服务器能够做到和用户 X 来自同一个网络，那么用户 X 访问反向代理服务器 X，就会得到很高质量的速度。这正是 CDN 技术的核心。如下图：

    ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-77514732.jpg)

   我们并不是讲解 CDN，所以去掉了 CDN 最关键的核心技术智能 DNS。只是展示 CDN 技术实际上利用的正是反向代理原理这块。

反向代理结论与正向代理正好相反，对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容原本就是它自己的一样。

基本上，网上做正反向代理的程序很多，能做正向代理的软件大部分也可以做反向代理。开源软件中最流行的就是 squid，既可以做正向代理，也有很多人用来做反向代理的前端服务器。另外 MS ISA 也可以用来在 Windows 平台下做正向代理。反向代理中最主要的实践就是 Web 服务，近些年来最火的就是 Nginx 了。网上有人说 Nginx 不能做正向代理，其实是不对的。Nginx 也可以做正向代理，不过用的人比较少了。

#### 1.2.3 透明代理

如果把正向代理、反向代理和透明代理按照人类血缘关系来划分的话。那么正向代理和透明代理是很明显堂亲关系，而正向代理和反向代理就是表亲关系了 。

透明代理的意思是客户端根本不需要知道有代理服务器的存在，它改编你的 request fields（报文），并会传送真实 IP。注意，加密的透明代理则是属于匿名代理，意思是不用设置使用代理了。 透明代理实践的例子就是时下很多公司使用的行为管理软件。如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-54606939.jpg)

用户 A 和用户 B 并不知道行为管理设备充当透明代理行为，当用户 A 或用户 B 向服务器 A 或服务器 B 提交请求的时候，透明代理设备根据自身策略拦截并修改用户 A 或 B 的报文，并作为实际的请求方，向服务器 A 或 B 发送请求，当接收信息回传，透明代理再根据自身的设置把允许的报文发回至用户 A 或 B，如上图，如果透明代理设置不允许访问服务器 B，那么用户 A 或者用户 B 就不会得到服务器 B 的数据。



## 二、在Linux下发布项目

使用的为：CentOS 系统

### 2.1 Linux系统上安装JDK

登录 Linux 系统后，先检测是否安装了 jdk，运行：`java -version`，如果有 OpenJDK，将其卸载，我们安装使用 SunJDK。关于两者相关区别：

- [Linux下的JDK和OpenJDK有什么具体的区别啊？](https://www.oschina.net/question/1037640_119908)
- [OpenJDK和SunJDK有啥区别？](https://www.zhihu.com/question/19646618)
- [OpenJDK和Sun/OracleJDK 区别 与联系](https://www.cnblogs.com/zengkefu/p/5633342.html)

1、卸载 OpenJDK

- 查看和 java 相关的包：`rpm –qa | grep java`   

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-54493905.jpg)

- 卸载 OpenJDK

  - 先卸载 openjdk 1.7：`rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.i686`

  - 再卸载 openjdk 1.6：`rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.i686`

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-72835634.jpg)

2、安装 jdk：（上传 jdk， 通过 ftp 软件上传，上传到 root 目录下）

- 在 /usr/local 创建一个 java 目录：`mkdir java`

- 将上传的 jdk 复制到 java 目录下：`cp /root/jdk.xxxxx.tar /usr/local/java`

- 将其解压：`tar -xvf jdk.xxx.tar`

- 安装依赖：`yum install glibc.i686`

  > 要想运行使用我们 jdk 还需要安装一些插件，就像我们 java 一样，底层依赖一些 C++、C 语言的东西。
  >
  > 关于 yum，菜鸟教程：[linux yum 命令](http://www.runoob.com/linux/linux-yum.html)

- 配置环境变量：

  ``` xml
  编辑  vi /etc/profile
  在文件最后添加一下信息
      #set java environment
      JAVA_HOME=/usr/local/java/jdk1.7.0_72
      CLASSPATH=.:$JAVA_HOME/lib.tools.jar
      PATH=$JAVA_HOME/bin:$PATH
      export JAVA_HOME CLASSPATH PATH
  保存退出
  source /etc/profile  重新加载配置文件，使更改的配置立即生效
  ```

注：关于如何上上传文件到 root 目录，如下，可以使用 FileZilla 软件，直接拖动文件到该目录即可完成上传。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-16929443.jpg)

### 2.2 Linux系统上安装MySQL

1. 检测是否按照了 mysql：`rpm  -qa | grep mysql`

2. 卸载系统自带的mysql：`rpm -e --nodeps mysql-libs-5.1.71-1.el6.i386 `

3. 上传 mysql 包

4. 在 /usr/local/ 创建一个 mysql 目录

5. 复制 mysql 包到 mysql 目录下

6. 解压 tar：`tar -xvf MySQL-5.6.22-1.el6.i386.rpm-bundle.tar`，会有几个 rpm 文件，如下：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-67582150.jpg)

7. 安装

   - 安装 mysql 的服务器端：`rpm -ivh MySQL-server-5.5.49-1.linux2.6.i386.rpm`

     注意：第一次登录 mysql 的时候是不需要密码的，以后都需要。

   - 安装 mysql 的客户端：`rpm -ivh MySQL-client-5.5.49-1.linux2.6.i386.rpm`

8. 查看 mysql 的服务状态：`service mysql status `

   - 启动 mysql：`service mysql start`
   - 停止 mysql：`service mysql stop`

9. 修改 mysql 的 root 的密码

   ``` xml
   登录:mysql -uroot
   修改密码:
       use mysql;
       update user set password = password('1234') where user = 'root';
       flush privileges;# 刷新
   ```

10. 开启远程访问

    ``` xml
    grant all privileges on *.* to 'root' @'%' identified by '1234';
    flush privileges;
    ```

11. 开启防火墙端口 3306 退出 mysql

    ``` xml
    3306端口放行 
    /sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
    将该设置添加到防火墙的规则中
    /etc/rc.d/init.d/iptables save
    ```

    测试连接 mysql 数据库，连接成功：

    ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-17758372.jpg)

12. 设置 mysql 的服务随着系统的启动而启动

    ``` xml
    加入到系统服务：
    chkconfig --add mysql
    自动启动：
    chkconfig mysql on
    ```

### 2.3 Linux系统下安装Tomcat

1. 在 /usr/local/ 创建 tomcat 目录：`mkdir tomcat`

2. 复制 tomcat 到 /usr/local/tomcat：`cp /root/apache-tomcat-7.0.52.tar.gz ./tomcat/`

3. 解压 tomcat：`tar -xvf apache-tomcat-7.0.52.tar.gz`

4. 启动 tomcat，进入 bin：
   - 方式一：`sh startup.sh`

   - 方式二：`./startup.sh`

   注意：

   ```xml
   查看日志文件
   tail -f logs/catalina.out
   退出 ctrl+c
   ```
5. 开启端口号 8080：

   ``` xml
   8080端口放行 
   /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
   将该设置添加到防火墙的规则中
   /etc/rc.d/init.d/iptables save
   ```


### 2.4 发布项目到Linux

1. 数据库和表

   ``` xml
   ①备份store28的数据库：
   在windows下：mysqldump -uroot -p1234 store28 > C:/1.sql

   ②将C盘下的1.sql上传到Linux下的root目录

   ③通过远程工具还原数据库
       先登录mysql
       创建数据库 store28
       进入store28
       	source /root/1.sql
   ```

2. 项目

   ``` xml
   ①将项目打包 war
   	war包的特点:
   		在tomcat/webapps目录下 只要tomcat启动 war会自动解压
   ②将store.war上传到Linux系统下的root目录下
   ③将store.war移动到tomcat/webapps下即可
   ```



## 三、Nginx

### 3.1 nginx相关概念

在介绍 Ngnix 之前，先还是回顾几个与 Ngnix 相关的概念。

1、反向代理

> 反向代理（Reverse Proxy）方式是指以代理服务器来接受 Internet 上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给 Internet 上请求连接的客户端，此时代理服务器对外就表现为一个服务器。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-94237863.jpg)

2、负载均衡

> 负载均衡，英文名称为 Load Balance，是指建立在现有网络结构之上，并提供了一种廉价有效透明的方法扩展网络设备和服务器的带宽、增加吞吐量、加强网络数据处理能力、提高网络的灵活性和可用性。其原理就是数据流量分摊到多个服务器上执行，减轻每台服务器的压力，多台服务器共同完成工作任务，从而提高了数据的吞吐量。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-7715634.jpg)

### 3.2 nginx简介

1、什么是 ngnix？

> Nginx(engine x) 是一个高性能的[HTTP](https://baike.baidu.com/item/HTTP)和[反向代理](https://baike.baidu.com/item/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86/7793488)服务，也是一个IMAP/POP3/SMTP服务。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。
>
> 其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。2011年6月1日，nginx 1.0.4发布。
>
> Nginx是一款轻量级服务器反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘等。（*摘自[百度百科](https://baike.baidu.com/item/nginx)*）

2、为什么使用 ngnix？

> 背景：
>
> 互联网飞速发展的今天，大用户量高并发已经成为互联网的主体。怎样能让一个网站能够承载几万个或几十万个用户的持续访问呢？这是一些中小网站急需解决的问题。用单机 tomcat 搭建的网站，在比较理想的状态下能够承受的并发访问量在 150 到 200 左右。按照并发访问量占总用户数量的 5% 到 10% 这样计算，单点 tomcat 网站的用户人数在 1500 到 4000 左右。对于一个为全国范围提供服务的网站显然是不够用的，为了解决这个问题引入了负载均衡方法。负载均衡就是一个 web 服务器解决不了的问题可以通过多个 web 服务器来平均分担压力来解决，并发过来的请求被平均分配到多个后台 web 服务器来处理，这样压力就被分解开来。
>
> 负载均衡服务器分为两种一种是通过硬件实现的负载均衡服务器，简称硬负载例如：f5。另一种是通过软件来实现的负载均衡，简称软负载，例如 apache 和 nginx。硬负载和软负载相比，前者作用的网络层次比较多，可以作用到 socket 接口的数据链路层对发出的请求进行分组转发但是价格成本比较贵，而软负载作用的层次在 http 协议层之上可以对 http 请求进行分组转发并且因为是开源的所以几乎是 0 成本，并且阿里巴巴，京东等电商网站使用的都是 Nginx 服务器。

很多大网站都是使用 nginx 做反向代理，应用非常广泛。Nginx 是一款高性能的 http 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。由俄罗斯的程序设计师 Igor Sysoev 所开发，官方测试 nginx 能够支支撑5万并发链接，并且 cpu、内存等资源消耗却非常低，运行非常稳定。

应用场景：

1. http 服务器，可以做静态网页的 http 服务器。
2. 配置虚拟机。一个域名可以被多个 ip 绑定。可以根据域名的不同把请求转发给运行在不同端口的服务器。


3. 反向代理，负载均衡。把请求转发给不同的服务器。



## 四、搭建集群

### 4.1 在Windows下搭建集群

1、在 G 盘新建两个新建两个目录 tomcat1、tomcat2

2、修改 tomcat2 的端口，例如， 在以 tomcat1 的端口为基础上+10，修改完，启动这两个 tomcat，再分别访问

- 访问 8080 端口：`localhost:8080/test/`

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-37890319.jpg)

- 访问 8090 端口：`localhost:8090/test/`

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-99815718.jpg)

3、解压 nginx

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-64409573.jpg)

修改 ngnix 的 `nginx.conf` 文件，在 location / 下添加反向代理服务器，代理 8080 端口：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-55561515.jpg)

以上只是代理了一台服务器。启动 nginx，每次访问 `localhost/test/`， 最后访问的都是8080端口服务器的数据。

4、代理集群，例如代理两台服务器

``` xml
需要在http节点上添加一个
upstream servlet_yujia{
    server 127.0.0.1:8080;
    server 127.0.0.1:8090;
}
修改location /下的反向代理 
	proxy_pass http://servlet_yujia
```

修改如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-41643690.jpg)

另外，还可以给服务器添加权重：![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-34977749.jpg)

表示如果有 6 次访问，则有 4 次可能访问到第二个服务器。启动 nginx，则每次访问`localhost/test/`，会有可能访问到8080端口服务器的数据，也可能访问到8090端口服务器的数据。

5、Session共享问题

1. 解决方式一（只能在 windows 下好使）：

   web 服务器解决（广播机制），注意：tomcat 下性能低

   修改两个地方：

   1. 修改两个 tomcat 的 server.xml，支持共享，将引擎标签下的  `<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>`注释去掉

      ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-29411848.jpg)

   2. 修改项目的配置文件 web.xml 中添加一个节点 `<distributable/>`

      ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-10852419.jpg)

   

2. 解决方式二：可以将 session 的 id 放入 redis 中

3. 解决方式三：保证一个 ip 地址永远的访问一台 web 服务器，就不存在 session 共享问题了，在 nginx 的配置文件中 upstream 中添加 ip_hash（**在 Linux 下经常使用该方式**）

   > ip_hash 指令能够将某个客户端 IP 的请求通过哈希算法定位到同一台后端服务器上。

### 4.2 在Linux下搭建集群

1、先将 nginx 上传到 linux 上

2、解压 nginx

3、先编译 nginx

``` xml
安装依赖包
    yum install gcc-c++
    yum install -y pcre pcre-devel
    yum install -y zlib zlib-devel
    yum install -y openssl openssl-devel
执行编译
    先进入 nginx的目录
    执行
		./configure
```

> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-82525183.jpg)

4、安装 nginx

- 执行 `make`
- 执行 `make install`

5、启动 nginx：

``` xml
cd nginx 目录下
	配置文件 conf
	启动nginx 
		./nginx 
```

注：①在 nginx 目录下有一个 sbin 目录，sbin 目录下有一个 nginx 可执行程序 

②关闭 ngnix：

- 关闭命令：相当于找到 nginx 进程kill  `./nginx -s stop`
- 退出命令 `./nginx -s quit`等程序执行完毕后关闭，建议使用此命令。

6、将端口号 80 放行

``` xml
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
将该设置添加到防火墙的规则中
/etc/rc.d/init.d/iptables save
```

7、修改 conf 文件和 window 下一样：配置集群

