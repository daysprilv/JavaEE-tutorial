
## 一、依赖管理

### 1.1 传递依赖

1、 什么是传递依赖

当 A 依赖 B、B 依赖C，在 A 中导入 B 后会自动导入 C，C 是 A 的传递依赖，如果 C 依赖 D 则 D 也可能是 A 的传递依赖。

2、依赖范围对传递依赖的影响

依赖会有依赖范围，依赖范围对传递依赖也有影响，有 A、B、C，A 依赖 B、B 依赖 C，C 可能是 A 的传递依赖，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-1-62745972.jpg)

最左边一列为直接依赖，理解为 A 依赖 B 的范围，最顶层一行为传递依赖，理解为 B 依赖 C 的范围，行与列的交叉即为 A 传递依赖 C 的范围。

举例：比如 A 对 B 有 compile 依赖，B 对 C 有 runtime 依赖，那么根据表格所示 A 对 C 有 runtime 依赖。

测试：Dao 层依赖 junit 包，scop 为 test；Service 层依赖 Dao 层。

查看下图红色框内所示传递依赖范围：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-1-73421110.jpg)

所以 maven-first 所依赖的 junit 的 jar 没有加入到 maven-web 工程。如果修改 maven-first 依赖 junit 的 scop 为 compile，maven-first 所依赖的 junit 的 jar 包会加入到 maven-web 工程中，符合上边表格所示，查看下图红色框内所示：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-1-43125516.jpg)

### 1.2 依赖版本冲突解决

#### 1.2.1 问题

当一个项目依赖的构件比较多时，它们相互之前存在依赖，当你需要对依赖版本统一管理时如果让 maven 自动来处理可能并不能如你所愿，如下例子：

同时加入以下依赖，观察依赖：

``` xml
<!-- struts2-spring-plugin依赖spirng-beans-3.0.5 -->
<dependency>
    <groupId>org.apache.struts</groupId>
    <artifactId>struts2-spring-plugin</artifactId>
    <version>2.3.24</version>
</dependency>

<!-- spring-context依赖spring-beans-4.2.4 -->

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.2.4.RELEASE</version>
</dependency>
```

org.apache.struts 依赖 spirng-beans-3.0.5，spring-context 依赖 spring-beans-4.2.4，但是发现 spirng-beans-3.0.5 加入到工程中，而我们希望 spring-beans-4.2.4 加入工程。

#### 1.2.2 依赖调解原则

maven 自动按照下边的原则调解：

**1、第一声明者优秀原则**

在 pom.xml 文件定义依赖，先声明的依赖为准。

测试：如果将上边 struts-spring-plugins 和 spring-context 顺序颠倒，系统将导入 spring-beans-4.2.4。

> 分析：由于 spring-context 在前面，以 spring-context 依赖的 spring-beans-4.2.4 为准，所以最终 spring-beans-4.2.4 添加到了工程中。

**2、路径近者优先原则**

例如：A 依赖 spirng-beans-4.2.4，A 依赖 B 依赖 spirng-beans-3.0.5，则 spring-beans-4.2.4 优先被依赖在 A 中，因为 spring-beans-4.2.4 相对 spirng-beans-3.0.5 被 A 依赖的路径最近。

测试：在本工程中的 pom.xml 中加入 spirng-beans-4.2.4 的依赖，根据路径近者优先原则，系统将导入spirng-beans-4.2.4：

``` xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>4.2.4.RELEASE</version>
</dependency>
```

#### 1.2.3 排除依赖

上边的问题也可以通过排除依赖方法辅助依赖调解，如下：比如在依赖 struts2-spring-plugin 的设置中添加排除依赖，排除 spring-beans，下边的配置表示：依赖 struts2-spring-plugin，但排除 struts2-spring-plugin 所依赖的 spring-beans。

``` xml
<!-- struts2-spring-plugin依赖spirng-beans-3.0.5 -->
<dependency>
    <groupId>org.apache.struts</groupId>
    <artifactId>struts2-spring-plugin</artifactId>
    <version>2.3.24</version>
    <!-- 排除 spring-beans-->
    <exclusions>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

#### 1.2.4 锁定版本

对众多的依赖，有一种方法不用考虑依赖路径、声明优化等因素可以采用直接锁定版本的方法确定依赖构件的版本，版本锁定后则不考虑依赖的声明顺序或依赖的路径，以锁定的版本的为准添加到工程中，此方法在企业开发中常用。

如下的配置是锁定了 spring-beans 和 spring-context 的版本：

``` xml
<dependencyManagement>
    <dependencies>
        <!--这里锁定版本为4.2.4 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

