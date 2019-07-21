
# 1. 关键字

关键字的定义：被 Java 语言赋予了特殊含义，用做专门用途的字符串（单词）

关键字的特点：关键字中所有字母都为小写

- 用于定义数据类型的关键字：class 、interface、enum、byte、short、int、long、float、double、char、boolean、void
- 用于定义数据类型值的关键字：true、false、null
- 用于定义流程控制的关键字：if、else、switch、case、default、while、do、for、break、continue、return
- 用于定义访问权限修饰符的关键字：private、protected、public
- 用于定义类，函数，变量修饰符的关键字：abstract、final、static、synchronized
- 用于定义类与类之间关系的关键字：extends、implements
- 用于定义建立实例及引用实例，判断实例的关键字：new、this、super、instanceof
- 用于异常处理的关键字：try、catch、finally、throw
- 用于包的关键字：package、import
- 其他修饰符关键字：native、strictfp、transient、volatile

> Java 保留字：现有 Java 版本尚未使用，但以后版本可能会作为关键字使用。自己命名标识符时要避免使用这些保留字：byValue、cast、future、generic、inner、operator、outer、rest、var、goto、const



# 2. 标识符

Java 对各种变量、方法和类等要素命名时使用的字符系列成为标识符。凡是自己可以起名字的地方都叫标识符。

定义合法标识符规则：

1. 由 26 个英文字母大小写，0-9，_ 或 $ 组成
2. 数字不可以开头
3. 不可以使用关键字和保留字，但能包含关键字和保留字
4. Java 中严格区分大小写，长度无限制
5. 标识符不能包含空格

> 注意：在起名字时，为了提高阅读性，要尽量有意义，“见名知意”。

Java 中的名称命名规范：（不遵守，也不会出现编译的错误）

- 包名：多单词组成时所有字母都小写：xxxyyyzzz
- 类名、接口名：多单词组成时，所有单词的首字母大写：XxxYyyZzz
- 变量名、方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：xxxYyyZzz
- 常量名：所有字母都大写。多单词时每个单词用下划线连接：XXX_YYY_ZZZ



# 3. 变量

**（1）Java 中变量按照数据类型来分类：基本数据类型 vs  引用数据类型**

1. 基本数据类型：
   - 整型：byte（8 bit）  short   int（默认类型）   long
   - 浮点型：float double （默认类型）
   - 字符型：char（‘ ’）
   - 布尔类型： boolean（只能取值为 true 或 false，不能取 null）
2. 引用数据类型：数组、类、接口

补充，按照在类中存在的位置的不同：成员变量 vs 局部变量

**（2）进制（了解）**

- 二进制：计算机底层都是用二进制来存储、运算。
- 二进制 与十进制之间的转换。
- 二进制在底层存储：正数、负数都是以补码的形式存储的。（原码、反码、补码）
- 四种进制间的转换

**（3）变量的运算**

①自动类型转换：容量小的数据类型自动转换为容量大的数据类型。

``` java
short s = 12;
int i = s + 2;
```

> 注意：byte  short char之间做运算，结果为 int 型。

②强制类型转换：是①的逆过程。使用 `()` 实现强转。



# 4. 运算符

运算符是一种特殊的符号，用以表示数据的运算、赋值和比较等。

**（1）算术运算符**

``` xml
+  -  + - * / % ++ -- +
```

注意：

``` xml
1） /:  int i = 12;  i = i / 5;
2） %：最后的符号只跟被模数相同
3）前++：先+1，后运算     后++：先运算，后+1
4）+：String字符串与其他数据类型只能做连接运算，且结果为String类型。sysout('' + '\t' + ''); vs  sysout("" + '\t' + '');
```

**（2）赋值运算符**

``` xml
=   +=  -=  *=  /=   %=
int i= 12；
i  = i * 5;
i *= 5;//与上一行代码同样的意思
```

特别地：

``` xml
short s = 10;
s = s + 5;//报编译的异常
s = (short)(s + 5);
s += 5;//s = s + 5，但是结果不会改变s的数据类型。
```

**（3）比较运算符（关系运算符）**

``` xml
==  >   <  >=   <= instanceof  
```

注意：区分 `==`  与 `=`  区别。

进行比较运算操作以后，返回一个 boolean 类型的值

- 4>=3：表达的是4 > 3或者 4 = 3，结果是 true。

- `if(i > 1 && i < 10){  }`不能写为：`if(1 < i < 10){}`

**（4）逻辑运算符**

``` xml
& &&  |  ||  ^ !
```

注意：运算符的两端是 boolean 值。另外，区分 & 与 && 的区别，以及 |  与 || 的区别。我们使用的时候，选择&& ， ||

**（5）位运算符**

``` xml
<<   >>   >>>  &  |   ^  ~
```

注意：两端是数值类型的数据。

例子：

1. 如何交换m = 12和n = 5的值
2. 将60转换为十六进制输出。             

