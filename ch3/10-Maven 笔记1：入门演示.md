
## 一、Maven介绍

### 1.1 Maven是什么

maven 翻译为“专家”，“内行”。Maven 是 Apache 下的一个纯 Java 开发的开源项目，它是一个项目管理工具，使用 maven 对 java 项目进行构建、依赖管理。当前使用 Maven 的项目在持续增长。

### 1.2 什么是项目构建

项目构建是一个项目从编写源代码到编译、测试、运行、打包、部署、运行的过程。

#### 1.2.1 传统项目构建过程

1、传统的使用 Eclipse 构建项目的过程如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-16191487.jpg)

构建过程如下：

1）在 eclipse 中创建一个 java web工 程

2）在工程中编写源代码及配置文件等

3）对源代码进行编译，java 文件编译成 class 文件

4）执行 Junit 单元测试

5）将工程打成 war 包部署至 tomcat 运行

#### 1.2.2 maven项目构建过程

maven 将项目构建的过程进行标准化，每个阶段使用一个命令完成，下图展示了构建过程的一些阶段：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-17410663.jpg)上图中部分阶段对应命令如下：

- 清理阶段对应 maven 的命令是 clean，清理输出的 class 文件
- 编译阶段对应 maven 的命令是 compile，将 java 代码编译成 class 文件。
- 打包阶段对应 maven 的命令是 package，java 工程可以打成 jar 包，web 包可以打成 war 包

**运行一个 maven工程（web工程）需要一个命令**：`tomat:run`

maven工程构建的优点：

1. 一个命令完成构建、运行，方便快捷。
2. maven 对每个构建阶段进行规范，非常有利于大型团队协作开发。

### 1.3 什么是依赖管理

1、什么是依赖？

一个 java 项目可能要使用一些第三方的 jar 包才可以运行，那么我们说这个 java 项目依赖了这些第三方的 jar 包。举个例子：一个 CRM 系统，它的架构是 SSH 框架，该 CRM 项目依赖 SSH 框架，具体它依赖的 Hibernate、Spring、Struts2。

2、什么是依赖管理？

就是对项目所有依赖的 jar 包进行规范化管理。

#### 1.3.1 传统项目的依赖管理

传统的项目工程要管理所依赖的 jar 包完全靠人工进行，程序员从网上下载 jar 包添加到项目工程中，如下图：程序员手工将 Hibernate、struts2、Spring 的 jar 添加到工程中的 WEB-INF/lib 目录下。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-25445899.jpg)

手工拷贝jar包添加到工程中的问题是：

1. 没有对 jar 包的版本统一管理，容易导致版本冲突。
2. 从网上找 jar 包非常不方便，有些 jar 找不到。
3. jar 包添加到工程中导致工程过大。

#### 1.3.2 maven项目的依赖管理

maven 项目管理所依赖的 jar 包不需要手动向工程添加 jar 包，只需要在 `pom.xml`（maven工程的配置文件）添加 jar 包的坐标，自动从 maven 仓库中下载 jar 包、运行，如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-36990199.jpg)

使用 maven 依赖管理添加 jar 的好处：

1. 通过 pom.xml 文件对 jar 包的版本进行统一管理，可避免版本冲突。
2. maven 团队维护了一个非常全的 maven 仓库，里边包括了当前使用的 jar 包，maven 工程可以自动从 maven 仓库下载 jar 包，非常方便。

### 1.4 使用maven的好处

通过上边介绍传统项目和 maven 项目在项目构建及依赖管理方面的区域，maven 有如下的好处：

1. 一步构建：maven 对项目构建的过程进行标准化，通过一个命令即可完成构建过程。
2. 依赖管理：maven工程不用手动导 jar 包，通过在 pom.xml 中定义坐标从 maven 仓库自动下载，方便且不易出错。
3. maven 的跨平台，可在 window、linux 上使用。
4. maven 遵循规范开发有利于提高大型团队的开发效率，降低项目的维护成本，大公司都会考虑使用 maven 来构建项目。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-26657875.jpg)

## 二、Maven安装

### 2.1 下载安装

1、下载：从该网站 http://maven.apache.org/download.cgi 下载

2、解压：将 maven 解压到一个不含有中文和空格的目录中

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-34445105.jpg)

- bin 目录：mvn.bat （以run方式运行项目）、 mvnDebug.bat（以debug方式运行项目 ）
- boot 目录：maven 运行需要类加载器 
- conf 目录：settings.xml 整个maven工具核心配置文件 
- lib 目录：maven 运行依赖jar包

### 2.2 环境变量配置

电脑上需安装 java 环境，安装 JDK1.7 + 版本  （将 JAVA_HOME/bin 配置环境变量 path ）

