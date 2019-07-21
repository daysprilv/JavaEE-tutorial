
# 1. Java语言的诞生、特点

Java之 父 Jgosling 团队在开发 ”Green” 项目时，发现 C 缺少垃圾回收系统，还有可移植的安全性、分布程序设计、和多线程功能。最后，他们想要一种易于移植到各种设备上的平台。

Java 确实是从 C 语言和 C++ 语言继承了许多成份，甚至可以将 Java 看成是类 C 语言发展和衍生的产物。比如 Java 语言的变量声明，操作符形式，参数传递，流程控制等方面和 C 语言、C++ 语言完全相同。但同时，Java 是一个纯粹的面向对象的程序设计语言，它继承了 C++ 语言面向对象技术的核心。Java 舍弃了 C 语言中容易引起错误的指针（以引用取代）、运算符重载（operator overloading）、多重继承（以接口取代）等特性，增加了垃圾回收器功能用于回收不再被引用的对象所占据的内存空间。JDK1.5 又引入了泛型编程（Generic Programming）、类型安全的枚举、不定长参数和自动装/拆箱。

Java 语言的主要特性：

- Java 语言是安全的

  Java 通常被用在网络环境中，为此，Java 提供了一个安全机制以防恶意代码的攻击。如：安全防范机制（类ClassLoader），如分配不同的名字空间以防替代本地的同名类、字节代码检查。

- Java 语言是体系结构中立的

  Java 程序（后缀为 java 的文件）在 Java 平台上被编译为体系结构中立的字节码格式（后缀为 class 的文件），然后可以在实现这个 Java 平台的任何系统中运行。

- Java 语言是解释型的

  如前所述，Java 程序在 Java 平台上被编译为字节码格式，然后可以在实现这个 Java 平台的任何系统的解释器中运行。

- Java 是性能略高的

  与那些解释型的高级脚本语言相比，Java的性能还是较优的。

- Java语言是原生支持多线程的

  在 Java 语言中，线程是一种特殊的对象，它必须由 Thread 类或其子（孙）类来创建。



# 2. Java语言简介

Java 编程语言是 SUN（Stanford University Network，斯坦福大学网络公司）1995 年推出的一门高级编程语言。1995 年，SUN发布 JDK 1.0，1998 年，JDK1.2，后续 JDK1.3、1.4、1.5（更名为 Java5.0）等等。

Java 语言是一种面向 Internet 的编程语言。随着 Java 技术在 Web 方面的不断成熟，已经成为 Web 应用程序的首选开发语言。

Java 技术体系平台：

（1）Java SE(Java Standard Edition)标准版

支持面向桌面级应用（如 Windows 下的应用程序）的 Java 平台，提供了完整的 Java 核心 API，此版本以前称为 J2SE。

（2）Java EE(Java Enterprise Edition)企业版

是为开发企业环境下的应用程序提供的一套解决方案。该技术体系中包含的技术如：Servlet 、Jsp 等，主要针对于 Web 应用程序开发。版本以前称为 J2EE。

（3）Java ME(Java Micro Edition)小型版

支持 Java 程序运行在移动终端（手机、PDA）上的平台，对 Java API 有所精简，并加入了针对移动终端的支持，此版本以前称为 J2ME。

（4）Java Card

支持一些 Java 小程序（Applets）运行在小内存设备（如智能卡）上的平台。



# 3. 计算机基础要点

## (1) 基础

（1）软件： 系统软件 VS 应用软件

（2）人与计算机做交互：使用计算机语言。

（3）图形化界面 VS 命令行方式（ dir  md rd  cd  cd..  cd/  del  exit）

（4）语言的分类：

- 第一代：机器语言  

- 第二代：汇编语言  

- 第三代：高级语言

（5）Java 语言的特性：①面向对象性 ②健壮性 ③跨平台性（write once ，run anywhere，基于 JVM）

（6）安装 JDK 及配置 path 环境变量

- 1) 傻瓜式安装 JDK；

- 2) path：Windows 操作系统在执行命令时所要搜寻的路径。我们需要将 jdk 中 bin 目录所在的路径: `D:\Java\jdk1.7.0_07\bin` 保存在 path 环境变量下；