**（6）三元运算符**

``` xml
(条件表达式)？ 表达式1 ： 表达式2；
```

1) 既然是运算符，一定会返回一个结果，并且结果的数据类型与表达式 1，2 的类型一致

2) 表达式1与表达式2的数据类型一致。

3) 使用三元运算符的，一定可以转换为 if-else。反之不一定成立。例子：获取两个数的较大值；获取三个数的最大值。



# 5. 程序流程控制

## (1) 顺序结构

程序从上到下逐行地执行，中间没有任何判断和跳转。

## (2) 分支结构

根据条件，选择性地执行某段代码。有 if...else 和 switch...case 两种分支语句。

### ①if-else语句

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219163445.png)

例如实现：

``` xml
/*
	score>=90 	等级为：A
	70<=score<90	等级为：B
	60<=score<70	等级为C
	score<60	等级为：D
*/
```

Java 代码：

```java
import java.util.Scanner;
public class TestScore {
	public static void main(String[] args) {
		Scanner s = new Scanner(System.in);
		System.out.println("请输入学生成绩：");
		int score = s.nextInt();
		char level;
		if (score >= 90) {
			level = 'A';
			System.out.println("等级为："+level);
		}
		if (score >= 70 && score < 90) {
			level = 'B';
			System.out.println("等级为："+level);
		}
		if (score >= 60 && score < 70) {
			level = 'C';
			System.out.println("等级为："+level);
		}
		if (score < 60) {
			level = 'D';
			System.out.println("等级为："+level);
		}
	}
}
```

```java
import java.util.Scanner;
public class TestScore {
	public static void main(String[] args) {
		Scanner s = new Scanner(System.in);
		System.out.println("请输入学生成绩：");
		int score = s.nextInt();
		char level;
		if (score > 90) {
			level = 'A';
		} else if (score >= 70) {
			level = 'B';
		} else if (score >= 60) {
			level = 'C';
		} else {
			level = 'D';
		}
		System.out.println("等级为：" + level);
	}
}
```

### ②switch-case语句

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219163648.png)

1. 没有写 break; 语句，则在找到对应 case 语句后，还会继续向下执行。
2. 其中变量可以是哪些类型？可以是 char，byte，short，int，枚举，String(jdk1.7)，double、float 等不可以。
3. case 条件：其中条件只能是值，不能是取值范围。

## (3) 循环结构
根据循环条件，重复性的执行某段代码。有 while、do...while、for 三种循环语句。

注：JDK1.5 提供了 foreach 循环，方便的遍历集合、数组元素。

①初始化条件 ②循环条件 ③迭代条件 ④循环体

### ①for循环

``` xml
1. 格式：
for(①;②;③){
//④
}
2. 执行过程：①-②-④-③-②-④-③-....-④-③-②
```

### ②while循环

``` xml
格式：
①
while(②){
	④
    ③
}
```

### ③do-while循环

``` xml
格式：
do{
④
③
}while(②)
```

### 无限循环

``` xml
for( ; ; ){}
或者
while(true){
}
```

说明：一般情况下，在无限循环内部要有程序终止的语句，使用 break 实现，若没有，那就是死循环。

1) 嵌套循环例子，实现如下图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219164154.png)

Java 代码：

```java
public class TestFor {
	public static void main(String[] args) {
		//上半部分
		for(int i = 0;i < 5; i++){
			for(int k = 0; k < 4-i; k++){
				System.out.print(" ");
			}
			for(int j = 0;j < i+1; j++){
				System.out.print("* ");
			}
			System.out.println();
		}
		//下半部分
		for(int i = 0; i < 4; i++){
			for(int k =0;k < i+1; k++){
				System.out.print(" ");
			}
			for(int j = 0; j < 4-i; j++){
				System.out.print("* ");
			}
			System.out.println();
		}
	}
}
```

2) 实现九九乘法表

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219164234.png)

Java 代码：

```java
public class TestJiuJiu {
	public static void main(String[] args) {
		for(int i = 1;i <= 9; i++){//一共有九行
			for(int j = 1;j <= i; j++){//每行有 i 个等式
				System.out.print(i + "*" + j + "=" + i*j + "\t");
			}
			System.out.println();
		}
	}
}
```

## (4) break和continue关键字

①break：使用在 switch-case 中或者循环中。如果使用在循环中，表示：结束“当前”循环。

②continue：使用在循环结构中，表示：结束“当次”循环。

关于 break 和 continue 中标签的使用。

```java
public class TestBreakContinue {
	public static void main(String[] args) {
		//break和continue中标签的使用
		label:for (int i = 1; i < 5; i++) {
			for (int j = 1; j < 10; j++) {
				if(j % 4 == 0){
					//break;
					//continue;
					continue label;
				}
				System.out.print(j);
			}
			System.out.println();
		}
	}
}
```



# 6. 数组