配置 MAVEN_HOME：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-89723070.jpg)

将 `%MAVEN_HOME%/bin` 加入环境变量 path：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-99967691.jpg)

通过 `mvn -v` 命令检查 maven 是否安装成功。

### 2.3 maven仓库

#### 2.3.1 maven仓库的作用

maven 的工作需要从仓库下载一些 jar 包，如下图所示，本地的项目 A、项目 B 等都会通过 maven 软件从远程仓库（可以理解为互联网上的仓库）下载 jar 包并存在本地仓库，本地仓库就是本地文件夹，当第二次需要此 jar 包时则不再从远程仓库下载，因为本地仓库已经存在了，可以将本地仓库理解为缓存，有了本地仓库就不用每次从远程仓库下载了。

下图描述了 maven 中仓库的类型：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-82089781.jpg)

- 本地仓库 ：用来存储从远程仓库或中央仓库下载的插件和 jar 包，项目使用一些插件或 jar 包，优先从本地仓库查找。默认本地仓库位置在 `${user.dir}/.m2/repository`，`${user.dir}`表示 windows 用户目录。

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-59819473.jpg)

- 远程仓库：如果本地需要插件或者 jar 包，本地仓库没有，默认去远程仓库下载。远程仓库可以在互联网内也可以在局域网内。

- 中央仓库 ：在 maven 软件中内置一个远程仓库地址 `http://repo1.maven.org/maven2`，它是中央仓库，服务于整个互联网，它是由 Maven 团队自己维护，里面存储了非常全的 jar 包，它包含了世界上大部分流行的开源项目构件。

#### 2.3.2 配置本地仓库

在 MAVE_HOME/conf/settings.xml 文件中配置本地仓库位置：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-70959264.jpg)

表示下载的 jar 包会存放在该磁盘位置。

#### 2.3.3 全局setting与用户setting

maven 仓库地址、私服等配置信息需要在 setting.xml 文件中配置，分为全局配置和用户配置。

在 maven 安装目录下的有 conf/setting.xml 文件，此 setting.xml 文件用于 maven 的所有 project 项目，它作为 maven 的全局配置。

如需要个性配置则需要在用户配置中设置，用户配置的 setting.xml 文件默认的位置在：`${user.dir} /.m2/settings.xml`目录中，`${user.dir}` 指 windows 中的用户目录。

maven 会先找用户配置，如果找到则以用户配置文件为准，否则使用全局配置文件。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-91196105.jpg)

## 三、入门程序

### 3.1 maven工程运行演示

通过使用 maven 提供的命令来运行 maven 工程。

1、运行 web 工程

进入 maven 工程目录（当前目录有 pom.xml 文件），运行 `tomcat:run` 命令。\

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-66302162.jpg)

根据上边的提示信息，通过浏览器访问：http://localhost:8080/maven-helloworld/

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-16531592.jpg)

2、问题处理

如果本地仓库配置错误会报下边的错误：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-14452134.jpg)

分析：maven 工程运行先从本地仓库找 jar 包，本地仓库没有再从中央仓库找，上边提示 downloading。。。表示从中央仓库下载 jar，由于本地没有联网，报错。

解决：在 maven 安装目录的 conf/setting.xml 文件中配置本地仓库。

### 3.2 maven项目工程目录约定

使用 maven 创建的工程我们称它为 maven 工程，maven 工程具有一定的目录规范，如下：

``` xml
src/main/java —— 存放项目的.java文件
src/main/resources —— 存放项目资源文件，如spring, hibernate配置文件
src/test/java —— 存放所有单元测试.java文件，如JUnit测试类
src/test/resources —— 测试资源文件
target —— 项目输出位置，编译后的class文件会输出到此目录
pom.xml——maven项目核心配置文件
```

maven工程的标准目录结构：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-70714503.jpg)

maven 目录结构的规范：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-30-31263930.jpg)

### 3.3 maven常用命令

#### 3.3.1 compile

compile 是 maven 工程的编译命令，作用是将 src/main/java 下的文件编译为 class 文件输出到 target 目录下。进入 cmd 命令状态，执行 mvn compile，如下图提示成功：
![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-28675263.jpg)

查看 target 目录，class 文件已生成，编译完成。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-77302635.jpg)



#### 3.3.2 test

test 是 maven 工程的测试命令，会执行 src/test/java 下的单元测试类。进入 cmd 下执行 `mvn test` 执行 src/test/java 下单元测试类，下图为测试结果，运行1个测试用例，全部成功。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-19222977.jpg)

#### 3.3.3 clean

clean 是 maven 工程的清理命令，执行 clean 会删除 target 目录的内容。

#### 3.3.4 package

package 是 maven 工程的打包命令，对于 java 工程执行 package 打成 jar 包，对于 web 工程打成 war 包。