注意：在工程中锁定依赖的版本并不代表在工程中添加了依赖，如果工程需要添加锁定版本的依赖则需要单独添加`<dependencies></dependencies>`标签，如下：

``` xml
<dependencies>
    <!--这里是添加依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
</dependencies>
```

上边添加的依赖并没有指定版本，原因是已在`<dependencyManagement>`中锁定了版本，所以在`<dependency>`下不需要再指定版本。



## 二、maven构建ssh工程

需求：在 web 工程的基础上实现 ssh 工程构建，规范依赖管理。

创建数据库：maven，导入 sql/cst_customer.sql 创建表：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-1-56924845.jpg)

定义 pom.xml：maven 工程首先要识别依赖，web 工程实现 SSH 整合，需要依赖 struts2.3.24、 spring4.2.4、hibernate5.0.7 等，在 pom.xml 添加工程如下依赖。

分两步：

1）锁定依赖版本

2）添加依赖

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>cn.itcast.maven</groupId>
    <artifactId>maven-web-0120</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>war</packaging>
    <name>web工程，包括jsp、action等</name>
    <description>web工程，包括jsp、action等</description>

    <!-- 属性 -->
    <properties>
        <spring.version>4.2.4.RELEASE</spring.version>
        <hibernate.version>5.0.7.Final</hibernate.version>
        <struts.version>2.3.24</struts.version>
    </properties>

    <!-- 锁定版本，struts2-2.3.24、spring4.2.4、hibernate5.0.7 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-context</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aspects</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-orm</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>${spring.version}</version>
            </dependency>
            <dependency>
                <groupId>org.hibernate</groupId>
                <artifactId>hibernate-core</artifactId>
                <version>${hibernate.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.struts</groupId>
                <artifactId>struts2-core</artifactId>
                <version>${struts.version}</version>
            </dependency>
            <dependency>
                <groupId>org.apache.struts</groupId>
                <artifactId>struts2-spring-plugin</artifactId>
                <version>${struts.version}</version>
            </dependency>

        </dependencies>
    </dependencyManagement>

    <!-- 依赖管理 -->
    <dependencies>
        <!-- spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
        </dependency>
        <!-- hibernate -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
        </dependency>

        <!-- 数据库驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
            <scope>runtime</scope>
        </dependency>
        <!-- c3p0 -->
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>
        <!-- 导入 struts2 -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-spring-plugin</artifactId>
        </dependency>

        <!-- servlet jsp -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
            <scope>provided</scope>
        </dependency>
        <!-- 日志 -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.2</version>
        </dependency>
        <!-- junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.9</version>
            <scope>test</scope>
        </dependency>
        <!-- jstl -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- 设置编译版本为1.7 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>

            <!-- maven内置 的tomcat6插件 -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>tomcat-maven-plugin</artifactId>
                <version>1.1</version>
                <configuration>
                    <!-- 可以灵活配置工程路径 -->
                    <path>/ssh</path>
                    <!-- 可以灵活配置端口号 -->
                    <port>8080</port>
                </configuration>
            </plugin>

        </plugins>
    </build>
</project>
```

接下来就是 Dao、Service、Web 层以及页面的编写，此文略。



## 三、maven工程的拆分与聚合（重点）

#### 3.1 需求

一个完整的早期开发好的 CRM 项目，现在要使用 maven 工程对它进行拆分，这时候就可以将 dao 拆解出来形成表现独立的工程，同样 service、action 也都这样拆分。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-87501984.jpg)

工程拆分之后，将来还要聚合（聚合就是将拆分的工程进一步组合在一起，又形成一个完整的项目）。

> 继承：创建一个 parent 工程将通用的 pom.xml 配置抽取出来。
>
> 聚合：聚合多个模块运行。

为了达到聚合的目标，所以会引入父工程（maven project）、子模块(maven module)  dao、service、web。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-95059977.jpg)

#### 3.2 理解继承和聚合

通常继承和聚合同时使用。

1、何为继承？

继承是为了消除重复，如果将 dao、service、web 分开创建独立的工程则每个工程的 pom.xml 文件中的内容存在重复，比如：设置编译版本、锁定 spring 的版本的等，可以将这些重复的配置提取出来在父工程的 pom.xml 中定义。 

2、何为聚合？

项目开发通常是分组分模块开发，每个模块开发完成要运行整个工程需要将每个模块聚合在一起运行，比如： dao、service、web 三个工程最终会打一个独立的 war 运行。

#### 3.3 开发步骤

（1）创建一个 maven 工程

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-25261136.jpg)

点下一步：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-77863215.jpg)

创建后的父工程如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-42486746.jpg)

从它的目录结构可以看出，父工程本身不写代码，它里面有一个 pom.xml 文件，这个文件可以将多个子模块中通用的 jar 所对应的坐标，集中在父工程中配置，将来的子模块就可以不需要在 pom.xml 中配置通用 jar 的坐标了。

（2）如何创建这个父工程的一个子模块？

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-97827164.jpg)

点 Next 进入如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-47678026.jpg)

点 Next 进入如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-37849023.jpg)

（3）再次查看 pom.xml 文件

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-49442738.jpg)

（4）查看子模块的 pom.xml，发现多了一个 parent 结点

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-82818913.jpg)

并且内部所包含的结点，其实就是父工程的坐标。

坐标=groupId（组织名）+artifactId（项目名）+version（版本）。

#### 3.4 冲突问题的解决

**1、通过添加`<exclusion>`标签来解决冲突**

在父工程中引入了 struts-core、hibernate-core，就发现 jar 包是有冲突的，Javassist 存在版本上冲突问题。

解决：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-47041897.jpg)

进入下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-33791165.jpg)

> 背后的父工程的 pom.xml 文件中，添加的内容：
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-69554694.jpg)

**2、依赖调解原则**

maven 自动按照下边的原则调解。

1. 第一声明者优先原则

   > 在 pom.xml 文件定义依赖，先声明的依赖为准。

2. 路径近者优先原则

   > 例如：A 依赖 spirng-beans-4.2.4，A 依赖 B 依赖 spirng-beans-3.0.5，则 spring-beans-4.2.4 优先被依赖在 A 中，因为 spring-beans-4.2.4 相对 spirng-beans-3.0.5 被 A 依赖的路径最近。

**3、使用版本锁定解决冲突**

首先父工程中 pom.xml 文件添加：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-3118232.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-19271085.jpg)

在使用坐标时，对于同一个框架，引入多次时，它的版本信息就会多次出现，所以可以借用常量的思想，将这些版本号提取出来，在需要用到的时候，直接写版本的常量名称就可以了。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-5315308.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-965539.jpg)

#### 3.5 编写 Service 模块

1）创建一个 maven module 项目

创建结束后，父工程中结构如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-49561218.jpg)

父工程的 pom.xml 文件如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-13809142.jpg)

2）在 service 的 pom.xml 文件中引入 dao 的 jar 包（**即把 dao 作为 jar 包**）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-55356737.jpg)

Web 层的子模块创建：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-99067253.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-73135393.jpg)

#### 3.6 运行调试

方法1：在 maven-web 工程的 pom.xml 中配置 tomcat 插件运行

运行 maven-web 工程它会从本地仓库下载依赖的 jar 包，所以当 maven-web 依赖的 jar 包内容修改了必须及时发布到本地仓库，比如：maven-web 依赖的 maven-service 修改了，需要及时将 maven-service 发布到本地仓库。

方法2：在父工程的 pom.xml 中配置 tomcat 插件运行，自动聚合并执行

推荐方法2，如果子工程都在本地，采用方法2则不需要子工程修改就立即发布到本地仓库，父工程会自动聚合[并]()使用最新代码执行。

 注意：如果子工程和父工程中都配置了 tomcat 插件，运行的端口和路径以子工程为准。



## 四、maven私服

#### 4.1 需求

问题：

> 项目组编写了一个通用的工具类，其它项目组将类拷贝过去使用，当工具类修改 bug 后通过邮件发送给各各项目组，这种分发机制不规范可能导致工具类版本不统一。
>
> 解决方案：项目组将写的工具类通过 maven 构建，打成 jar，将 jar 包发布到公司的 maven 仓库中，公司其它项目通过 maven 依赖管理从仓库自动下载 jar 包。

分析：

> 公司在自己的局域网内搭建自己的远程仓库服务器，称为私服，私服服务器即是公司内部的 maven 远程仓库，每个员工的电脑上安装 maven 软件并且连接私服服务器，员工将自己开发的项目打成 jar 并发布到私服服务器，其它项目组从私服服务器下载所依赖的构件（jar）。
>
> 私服还充当一个代理服务器，当私服上没有 jar 包会从互联网中央仓库自动下载，如下图：
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-87728389.jpg)

#### 4.2 私服搭建

1、下载 nexus

Nexus 是 maven 仓库管理器，通过 nexus 可以搭建 maven 仓库，同时 nexus 还提供强大的仓库管理功能，构件搜索功能等。下载 Nexus，下载地址：http://www.sonatype.org/nexus/archived/

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-45753612.jpg)

2、安装 nexus

1. 解压，进入指定的目录

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-33492103.jpg)

2. 安装并启动这个应用程序

   cmd 进入 bin 目录，执行 `nexus.bat install`：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-12891272.jpg)

   安装成功在服务中查看有 nexus 服务：

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-4374339.jpg)

3、卸载

cmd 进入nexus 的 bin 目录，执行：`nexus.bat uninstall`

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-5227792.jpg)

查看 window 服务列表 nexus 已被删除。

4、启动  nexus

方法1：cmd 进入 bin 目录，执行 `nexus.bat start`

方法2：直接启动 nexus 服务

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-46881617.jpg)

查看 nexus 的配置文件 conf/nexus.properties：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-81845434.jpg)

``` xml
# Jetty section
application-port=8081  	# nexus的访问端口配置
application-host=0.0.0.0 	# nexus主机监听配置(不用修改)
nexus-webapp=${bundleBasedir}/nexus 	# nexus工程目录
nexus-webapp-context-path=/nexus	 # nexus的web访问路径

# Nexus section
nexus-work=${bundleBasedir}/../sonatype-work/nexus   # nexus仓库目录
runtime=${bundleBasedir}/nexus/WEB-INF  # nexus运行程序目录
```

访问：http://localhost:8081/nexus/，使用 nexus 内置账户名 admin，密码 admin123 进行登陆，登陆成功：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-11559037.jpg)

nexus 的仓库有 4 种类型：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-78429484.jpg)

> 1）hosted，宿主仓库，部署自己的 jar 到这个类型的仓库，包括 releases 和 snapshot 两部分，Releases 公司内部发布版本仓库、 Snapshots 公司内部测试版本仓库
>
> 2）proxy，代理仓库，用于代理远程的公共仓库，如 maven 中央仓库，用户连接私服，私服自动去中央仓库下载 jar 包或者插件。 
>
> 3）group，仓库组，用来合并多个 hosted/proxy 仓库，通常我们配置自己的 maven 连接仓库组。
>
> 4）virtual(虚拟)：兼容 Maven1 版本的 jar 或者插件 

nexus 仓库默认在 sonatype-work 目录中：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-90944452.jpg)

- central：代理仓库，代理中央仓库

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-79513157.jpg)



- apache-snapshots：代理仓库，存储 snapshots 构件，代理地址 https://repository.apache.org/snapshots/
- central-m1：virtual 类型仓库，兼容 Maven1 版本的 jar 或者插件
- releases：本地仓库，存储 releases 构件
- snapshots：本地仓库，存储 snapshots 构件
- thirdparty：第三方仓库
- public：仓库组

#### 4.3 将项目发布到私服

企业中多个团队协作开发通常会将一些公用的组件、开发模块等发布到私服供其它团队或模块开发人员使用。

本例子假设多团队分别开发 dao、service、web，某个团队开发完在 dao 会将 dao 发布到私服供 service 团队使用，本例子会将 dao 工程打成 jar 包发布到私服。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-23576274.jpg)

**配置过程：**

1）第一步： 需要在客户端即部署 dao 工程的电脑上配置 maven 环境，并修改 settings.xml 文件，配置连接私服的用户和密码 。

此用户名和密码用于私服校验，因为私服需要知道上传的账号和密码，是否和私服中的账号和密码一致。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-68382943.jpg)

- releases 连接发布版本项目仓库
- snapshots 连接测试版本项目仓库 

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-76042979.jpg)

2）第二步： 配置项目 pom.xml

配置私服仓库的地址，本公司的自己的 jar 包会上传到私服的宿主仓库，根据工程的版本号决定上传到哪个宿主仓库，如果版本为 release 则上传到私服的 release 仓库，如果版本为 snapshot 则上传到私服的 snapshot 仓库

``` xml
<distributionManagement>
    <repository>
        <id>releases</id>
        <url>http://localhost:8081/nexus/content/repositories/releases/</url>
    </repository> 
    <snapshotRepository>
        <id>snapshots</id>
        <url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
    </snapshotRepository> 
</distributionManagement>
```

注意：pom.xml 这里 \<id> 和 settings.xml 配置 \<id> 对应！

**测试：**

将项目 dao 工程打成 jar 包发布到私服：

1、首先启动 nexus

2、对 dao 工程执行 deploy 命令

根据本项目 pom.xml 中 version 定义决定发布到哪个仓库，如果 version 定义为 snapshot，执行 deploy 后查看nexus 的 snapshot 仓库，如果 version 定义为 release 则项目将发布到 nexus 的 release 仓库，本项目将发布到 snapshot 仓库：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-19212546.jpg)

也可以通过 http 方式查看：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-41224669.jpg)

#### 4.4 从私服下载jar包

1）需求

没有配置 nexus 之前，如果本地仓库没有，去中央仓库下载，通常在企业中会在局域网内部署一台私服服务器，有了私服本地项目首先去本地仓库找 jar，如果没有找到则连接私服从私服下载 jar 包，如果私服没有 jar 包私服同时作为代理服务器从中央仓库下载 jar 包，这样做的好处是一方面由私服对公司项目的依赖 jar 包统一管理，一方面提高下载速度，项目连接私服下载 jar 包的速度要比项目连接中央仓库的速度快的多。

2）管理仓库组

nexus 中包括很多仓库，hosted 中存放的是企业自己发布的 jar 包及第三方公司的 jar 包，proxy 中存放的是中央仓库的 jar，为了方便从私服下载 jar 包可以将多个仓库组成一个仓库组，每个工程需要连接私服的仓库组下载 jar 包。

打开 nexus 配置仓库组，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-93906847.jpg)

上图中仓库组包括了本地仓库、代理仓库等。

3）在 setting.xml 中配置仓库

在客户端的 setting.xml 中配置私服的仓库，由于 setting.xml 中没有 repositories 的配置标签需要使用 profile 定义仓库。

``` xml
<profile>   
    <!--profile的id-->
    <id>dev</id>   
    <repositories>   
        <repository>  
            <!--仓库id，repositories可以配置多个仓库，保证id不重复-->
            <id>nexus</id>   
            <!--仓库地址，即nexus仓库组的地址-->
            <url>http://localhost:8081/nexus/content/groups/public/</url>   
            <!--是否下载releases构件-->
            <releases>   
                <enabled>true</enabled>   
            </releases>   
            <!--是否下载snapshots构件-->
            <snapshots>   
                <enabled>true</enabled>   
            </snapshots>   
        </repository>   
    </repositories>  
    <pluginRepositories>  
        <!-- 插件仓库，maven的运行依赖插件，也需要从私服下载插件 -->
        <pluginRepository>  
            <!-- 插件仓库的id不允许重复，如果重复后边配置会覆盖前边 -->
            <id>public</id>  
            <name>Public Repositories</name>  
            <url>http://localhost:8081/nexus/content/groups/public/</url>  
        </pluginRepository>  
    </pluginRepositories>  
</profile>  
```

使用 profile 定义仓库需要激活才可生效。

``` xml
<activeProfiles>
    <activeProfile>dev</activeProfile>
</activeProfiles>
```

配置成功后通过 eclipse 查看有效 pom，有效 pom 是 maven 软件最终使用的 pom 内容，程序员不直接编辑有效 pom，打开有效 pom：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-9-2-93212905.jpg)

有效 pom 内容如下：

> 下边的 pom 内容中有两个仓库地址，maven 会先从前边的仓库的找，如果找不到 jar 包再从下边的找，从而就实现了从私服下载 jar 包。

``` xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <id>public</id>
        <name>Public Repositories</name>
        <url>http://localhost:8081/nexus/content/groups/public/</url>
    </repository>
    <repository>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
        <id>central</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
</repositories>
<pluginRepositories>
    <pluginRepository>
        <id>public</id>
        <name>Public Repositories</name>
        <url>http://localhost:8081/nexus/content/groups/public/</url>
    </pluginRepository>
    <pluginRepository>
        <releases>
            <updatePolicy>never</updatePolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
        <id>central</id>
        <name>Central Repository</name>
        <url>https://repo.maven.apache.org/maven2</url>
    </pluginRepository>
</pluginRepositories>
```

4）测试从私服下载 jar 包

☛ 测试1：局域网环境或本地网络即可

在 service 工程中添加以上配置后，添加 dao 工程的依赖，删除本地仓库中 dao 工程，同时在 eclipse 中关闭 dao 工程。观察控制台。

项目先从本地仓库找 dao，找不到从私服找，由于之前执行 deploy 将 dao 部署到私服中，所以成功从私服下载 dao 并在本地仓库保存一份。

如果此时删除私服中的 dao，执行 update project 之后是否正常？

如果将本地仓库的 dao 和私服的 dao 全部删除是否正常？

☛ 测试2：需要互联网环境

在项目的 pom.xml 添加一个依赖，此依赖在本地仓库和私服都不存在，maven 会先从本地仓库找，本地仓库没有再从私服找，私服没有再去中央仓库下载，jar 包下载成功在私服、本地仓库分别存储一份。