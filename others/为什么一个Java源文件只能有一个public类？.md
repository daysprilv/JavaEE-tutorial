为什么一个 Java 源文件只能有一个 public 类？

> 突然想到这个问题，求知欲使我继续探究下去。文章内容来源：[浅谈为什么一个java源文件中只能有一个public类？](https://blog.csdn.net/bareheadzzq/article/details/6562211)，然后我实践了一遍。



网上很多都会提到：**每个编译单元(文件)只能有一个 public 类，这么做的意思是，每个编译单元只能有一个公开的接口，而这个接口就由其 public 类来表示。** 

实验①：

`Test3.java` 源文件：

``` java
class Test1 {
	int i = 1;
}

class Test2 {
	int i = 2;
	public static void main(String[] args) {
		System.out.println("main method");
	}
}
```

首先编译：`javac Test3.java`，可以看到有会生成如下两个字节码文件：`Test1.class`、`Test2.class`。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-30-58946191.jpg)

反编译`Test2.class`文件可以看到：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-30-50733391.jpg)

然后`java Test2`，即运行 Test2，可以看到结果：

``` xml
main method
```

但运行 Test3（`java Test3`） 则会报错，找不到该类，这个错误原因很简单，JVM 的类加载器找不到 `Test3.class`。同时也说明了**包含main()的类如果想运行则不一定要是public的。**

《深入JVM第二版》中有这样一句话：

> Java 虚拟机实例通过调用某个类的main()来运行一个 Java 程序，而这个 main() 必须是 public static void 并接收一个字符串数组作为参数，任何拥有这样一个 main() 的类都可以作为 Java 程序的起点。

可以看出：并没有说拥有 main() 方法的类一定要是 public 类。



实验②：

`Test7.java` 源文件：

``` java
class Test5 {
	int i = 1;
}

public class Test6 {
	int i = 2;
	public static void main(String[] args) {
		System.out.println("main method");
    }   
}
```

如果编译 Test7.java 报错。`javac Test7.java`

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-30-37207035.jpg)

这里说明了文件名必须与 public 类的类名一致（如果文件中有public类）。

另外：如果有多个 public 类，那么文件名应该是哪个public类的呢？显然一个 Java 源文件

只能有一个 public 类。

总结：

> 一个Java源文件中最多只能有一个public类，当有一个public类时，源文件名必须与之一致，否则无法编译，如果源文件中没有一个public类，则文件名与类中没有一致性要求。至于main()不是必须要放在public类中才能运行程序。



对于更详细的解释可以阅读这篇文章：[Java文件中为什么只能有一个public修饰的类, 并且类名还必须与文件名相同](https://www.cnblogs.com/brucecloud/p/5504242.html)