数组：相同数据类型的数据的组合。

## (1) 一维数组

如：

``` xml
int score = 72;
int score = 90;
int score = 58;
```

使用数组：

1. 静态初始化：在声明并初始化与给数组相应的元素赋值操作同时进行。

  ` int[] score1 = new int[]{72, 90, 58};` //int[] score1 = {72， 90， 58};

2. 动态初始化：在声明并初始化与给数组相应的元素赋值操作分开进行。

   ``` java
   int score2 = new int[3]；
   score[0] = 72;
   score[1] = 90;
   score[2] = 58;
   ```

注：数组长度一旦创建后数组长度不可变。

声明数组的错误写法：

``` xml
1) String names = new String[5]{"AA","BB","CC"};
2) int a[10];
3) int i = new int[];
```

**注：** 

1. 对于 byte、short、int、long 数组元素值默认为 0
2. 对于 float、double 数组元素值默认为 0.0
3. 对于 char 数组元素值默认为空格
4. 对于 boolean 数组元素值默认为 false
5. 对于引用类型的变量构成的数组而言，默认初始化为 null，以 String 为例

## (2) 二维数组

①静态初始化

``` java
int[][ ] scores;
scores = new int[][]{ {1, 2, 3},{3, 4, 5},{6} };
```

②动态初始化

```java
String[][] names;
names = new String[3][2];//动态初始化之一
或者
names = new String[4][];//动态初始化之二（不指定二维的长度）
names[0] = new String[5];
names[1] = new String[4];
namse[2] = new String[7];
```
错误的初始化：

``` xml
names = new String;
names = new String;
都是未指定第一维长度。
```

Q：二维数组如何遍历？

```java
for(int m = 0;m < score.length;m++ ){
    for(int n = 0;n < score[m].length;n++){
        System.out.println(score[m][n]);
    }
}
```
## (3) 内存结构

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219164713.png)

***举例：***

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219164740.png)

一维数组练习：

``` xml
/*
 从键盘读入学生成绩，找出最高分，并输出学生成绩。
 成绩>=最高分-10	等级为A
 成绩>=最高分-20	等级为B
 成绩>=最高分-30	等级为C
 其余				等级为D
 提示：先读入学生人数，根据人数创建int数组，存放学生成绩
 */
```

Java 代码：

```java
public class TestStudentScore {
    public static void main(String[] args) {
        // 1,创建Scanner的对象，并从键盘获取学生的个数n
        Scanner s = new Scanner(System.in);
        System.out.println("请输入学生的个数：");
        int count = s.nextInt();// count记录学生的个数
        // 2,根据输入的学生个数n，创建一个长度为n的int型数组
        int[] scores = new int[count];
        int maxScore = 0;
        // 3,依次从键盘获取n个学生的成绩，并赋给相应的的数组元素，并获取n个学生中的最高分
        System.out.println("请输入" + count + "个数学生成绩：");
        for (int i = 0; i < scores.length; i++) {
            int score = s.nextInt();// 依次从键盘获取学生的成绩
            scores[i] = score;
            if (scores[i] > maxScore) {
                maxScore = scores[i];
            }
        }
        System.out.println("最高分为：" + maxScore);
        // 4,遍历学生成绩的数组，并根据学生成绩与最高分的差值，赋予相应的等级，并输出
        for (int i = 0; i < scores.length; i++) {
            char level;
            if (scores[i] >= maxScore - 10) {
                level = 'A';
            } else if (scores[i] >= maxScore - 20) {
                level = 'B';
            } else if (scores[i] >= maxScore - 30) {
                level = 'C';
            } else {
                level = 'D';
            }
            System.out.println("Student " + (i + 1) + " score is " + scores[i]
                               + " level is " + level);
        }
    }
}

```

声明：`int[]x，y[];`，以下选项允许通过编译的是：

``` java
a）x[0]=y；//no 
b）y[0]=x；//yes 
c）y[0][0]=x；//no
d）x[0][0]=y；//no 
e）y[o][0]=x[0]；//yes 
f）x=y；//no

一维数组：int[]x或者intx[]
二维数组：int[][]y或者 int[]y]或者int yl[]214
```

## (4) 数组的常见异常

```java
//1.数组下标越界的异常:java.lang.ArrayIndexOutOfBoundsException
		int[] i = new int[10];
//		i[0] = 90;
//		i[10] = 99;
		
//		for(int m = 0;m <= i.length;m++){
//			System.out.println(i[m]);
//		}
		//2.空指针的异常：NullPointerException
		//第一种：
//		boolean[] b = new boolean[3];
//		b = null;
//		System.out.println(b[0]);
		
		//第二种：
//		String[] str = new String[4];
//		//str[3] = new String("AA");//str[3] = "AA";
//		System.out.println(str[3].toString());
		
		//第三种：
		int[][] j = new int[3][];
		j[2][0] = 12;
```