#### 3.3.5 install

install 是 maven 工程的安装命令，执行 install 将 maven 打成 jar 包或 war 包发布到本地仓库。



## 四、生命周期（了解）

maven 对项目构建过程分为三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”，这三套生命周期分别是：

- CleanLifecycle 在进行真正的构建之前进行一些清理工作。
- DefaultLifecycle 构建的核心部分，编译，测试，打包，部署等等。
- Site Lifecycle 生成项目报告，站点，发布站点。

#### 4.1 生命周期的阶段 

每个生命周期都有很多阶段，每个阶段对应一个执行命令。

1、如下是 clean 生命周期的阶段

- pre-clean 执行一些需要在 clean 之前完成的工作 

- clean 移除所有上一次构建生成的文件 

- post-clean 执行一些需要在 clean 之后立刻完成的工作 


2、如下是 default 周期的内容

``` xml
validate
generate-sources
process-sources
generate-resources
process-resources     复制并处理资源文件，至目标目录，准备打包。
compile     编译项目的源代码。
process-classes
generate-test-sources 
process-test-sources
generate-test-resources
process-test-resources     复制并处理资源文件，至目标测试目录。
test-compile     编译测试源代码。
process-test-classes
test     使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
prepare-package
package     接受编译好的代码，打包成可发布的格式，如 JAR 。
pre-integration-test
integration-test
post-integration-test
verify
install     将包安装至本地仓库，以让其它项目依赖。
deploy     将最终的包复制到远程的仓库，以让其它开发人员与项目共享。
```

3、如下是 site 生命周期的阶段

- pre-site 执行一些需要在生成站点文档之前完成的工作 
- site 生成项目的站点文档 
- post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备 
- site-deploy 将生成的站点文档部署到特定的服务器上 

### 4.2 命令与生命周期的阶段

每个 maven 命令对应生命周期的某个阶段，例如：`mvn clean` 命令对应 clean 生命周期的 clean 阶段，`mvn test` 命令对应 default 生命周期的 test 阶段。 

执行命令会将该命令在的在生命周期当中之前的阶段自动执行，比如：执行 `mvn clean` 命令会自动执行 pre-clean 和 clean 两个阶段，`mvn test` 命令会自动执行 validate、compile、test 等阶段。

 注意：执行某个生命周期的某个阶段不会影响其它的生命周期！

 如果要同时执行多个生命周期的阶段可在命令行输入多个命令，中间以空格隔开，例如：`clean package` 该命令执行 clean 生命周期的 clean 阶段和 default 生命周期的 package 阶段。

### 4.3 maven的模型概念

maven 包含了一个项目对象模型 （Project Object Model），一组标准集合，一个项目生命周期（Project Lifecycle），一个依赖管理系统（Dependency Management System），和用来运行定义在生命周期阶段（phase）中插件（plugin）目标（goal）的逻辑。

下图是 maven 的概念模型图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-38074086.jpg)

- 项目对象模型(Project Object Model)

  > 一个 maven 工程都有一个 pom.xml 文件，通过 pom.xml 文件定义项目的坐标、项目依赖、项目信息、插件目标等。

- 依赖管理系统(Dependency Management System)

  > 通过 maven 的依赖管理对项目所依赖的 jar 包进行统一管理。
  >
  > 比如：项目依赖 junit4.9，通过在 pom.xml 中定义 junit4.9 的依赖即使用 junit4.9，如下所示是 junit4.9 的依赖定义：
  >
  > ``` xml
  > <!-- 依赖关系 -->
  > <dependencies>
  >     <!-- 此项目运行使用junit，所以此项目依赖junit -->
  >     <dependency>
  >         <!-- junit的项目名称 -->
  >         <groupId>junit</groupId>
  >         <!-- junit的模块名称 -->
  >         <artifactId>junit</artifactId>
  >         <!-- junit版本 -->
  >         <version>4.9</version>
  >         <!-- 依赖范围：单元测试时使用junit -->
  >         <scope>test</scope>
  >     </dependency>
  > ```

- 一个项目生命周期(Project Lifecycle)

  > 使用 maven 完成项目的构建，项目构建包括：清理、编译、测试、部署等过程，maven 将这些过程规范为一个生命周期，如下所示是生命周期的各各阶段：
  >
  > ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-39132423.jpg)
  >
  > maven 通过执行一些简单命令即可实现上边生命周期的各过程，比如执行 `mvn compile` 执行编译、执行 `mvn clean` 执行清理。

- 一组标准集合

  > maven 将整个项目管理过程定义一组标准，比如：通过 maven 构建工程有标准的目录结构，有标准的生命周期阶段、依赖管理有标准的坐标定义等。

