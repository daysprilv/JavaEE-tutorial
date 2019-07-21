
# 1. 关于导包和Eclipse中Build Path的问题

本人使用的是 IDE 为 Eclipse for Java EE。

1，在 Eclipse 下选择 New --> Project --> Web --> Dynamic Web Project

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-40815914.jpg)

2，下一步，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-25202597.jpg)

填写工程名称，选择 `Dynamic Web module version` 版本，关于这里版本的问题需要说下，选择 3.x 不会在默认勾选`web.xml`文件，选择 2.x 会默认创建`web.xml`文件。详细参考：[dynamic web module version 2.5与3.0的主要区别](https://blog.csdn.net/qq_39056805/article/details/79694331) 及网上资料。

3，假设上一步选择的为 3.0 版本，点击下一步，如图

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-97020019.jpg)

如上图，在这里可以选择 Java 源程序生成的 `.class` 字节码文件放在哪个目录，也可更改为别的地方，如在 WEB-INF 下新建 classes 文件夹，选择该文件夹存放字节码文件（网上有的博客这么做的）

4，下一步，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-62312096.jpg)

这里选择勾选

5，点击 Finish，项目新建完成

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-9087464.jpg)

6，开始编写 servlet 程序，src 目录下新建

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-28219480.jpg)

点击 Browse... 选择继承 HttpServlet，但却发现找不到（其实就是因为缺少对应的 jar 包的导入）。怎么回事呢？是因 servlet 属于 web 服务器技术，需要导入对应的 jar 包，即导入 servlet 的 jar 包（该 jar 包属于 tomcat 服务器的）。操作：找到 tomcat 目录，找到 ...\apache-tomcat-7.0.79\lib，该目录下的 `servlet-api.jar` 即为需要导入的 jar 包；

这里引申出关于导包的问题，如何导 jar 包呢？

- 方式1：可以直接把 jar 包复制到 WEB-INF/lib 目录下，在该项目下会自动生成一个 `Web App Libraries` 并且存在刚刚导入的包（我猜测：复制到 lib 目录下的 jar 包会关联项目），另外，在Build Path 中可以看到相关变化

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-70479329.jpg)



- 方式2：在 Build Path 中点击 Add Library...，再选择 Server Runtime 

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-10763999.jpg)

  然后点击下一步下一步完成即可：

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-36915807.jpg)

> *关于导包这块，花点时间了解下 Build Path 。*

网上资料参考：

- [eclipse的工程的build path引用的jar和工程的webinfo/lib包下所包含的jar包的作用有什么区别?](http://www.iteye.com/problems/31264)
- [Eclipse中Build Path的使用介绍---学习笔记](https://blog.csdn.net/u013991521/article/details/48295115)
- [How to Add JARs to Project Build Paths in Eclipse (Java)](https://www.wikihow.com/Add-JARs-to-Project-Build-Paths-in-Eclipse-(Java))

个人小结：

> 在一个成熟的  Java 工程中，不仅仅有自己编写的源代码，还需要引用系统运行库（JRE）、第三方的功能扩展库、工作空间中的其他工程，甚至外部的类文件，所有这些资源都是被这个工程所依赖的，并且只有被引用后，才能够将该工程编译成功，而 Build Path 就是用来配置和管理对这些资源的引用的。

看了一些资料后我的理解：第一种方式其实是单独导入第三方 jar 包（复制到 WEB-INF/lib 目录下，Build Path 会自动把其和项目关联，也可移除，右键 `Web App Libraries` --> Build Path --> Remove from Build Path，则移除了，然后如果要重新关联，则右键 lib 目录下的 `servlet-api.jar` --> Add to Build Path ）；第二种方式则是把整个 Tomcat 存在的的 jar 包都给导入了。

> 更新认识：经过应该是，Eclipse 下如果是新建的动态 Web 工程，复制到 lib 目录下的 jar 包自动会加入 Build Path，如果新建的是普通 Java 项目，则需要手动 Add to Build Path。

如上导包完成，则可以搜索到 HttpServlet了，于是便可以编写自己的 servlet 继承 HttpServlet。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-9114233.jpg)



# 2. 关于项目发布 

Eclipse 下具体如何发布 Web 动态项目就不多说了，参考：[Eclipse开发JavaWeb项目配置Tomcat，详细教程](https://blog.csdn.net/zs20082012/article/details/79138204)

这里要提一下关于 Tomcat 发布设置的问题。如果把项目发布到关联的 Tomcat 的 webapps 目录下（即会把项目代码复制到该目录），则需要在新建 tomcat 服务器的时候进行设置，如图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-54924843.jpg)

> 上图的红框部分表明，服务的部署是在 eclipse 里面（默认是放在工作空间里的 `.metadata` 文件夹，发布在 wtpwebapps 目录下），而不是发布在 Tomcat 的 webapps 目录下。

如需更改为发布在 tomcat 下，则做法是：

在 Server Locations 下更改为选择第二项 `Use Tomcat installation（takes control of Tomcat installation）`，Deploy path 下选择想要发布到目录名称，如果是 wtpwebapps ，则会在 tomcat 下生成 wtpwebapps 目录并且发布在此：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-19-98726659.jpg)

可以更改为 webapps，统一发布在这个目录。另外关于服务器端口等设置，网上都有很多资料，就不做记录了。



# 3. 关于Eclipse配置

Eclipse 需要做的配置：

1，统一工作空间的编码，选择 UTF-8（方法：Windows --> Preference --> 搜索 Workspace）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-222302.jpg)

2，把创建 JSP 页面的编码修改 UTF-8（方法：Windows --> Preference --> 搜索 jsp）

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-8-25-23658950.jpg)