- 3) 测试：在命令行窗口，任意的文件目录下，执行 `javac.exe` 或者 `java.exe` 都可以调用成功。

## (2)JDK/JRE/JVM区别

先看下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219154226.png)

### JVM

JVM 是一个虚拟的计算机，具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。

对于不同的平台，有不同的虚拟机。

Java 虚拟机机制屏蔽了底层运行平台的差别，实现了“一次编译，到处运行”。

### JDK

JDK 是提供给 Java 开发人员使用的，其中包含了 java 的开发工具，也包括了 JRE。所以安装了 JDK，就不用在单独安装 JRE 了。

其中的开发工具：编译工具(javac.exe)  打包工具(jar.exe)等。

### JRE

包括 Java 虚拟机（JVM Java Virtual Machine）和Java程序所需的核心类库等，如果想要运行一个开发好的 Java 程序，计算机中只需要安装 JRE 即可。

**简单而言，使用 JDK 的开发工具完成的 java 程序，交给 JRE 去运行。**

> *河床好比操作底层，jdk 好比是河水，Java应用程序好比是船。*

## (3) 如何编写并运行第一个java程序

过程：编写 --> 编译 –> 运行。

（1）编写：每一个 java 文件都是 `.java` 结尾的，称为源文件【HelloWorld.java】。java 程序就存在于源文件中，如下：

```java
public class HelloWorld{
    //程序的主方法，是程序的入口
    public static void main(String args[]){
        //要执行的代码
        System.out.println("HelloWorld");
    }
}
```
注意点：

- Java 源文件以“java”为扩展名。源文件的基本组成部分是类（class），如本类中的 HelloWorld 类。
- 一个源文件中最多只能有一个 public 类。其它类的个数不限，如果源文件包含一个 public 类，则文件名必须按该类名命名。
- Java 应用程序的执行入口是 main() 方法。它有固定的书写格式：`public static void main(String[] args)  {...}`
- Java 语言严格区分大小写。
- Java 方法由一条条语句构成，每个语句以 `;` 结束。
- 大括号都是成对出现的，缺一不可。

（2）编译：在源文件所在的目录下，执行 `javac.exe 源文件名.java` 生成诸多个 `.class` 结尾的字节码文件。

（3）运行：生成的字节码文件通过 `java.exe` 解释执行。

> 程序开发过程，要学会调试程序，这是一项基本操作。

代码注释：  

- 单行注释： `// `  
- 多行注释：`/*    */`   （多行注释不能够嵌套）
- 文档注释：`/**    */ `      



# 4. Java开发工具

文本编辑工具：记事本、UltraEdi、EditPlus、TextPad 等等。

Java 集成开发工具：

（1）IntelliJ IDEA       

IntelliJ IDEA 被认为是当前 Java 开发效率最快的 IDE 工具之一。它整合了开发过程中实用的众多功能，智能提示错误，强大的调试工具，Ant，JavaEE 支持，CVS 整合，最大程度的加快开发的速度。简单而又功能强大。与其他的一些繁冗而复杂的 IDE 工具有鲜明的对比。

（2）Eclipse

IntelliJ IDEA 被认为是当前 Java 开发效率最快的 IDE 工具之一。它整合了开发过程中实用的众多功能，智能提示错误，强大的调试工具，Ant，JavaEE 支持，CVS 整合，最大程度的加快开发的速度。简单而又功能强大。与其他的一些繁冗而复杂的 IDE 工具有鲜明的对比。

（3）Jbuilder

自从 Eclipse 火起来后，JBuilder 就风光不再了。JBuilder 在 04 年之前是最流行的 Java 开发工具，上手很快，非常适合开发 GUI 图形界面和 EJB，效率是其他开发工具至今都难以相比的。

（4）NetBean

SUN 公司的大作，完全免费，有众多插件，与 Eclipse 类似，但是启动太慢，很耗内存，也没有 Eclipse 流行，但是开发 Java、和 Java Web 还可以，整体表现一般，不如 Eclipse 好。