- 插件(plugin)目标(goal)

  > maven 管理项目生命周期过程都是基于插件完成的。



## 五、使用Eclipe开发maven项目

### 5.1 插件安装配置

通过入门程序中命令行的方式使用 maven 工作效率不高，可以在 eclipse 开发工具中集成 maven 软件，eclipse 是一个开发工具，maven 是一个项目管理工具，maven 有一套项目构建的规范，在 eclipse 集成 maven 软件，最终通过 eclipse 创建 maven 工程。

1、插件安装

因为使用的 eclipse 版本比较高，它自带了有 maven 插件：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-20448110.jpg)

2、指定 maven 安装目录

一些高版本的 eclipse 已经内置了 maven 的安装，下图是 eclipse mars2 版本中已经内置了 maven3.3.3 版本，项目为了统一 maven 的版本不会使用 eclipse 内置的 maven 版本，这里我们 maven3.3.9。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-64169292.jpg)

3、在 eclipse 中配置仓库的位置

在 eclipse 中配置使用的 maven 的 setting.xml 文件，使用 maven 安装目录下的 setting.xml 文件。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-108852.jpg)

注意：如果修改了 setting.xml 文件需要点击上图中的 “update settings” 按钮对本地仓库重建索引，点击 “Reindex”。

4、构建索引

maven 配置完成需要测试在 eclipse 中是否可以浏览 maven 的本地仓库，如果可以正常浏览 maven 本地仓库则说明 eclipse 集成 maven 已经完成。

在 eclipse 下打开：Window ---> show view ---> other ---> maven Repositories

重构索引，找到 Local respository 本地仓库项，点击 Rebuild index 重建索引：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-68523792.jpg)

### 5.2 定义maven坐标

每个 maven 工程都需要定义本工程的坐标，坐标是 maven 对 jar 包的身份定义，比如：入门程序的坐标定义如下：

``` xml
<!--项目名称，定义为组织名+项目名，类似包名-->
<groupId>cn.itcast.maven</groupId>
<!-- 模块名称 -->
<artifactId>maven-first</artifactId>
<!-- 当前项目版本号，snapshot为快照版本即非正式版本，release为正式发布版本 -->
<version>0.0.1-SNAPSHOT</version>
<packaging > ：打包类型
    jar：执行package会打成jar包
    war：执行package会打成war包
    pom ：用于maven工程的继承，通常父工程设置为pom 
```

### 5.3 构建web工程

1、在 eclipse 中创建一个 maven 工程：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-47266733.jpg)

2、选择 maven project

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-92506662.jpg)

3、点 next 进入下面的界面

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-23478240.jpg)

4、可以看到一个 helloworld 工程，但报错（添加下面的内容就 OK 了）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-30637033.jpg)

5、src/java/main 创建了一个 Servlet，但报错（是因为未导入 servlet 的 jar 包）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-7676029.jpg)

6、要解决问题，就是要将 `servlet-api-xxx.jar` 包放进来，作为 maven 工程应当添加 servlet 的坐标，从而导入它的 jar。但是很多情况我们是不知道 jar 包的的坐标，别急，可以通过如下方式查询：

方式1：使用maven插件的索引功能（如果在本地仓库有我们要的 jar 包，可以在 pom.xml 中右键添加依赖）

直接打开 helloworld 工程的 pom.xml 文件，再添加坐标

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-30041379.jpg)

添加后自动生成的结果如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-99847961.jpg)

方式2：从互联网搜索

- http://search.maven.org/
- http://mvnrepository.com/

添加 jar 包的坐标时，还可以指定这个 jar 包将来的作用范围：

> A 依赖 B，需要在 A 的 pom.xml 文件中添加 B 的坐标，添加坐标时需要指定依赖范围，依赖范围包括：
>
> - compile：编译范围，指 A 在编译时依赖 B，此范围为默认依赖范围。编译范围的依赖会用在编译、测试、运行，由于运行时需要所以编译范围的依赖会被打包。
>
>
> - provided：provided 依赖只有在当 JDK 或者一个容器已提供该依赖之后才使用， provided 依赖在编译和测试时需要，在运行时不需要，比如：servlet api 被 tomcat 容器提供。
>
>
> - runtime：runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如：jdbc 的驱动包。由于运行时需要所以 runtime 范围的依赖会被打包。
>
>
> - test：test 范围依赖在编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用，比如：junit。由于运行时不需要所以 test 范围依赖不会被打包。
>
>
> - system：system 范围依赖与 provided 类似，但是你必须显式的提供一个对于本地系统中 JAR 文件的路径，需要指定  systemPath磁盘路径，system 依赖不推荐使用。
>
> ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-68865653.jpg)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-31-85614464.jpg)

