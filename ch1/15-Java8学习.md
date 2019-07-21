
# 0. 前言

2014 年，Oracle 发布了 Java8 新版本。对于 Java 来说，这显然是一个具有里程碑意义的版本。但我在 2017 年 6 月才开始学习 Java8。

关于 Java8 学习的文章：

- [Java8学习小记](https://segmentfault.com/a/1190000006985405)
- [Java 8的新特性—终极版](http://blog.csdn.net/yczz/article/details/50896975#t2)
- [Java8初体验（二）Stream语法详解](http://ifeve.com/stream/)
- [Java基础知识总结之1.8新特性lambda表达式](http://www.cnblogs.com/changyaohua/p/4648772.html)

Java8 新特性了解下：

- 速度更快
- 代码更少（增加了新的语法 Lambda 表达式）
- 强大的 Stream API
- 便于并行
- 最大化减少空指针异常 Optional

其中最为核心的为 Lambda 表达式与 Stream API。



# 1. Lambda表达式

Q：为什么使用 Lambda 表达式？

A：Lambda 是一个匿名函数，我们可以把 Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

## (1) 从匿名内部类到Lambda的转换

例1：
``` java
//匿名内部类
Runnable r1 = new Runnable() {
	@Override
	public void run() {
		System.out.println("hello world");
	}
};
```
``` java
//Lambda表达式
Runnable r1 = () -> System.out.println("hello Lambda");
```
例2：
``` java
//原来使用匿名内部类作为参数传递
TreeSet<String> ts2 = new TreeSet<>(new Comparator<String>(){
	@Override
	public int compare(String o1, String o2) {
		return Integer.compare(o1.length(), o2.length());
	}		
});
```
``` java
//Lambda 表达式作为参数传递
Comparator<String> com = (x, y) -> Integer.compare(x.length(), y.length());
```
## (2) Lambda表达式语法
Lambda 表达式在 Java8 中引入了一个新的语法元素和操作符。这个操作符为 “->” ， 该操作符被称为 Lambda 操作符或剪头操作符。它将 Lambda 分为两个部分：

- 左侧： 指定了 Lambda 表达式需要的所有参数
- 右侧： 指定了 Lambda 体，即 Lambda 表达式要执行的功能。

**语法格式一：**无参数，无返回值 `() -> System.out.println("Hello Lambda!");`

**语法格式二：**有一个参数，并且无返回值 `(x) -> System.out.println(x);`

**语法格式三**：若只有一个参数，小括号可以省略不写 `x -> System.out.println(x);`

**语法格式四**：有两个以上的参数，有返回值，并且 Lambda 体中有多条语句

``` java
Comparator<Integer> com = (x, y) -> {
	System.out.println("函数式接口");
	return Integer.compare(x, y);
};
```

**语法格式五：**若 Lambda 体中只有一条语句，return 和 大括号都可以省略不写 `Comparator<Integer> com = (x, y) -> Integer.compare(x, y);`

**语法格式六：** Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断” `(Integer x, Integer y) -> Integer.compare(x, y);`
注：上述 Lambda 表达式中的参数类型都是由编译器推断得出的。 Lambda 表达式中无需指定类型，程序依然可以编译，这是因为 javac 根据程序的上下文，在后台推断出了参数的类型。 Lambda 表达式的类型依赖于上下文环境，是由编译器推断出来的。这就是所谓的“类型推断”。

上联：左右遇一括号省

下联：左侧推断类型省

横批：能省则省



# 2. 函数式接口

Q：什么是函数式接口？

- 只包含一个抽象方法的接口，称为函数式接口。
- 你可以通过 Lambda 表达式来创建该接口的对象。（若 Lambda 表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方法上进行声明）。
- 我们可以在任意函数式接口上使用 @FunctionalInterface 注解，这样做可以检查它是否是一个函数式接口，同时 javadoc 也会包含一条声明，说明这个接口是一个函数式接口。

## (1) 自定义函数接口
``` java
@FunctionalInterface
public interface MyNumber {
	public dobule getValue();
}

//函数式接口中使用泛型
@FunctionalInterface
public interface MyFunc<T> {
	public T getValue(T t);
}
```

作为参数传递 Lambda 表达式：
``` java
public String toUpperString(MyFunc<String> mf, String str){
	return mf.getValue(Str);
}

//作为参数传递 Lambda 表达式
String newStr = toUpperString(
	(str) -> str.toUpperCase(), "abcdef");
System.out.println(newStr);
```
注：作为参数传递 Lambda 表达式：为了将 Lambda 表达式作为参数传递，接收 Lambda 表达式的参数类型必须是与该 Lambda 表达式兼容的函数式接口的类型。

## (2) Java内置四大核心函数式接口

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219223854.png)

Java8 内置的四大核心函数式接口

``` java
Consumer：消费型接口 
void accept(T t);

Supplier<T>：供给型接口 
T get();

Function<T, R>：函数型接口 
R apply(T t);

Predicate<T>：断言型接口 
boolean test(T t);
```

断言型接口：
``` java
//Predicate<T> 断言型接口：
@Test
public void test4(){
    List<String> list = Arrays.asList("Hello", "atguigu", "Lambda", "www", "ok");
    List<String> strList = filterStr(list, (s) -> s.length() > 3);

    for (String str : strList) {
        System.out.println(str);
    }
}

//需求：将满足条件的字符串，放入集合中
public List<String> filterStr(List<String> list, Predicate<String> pre){
    List<String> strList = new ArrayList<>();

    for (String str : list) {
        if(pre.test(str)){
            strList.add(str);
        }
    }

    return strList;
}
```

函数型接口：
``` java
//Function<T, R> 函数型接口：
@Test
public void test3(){
    String newStr = strHandler("\t\t\t 我大尚硅谷威武   ", (str) -> str.trim());
    System.out.println(newStr);

    String subStr = strHandler("我大尚硅谷威武", (str) -> str.substring(2, 5));
    System.out.println(subStr);
}

//需求：用于处理字符串
public String strHandler(String str, Function<String, String> fun){
    return fun.apply(str);
}
```

供给型接口：
``` java
//Supplier<T> 供给型接口 :
@Test
public void test2(){
    List<Integer> numList = getNumList(10, () -> (int)(Math.random() * 100));

    for (Integer num : numList) {
        System.out.println(num);
    }
}

//需求：产生指定个数的整数，并放入集合中
public List<Integer> getNumList(int num, Supplier<Integer> sup){
    List<Integer> list = new ArrayList<>();

    for (int i = 0; i < num; i++) {
        Integer n = sup.get();
        list.add(n);
    }

    return list;
}
```

消费型接口：
``` java
//Consumer<T> 消费型接口 :
@Test
public void test1(){
    happy(10000, (m) -> System.out.println("你们刚哥喜欢大宝剑，每次消费：" + m + "元"));
} 

public void happy(double money, Consumer<Double> con){
    con.accept(money);
}
```

## (3) 其他接口
![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219224145.png)



# 3. 方法引用与构造器引用

## (1) 方法引用
若 Lambda 体中的功能，已经有方法提供了实现，可以使用方法引用（可以将方法引用理解为 Lambda 表达式的另外一种表现形式）

方法引用：使用操作符 `::` 将方法名和对象或类的名字分隔开来。

如下三种主要使用情况：

1. `对象::实例方法`
2. `类::静态方法`
3. `类::实例方法`

注意：

- ①方法引用所引用的方法的参数列表与返回值类型，需要与函数式接口中抽象方法的参数列表和返回值类型保持一致！
- ②若 Lambda 的参数列表的第一个参数，是实例方法的调用者，第二个参数(或无参)是实例方法的参数时，格式：ClassName::MethodName

例如：

- `(x) -> System.out.println(x);`等同于 `System.out::println;`

- `BinaryOperator<Double> bo = (x, y) -> Math.pow(x, y);` 等同于 ` BinaryOperator<Double> bo = Math::pow;`

- `compare((x,y) -> x.equals(y), "abcdef", "abcdef");` 等同于 ` compare(String::equals, "abc", "abc");`

**注意： 当需要引用方法的第一个参数是调用对象，并且第二个参数是需要引用方法的第二个参数(或无参数)时： ClassName::methodName**

对象的引用 :: 实例方法名
``` java
//对象的引用 :: 实例方法名
@Test
public void test2(){
    Employee emp = new Employee(101, "张三", 18, 9999.99);

    Supplier<String> sup = () -> emp.getName();
    System.out.println(sup.get());

    System.out.println("----------------------------------");

    Supplier<String> sup2 = emp::getName;
    System.out.println(sup2.get());
}

@Test
public void test1(){
    PrintStream ps = System.out;
    Consumer<String> con = (str) -> ps.println(str);
    con.accept("Hello World！");

    System.out.println("--------------------------------");

    Consumer<String> con2 = ps::println;
    con2.accept("Hello Java8！");

    Consumer<String> con3 = System.out::println;
}
```

类名 :: 静态方法名
``` java
@Test
public void test4(){
    Comparator<Integer> com = (x, y) -> Integer.compare(x, y);

    System.out.println("-------------------------------------");

    Comparator<Integer> com2 = Integer::compare;
}

@Test
public void test3(){
    BiFunction<Double, Double, Double> fun = (x, y) -> Math.max(x, y);
    System.out.println(fun.apply(1.5, 22.2));

    System.out.println("--------------------------------------------------");

    BiFunction<Double, Double, Double> fun2 = Math::max;
    System.out.println(fun2.apply(1.2, 1.5));
}
```

类名 :: 实例方法名
``` java
@Test
public void test5(){
    BiPredicate<String, String> bp = (x, y) -> x.equals(y);
    System.out.println(bp.test("abcde", "abcde"));

    System.out.println("-----------------------------------------");

    BiPredicate<String, String> bp2 = String::equals;
    System.out.println(bp2.test("abc", "abc"));

    System.out.println("-----------------------------------------");


    Function<Employee, String> fun = (e) -> e.show();
    System.out.println(fun.apply(new Employee()));

    System.out.println("-----------------------------------------");

    Function<Employee, String> fun2 = Employee::show;
    System.out.println(fun2.apply(new Employee()));

}
```

## (2) 构造器引用
格式： ClassName::new  

与函数式接口相结合，自动与函数式接口中方法兼容。可以把构造器引用赋值给定义的方法，与构造器参数列表要与接口中抽象方法的参数列表一致。

例如：`Function<Integer, MyClass> fun = (n) -> new MyClass(n);` 等同于 ` Function<Integer, MyClass> fun = MyClass::new;`

``` java
//构造器引用
@Test
public void test7(){
    Function<String, Employee> fun = Employee::new;

    BiFunction<String, Integer, Employee> fun2 = Employee::new;
}

@Test
public void test6(){
    Supplier<Employee> sup = () -> new Employee();
    System.out.println(sup.get());

    System.out.println("------------------------------------");

    Supplier<Employee> sup2 = Employee::new;
    System.out.println(sup2.get());
}
```

## (3) 数组引用
格式： type[] :: new

` Function<Integer, Integer[]> fun = (n) -> new Integer[n];` 等同于 `Function<Integer, Integer[]> fun = Integer[]::new;`

``` java
//数组引用
@Test
public void test8(){
    Function<Integer, String[]> fun = (args) -> new String[args];
    String[] strs = fun.apply(10);
    System.out.println(strs.length);

    System.out.println("--------------------------");

    Function<Integer, Employee[]> fun2 = Employee[] :: new;
    Employee[] emps = fun2.apply(20);
    System.out.println(emps.length);
}
```



# 4. 强大的Stream API

(1) 了解Stream

Java8 中有两大最为重要的改变。第一个是 Lambda 表达式；另外一个则是 Stream API：`java.util.stream.*`。

Stream 是 Java8 中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。使用Stream API 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。也可以使用 Stream API 来并行执行操作。简而言之，Stream API 提供了一种高效且易于使用的处理数据的方式。

(2) 什么是 Stream？

流（Stream）到底是什么呢？是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。“集合讲的是数据，流讲的是计算！ ”

注意：

- ①Stream 自己不会存储元素。
- ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新 Stream。
- ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

(3) Stream 的操作三个步骤
1. 创建 Stream：一个数据源（如： 集合、数组）， 获取一个流

2. 中间操作：一个中间操作链，对数据源的数据进行处理

3. 终止操作(终端操作)：一个终止操作，执行中间操作链，并产生结果

  ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219224754.png)

## (1) 创建 Stream
Java8 中的 Collection 接口被扩展，提供了两个获取流的方法：

-  `default Stream<E> stream() `：返回一个顺序流
- `default Stream<E> parallelStream()` ：返回一个并行流

Java8 中的 Arrays 的静态方法 stream() 可以获取数组流：

- `static <T> Stream<T> stream(T[] array)`: 返回一个流

重载形式，能够处理对应基本类型的数组：

- `public static IntStream stream(int[] array)`
- `public static LongStream stream(long[] array)`
- `public static DoubleStream stream(double[] array)`

由值创建流：可以使用静态方法 Stream.of()，通过显示值创建一个流。它可以接收任意数量的参数。

`public static<T> Stream<T> of(T... values)` : 返回一个流

由函数创建流：创建无限流。可以使用静态方法 `Stream.iterate()` 和
`Stream.generate()`创建无限流。

- 迭代：`public static<T> Stream<T> iterate(final T seed, final`
  `UnaryOperator<T> f)`
- 生成：`public static<T> Stream<T> generate(Supplier<T> s) :`


## (2) Stream的中间操作
多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理，而在终止操作时一次性全部处理，称为“惰性求值” 。

**筛选与切片：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225048.png)

**映射：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225103.png)

``` java
/*
		映射
		map——接收 Lambda ， 将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
		flatMap——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
*/
@Test
public void test1(){
    Stream<String> str = emps.stream()
        .map((e) -> e.getName());

    System.out.println("-------------------------------------------");

    List<String> strList = Arrays.asList("aaa", "bbb", "ccc", "ddd", "eee");

    Stream<String> stream = strList.stream()
        .map(String::toUpperCase);

    stream.forEach(System.out::println);

    Stream<Stream<Character>> stream2 = strList.stream()
        .map(TestStreamAPI1::filterCharacter);

    stream2.forEach((sm) -> {
        sm.forEach(System.out::println);
    });

    System.out.println("---------------------------------------------");

    Stream<Character> stream3 = strList.stream()
        .flatMap(TestStreamAPI1::filterCharacter);

    stream3.forEach(System.out::println);
}

public static Stream<Character> filterCharacter(String str){
    List<Character> list = new ArrayList<>();

    for (Character ch : str.toCharArray()) {
        list.add(ch);
    }

    return list.stream();
}
```

**排序：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225131.png)

``` java
/*
		sorted()——自然排序
		sorted(Comparator com)——定制排序
	 */
@Test
public void test2(){
    emps.stream()
        .map(Employee::getName)
        .sorted()
        .forEach(System.out::println);

    System.out.println("------------------------------------");

    emps.stream()
        .sorted((x, y) -> {
            if(x.getAge() == y.getAge()){
                return x.getName().compareTo(y.getName());
            }else{
                return Integer.compare(x.getAge(), y.getAge());
            }
        }).forEach(System.out::println);
}
```


## (3) Stream 的终止操作
终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如： List、 Integer，甚至是 void 。

**查找与匹配：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225252.png)

``` java
/*
		allMatch——检查是否匹配所有元素
		anyMatch——检查是否至少匹配一个元素
		noneMatch——检查是否没有匹配的元素
		findFirst——返回第一个元素
		findAny——返回当前流中的任意元素
		count——返回流中元素的总个数
		max——返回流中最大值
		min——返回流中最小值
	 */
@Test
public void test1(){
    boolean bl = emps.stream()
        .allMatch((e) -> e.getStatus().equals(Status.BUSY));

    System.out.println(bl);

    boolean bl1 = emps.stream()
        .anyMatch((e) -> e.getStatus().equals(Status.BUSY));

    System.out.println(bl1);

    boolean bl2 = emps.stream()
        .noneMatch((e) -> e.getStatus().equals(Status.BUSY));

    System.out.println(bl2);
}

@Test
public void test2(){
    Optional<Employee> op = emps.stream()
        .sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()))
        .findFirst();

    System.out.println(op.get());

    System.out.println("--------------------------------");

    Optional<Employee> op2 = emps.parallelStream()
        .filter((e) -> e.getStatus().equals(Status.FREE))
        .findAny();

    System.out.println(op2.get());
}

@Test
public void test3(){
    long count = emps.stream()
        .filter((e) -> e.getStatus().equals(Status.FREE))
        .count();

    System.out.println(count);

    Optional<Double> op = emps.stream()
        .map(Employee::getSalary)
        .max(Double::compare);

    System.out.println(op.get());

    Optional<Employee> op2 = emps.stream()
        .min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));

    System.out.println(op2.get());
}

//注意：流进行了终止操作后，不能再次使用
@Test
public void test4(){
    Stream<Employee> stream = emps.stream()
        .filter((e) -> e.getStatus().equals(Status.FREE));

    long count = stream.count();

    stream.map(Employee::getSalary)
        .max(Double::compare);
}
```

**归约：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225328.png)

注：map 和 reduce 的连接通常称为 map-reduce 模式，因 Google 用它来进行网络搜索而出名。
``` java
/*
		归约
		reduce(T identity, BinaryOperator) / reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。
	 */
@Test
public void test1(){
    List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);

    Integer sum = list.stream()
        .reduce(0, (x, y) -> x + y);

    System.out.println(sum);

    System.out.println("----------------------------------------");

    Optional<Double> op = emps.stream()
        .map(Employee::getSalary)
        .reduce(Double::sum);

    System.out.println(op.get());
}

//需求：搜索名字中 “六” 出现的次数
@Test
public void test2(){
    Optional<Integer> sum = emps.stream()
        .map(Employee::getName)
        .flatMap(TestStreamAPI1::filterCharacter)
        .map((ch) -> {
            if(ch.equals('六'))
                return 1;
            else 
                return 0;
        }).reduce(Integer::sum);

    System.out.println(sum.get());
}
```

**收集：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225355.png)

Collector 接口中方法的实现决定了如何对流执行收集操作(如收集到 List、 Set、 Map)。但是 Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例， 具体方法与实例如下表：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225423.png)

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225442.png)

``` java
//collect——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
@Test
public void test3(){
    List<String> list = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toList());

    list.forEach(System.out::println);

    System.out.println("----------------------------------");

    Set<String> set = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toSet());

    set.forEach(System.out::println);

    System.out.println("----------------------------------");

    HashSet<String> hs = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toCollection(HashSet::new));

    hs.forEach(System.out::println);
}

@Test
public void test4(){
    Optional<Double> max = emps.stream()
        .map(Employee::getSalary)
        .collect(Collectors.maxBy(Double::compare));

    System.out.println(max.get());

    Optional<Employee> op = emps.stream()
        .collect(Collectors.minBy((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));

    System.out.println(op.get());

    Double sum = emps.stream()
        .collect(Collectors.summingDouble(Employee::getSalary));

    System.out.println(sum);

    Double avg = emps.stream()
        .collect(Collectors.averagingDouble(Employee::getSalary));

    System.out.println(avg);

    Long count = emps.stream()
        .collect(Collectors.counting());

    System.out.println(count);

    System.out.println("--------------------------------------------");

    DoubleSummaryStatistics dss = emps.stream()
        .collect(Collectors.summarizingDouble(Employee::getSalary));

    System.out.println(dss.getMax());
}

//分组
@Test
public void test5(){
    Map<Status, List<Employee>> map = emps.stream()
        .collect(Collectors.groupingBy(Employee::getStatus));

    System.out.println(map);
}

//多级分组
@Test
public void test6(){
    Map<Status, Map<String, List<Employee>>> map = emps.stream()
        .collect(Collectors.groupingBy(Employee::getStatus, Collectors.groupingBy((e) -> {
            if(e.getAge() >= 60)
                return "老年";
            else if(e.getAge() >= 35)
                return "中年";
            else
                return "成年";
        })));

    System.out.println(map);
}

//分区
@Test
public void test7(){
    Map<Boolean, List<Employee>> map = emps.stream()
        .collect(Collectors.partitioningBy((e) -> e.getSalary() >= 5000));

    System.out.println(map);
}

//
@Test
public void test8(){
    String str = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.joining("," , "----", "----"));

    System.out.println(str);
}

@Test
public void test9(){
    Optional<Double> sum = emps.stream()
        .map(Employee::getSalary)
        .collect(Collectors.reducing(Double::sum));

    System.out.println(sum.get());
}
```

## (4) 并行流与串行流
并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。

Java8 中将并行进行了优化，我们可以很容易的对数据进行并行操作。 Stream API 可以声明性地通过 `parallel()` 与 `sequential()` 在并行流与顺序流之间进行切换。

## (5) 了解 Fork/Join 框架
Fork/Join 框架： 就是在必要的情况下，将一个大任务，进行拆分(fork)成若干个小任务（拆到不可再拆时），再将一个个的小任务运算的结果进行 join 汇总。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225554.png)

**Fork/Join 框架与传统线程池的区别：**

采用 “工作窃取”模式（work-stealing）：当执行新的任务时它可以将其拆分分成更小的任务执行，并将小任务加到线程队列中，然后再从一个随机线程的队列中偷一个并把它放在自己的队列中。

相对于一般的线程池实现，fork/join 框架的优势体现在对其中包含的任务的处理方式上。在一般的线程池中，如果一个线程正在执行的任务由于某些原因无法继续运行，那么该线程会处于等待状态.而在fork/join框架实现中，如果某个子问题由于等待另外一个子问题的完成而无法继续运行。那么处理该子问题的线程会主动寻找其他尚未运行的子问题来执行。这种方式减少了线程的等待时间,提高了性能。

``` java
import java.util.concurrent.RecursiveTask;

public class ForkJoinCalculate extends RecursiveTask<Long>{

    /**
	 * 
	 */
    private static final long serialVersionUID = 13475679780L;

    private long start;
    private long end;

    private static final long THRESHOLD = 10000L; //临界值

    public ForkJoinCalculate(long start, long end) {
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        long length = end - start;

        if(length <= THRESHOLD){
            long sum = 0;

            for (long i = start; i <= end; i++) {
                sum += i;
            }

            return sum;
        }else{
            long middle = (start + end) / 2;

            ForkJoinCalculate left = new ForkJoinCalculate(start, middle);
            left.fork(); //拆分，并将该子任务压入线程队列

            ForkJoinCalculate right = new ForkJoinCalculate(middle+1, end);
            right.fork();

            return left.join() + right.join();
        }

    }
}
```



# 5. 新时间日期 API

## (1) 使用 LocalDate、 LocalTime、 LocalDateTime
LocalDate、 LocalTime、 LocalDateTime 类的实例是不可变的对象，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间。它们提供了简单的日期或时间，并不包含当前的时间信息。也不包含与时区相关的信息。

注： ISO-8601 日历系统是国际标准化组织制定的现代公民的日期和时间的表示法。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225737.png)

**Instant 时间戳**

用于“时间戳”的运算。它是以 Unix 元年（传统的设定为 UTC时区1970年1月1日午夜时分）开始所经历的描述进行运算。

**Duration 和 Period**

Duration：用于计算两个“时间”间隔。

Period：用于计算两个“日期”间隔。

**日期的操纵**

TemporalAdjuster : 时间校正器。有时我们可能需要获取例如：将日期调整到“下个周日”等操作。

TemporalAdjusters : 该类通过静态方法提供了大量的常用 TemporalAdjuster 的实现。
例如获取下个周日：

``` java
LocalDate nextSunday = LocalDate.now().with(
    TemporalAdjusters.next(DayOfWeek.SUNDAY)
);
```

**解析与格式化**

java.time.format.DateTimeFormatter 类：该类提供了三种格式化方法：

- 预定义的标准格式
- 语言环境相关的格式
- 自定义的格式	

**时区的处理**

Java8 中加入了对时区的支持，带时区的时间为分别为：ZonedDate、 ZonedTime、ZonedDateTime

其中每个时区都对应着 ID，地区ID都为 “{区域}/{城市}”的格式。例如 ： Asia/Shanghai 等

ZoneId：该类中包含了所有的时区信息

getAvailableZoneIds() : 可以获取所有时区时区信息

of(id) : 用指定的时区信息获取 ZoneId 对象

**与传统日期处理的转换**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219225934.png)

``` java
public class TestLocalDateTime {

    //6.ZonedDate、ZonedTime、ZonedDateTime ： 带时区的时间或日期
    @Test
    public void test7(){
        LocalDateTime ldt = LocalDateTime.now(ZoneId.of("Asia/Shanghai"));
        System.out.println(ldt);

        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("US/Pacific"));
        System.out.println(zdt);
    }

    @Test
    public void test6(){
        Set<String> set = ZoneId.getAvailableZoneIds();
        set.forEach(System.out::println);
    }


    //5. DateTimeFormatter : 解析和格式化日期或时间
    @Test
    public void test5(){
        //		DateTimeFormatter dtf = DateTimeFormatter.ISO_LOCAL_DATE;

        DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss E");

        LocalDateTime ldt = LocalDateTime.now();
        String strDate = ldt.format(dtf);

        System.out.println(strDate);

        LocalDateTime newLdt = ldt.parse(strDate, dtf);
        System.out.println(newLdt);
    }

    //4. TemporalAdjuster : 时间校正器
    @Test
    public void test4(){
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        LocalDateTime ldt2 = ldt.withDayOfMonth(10);
        System.out.println(ldt2);

        LocalDateTime ldt3 = ldt.with(TemporalAdjusters.next(DayOfWeek.SUNDAY));
        System.out.println(ldt3);

        //自定义：下一个工作日
        LocalDateTime ldt5 = ldt.with((l) -> {
            LocalDateTime ldt4 = (LocalDateTime) l;

            DayOfWeek dow = ldt4.getDayOfWeek();

            if(dow.equals(DayOfWeek.FRIDAY)){
                return ldt4.plusDays(3);
            }else if(dow.equals(DayOfWeek.SATURDAY)){
                return ldt4.plusDays(2);
            }else{
                return ldt4.plusDays(1);
            }
        });

        System.out.println(ldt5);

    }

    //3.
    //Duration : 用于计算两个“时间”间隔
    //Period : 用于计算两个“日期”间隔
    @Test
    public void test3(){
        Instant ins1 = Instant.now();

        System.out.println("--------------------");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }

        Instant ins2 = Instant.now();

        System.out.println("所耗费时间为：" + Duration.between(ins1, ins2));

        System.out.println("----------------------------------");

        LocalDate ld1 = LocalDate.now();
        LocalDate ld2 = LocalDate.of(2011, 1, 1);

        Period pe = Period.between(ld2, ld1);
        System.out.println(pe.getYears());
        System.out.println(pe.getMonths());
        System.out.println(pe.getDays());
    }

    //2. Instant : 时间戳。 （使用 Unix 元年  1970年1月1日 00:00:00 所经历的毫秒值）
    @Test
    public void test2(){
        Instant ins = Instant.now();  //默认使用 UTC 时区
        System.out.println(ins);

        OffsetDateTime odt = ins.atOffset(ZoneOffset.ofHours(8));
        System.out.println(odt);

        System.out.println(ins.getNano());

        Instant ins2 = Instant.ofEpochSecond(5);
        System.out.println(ins2);
    }

    //1. LocalDate、LocalTime、LocalDateTime
    @Test
    public void test1(){
        LocalDateTime ldt = LocalDateTime.now();
        System.out.println(ldt);

        LocalDateTime ld2 = LocalDateTime.of(2016, 11, 21, 10, 10, 10);
        System.out.println(ld2);

        LocalDateTime ldt3 = ld2.plusYears(20);
        System.out.println(ldt3);

        LocalDateTime ldt4 = ld2.minusMonths(2);
        System.out.println(ldt4);

        System.out.println(ldt.getYear());
        System.out.println(ldt.getMonthValue());
        System.out.println(ldt.getDayOfMonth());
        System.out.println(ldt.getHour());
        System.out.println(ldt.getMinute());
        System.out.println(ldt.getSecond());
    }
}
```



# 6. 接口中的默认方法与静态方法

## (1) 接口中的默认方法
Java8 中允许接口中包含具有具体实现的方法，该方法称为“默认方法”，默认方法使用 default 关键字修饰。

例如：

``` java
interface MyFun<T>{
    T func(int a);
    default String getName(){
        return "hello java8!";
    }
}
```

**接口默认方法的” 类优先” 原则**

若一个接口中定义了一个默认方法，而另外一个父类或接口中又定义了一个同名的方法时：

1. 选择父类中的方法。如果一个父类提供了具体的实现，那么接口中具有相同名称和参数的默认方法会被忽略。
2. 接口冲突。如果一个父接口提供一个默认方法，而另一个接口也提供了一个具有相同名称和参数列表的方法（不管方法是否是默认方法）， 那么必须覆盖该方法来解决冲突。

代码演示：
``` java
interface MyFunc{
    default String getName(){
        return "hello java8!";
    }
}

interface Named{
    default String getName(){
        return "hello world!";
    }
}

class MyClass implements MyFunc, Named{

    @Override
    public String getName() {
        // TODO Auto-generated method stub
        return Named.super.getName();//如果覆盖MyFunc接口方法，则为MyFunc.super.getName();
    }
}
```
## (2) 接口中的静态方法
Java8 中，接口中允许添加静态方法。

例如：
``` java
interface Named{
    public Integer myFun();

    default String getName(){
        return "hello world!";
    }

    static void show(){
        System.out.println("hello Lambda!");
    }
}
```
``` java
public class TestDefaultInterface {

    public static void main(String[] args) {
        SubClass sc = new SubClass();
        System.out.println(sc.getName());

        MyInterface.show();
    }
}
```



# 7. 其他新特性

## (1) Optional 类
`Optional<T>` 类：`java.util.Optional` 是一个容器类，代表一个值存在或不存在，原来用 null 表示一个值不存在，现在 Optional 可以更好的表达这个概念，并且可以避免空指针异常。

常用方法：

``` xml
Optional.of(T t) : 创建一个 Optional 实例
Optional.empty() : 创建一个空的 Optional 实例
Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例
isPresent() : 判断是否包含值
orElse(T t) : 如果调用对象包含值，返回该值，否则返回t
orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值
map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
flatMap(Function mapper):与 map 类似，要求返回值必须是Optional
```

Man 类：
``` java
public class Man {

    private Godness god;

    public Man() {
    }

    public Man(Godness god) {
        this.god = god;
    }

    public Godness getGod() {
        return god;
    }

    public void setGod(Godness god) {
        this.god = god;
    }

    @Override
    public String toString() {
        return "Man [god=" + god + "]";
    }
}
```
Godness 类：
``` java
public class Godness {

    private String name;

    public Godness() {
    }

    public Godness(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Godness [name=" + name + "]";
    }
}
```
NewMan 类：
``` java
//注意：Optional 不能被序列化
public class NewMan {

    private Optional<Godness> godness = Optional.empty();

    private Godness god;

    public Optional<Godness> getGod(){
        return Optional.of(god);
    }

    public NewMan() {
    }

    public NewMan(Optional<Godness> godness) {
        this.godness = godness;
    }

    public Optional<Godness> getGodness() {
        return godness;
    }

    public void setGodness(Optional<Godness> godness) {
        this.godness = godness;
    }

    @Override
    public String toString() {
        return "NewMan [godness=" + godness + "]";
    }
}
```
测试：
``` java
/*
 * 一、Optional 容器类：用于尽量避免空指针异常
 * 	Optional.of(T t) : 创建一个 Optional 实例
 * 	Optional.empty() : 创建一个空的 Optional 实例
 * 	Optional.ofNullable(T t):若 t 不为 null,创建 Optional 实例,否则创建空实例
 * 	isPresent() : 判断是否包含值
 * 	orElse(T t) :  如果调用对象包含值，返回该值，否则返回t
 * 	orElseGet(Supplier s) :如果调用对象包含值，返回该值，否则返回 s 获取的值
 * 	map(Function f): 如果有值对其处理，并返回处理后的Optional，否则返回 Optional.empty()
 * 	flatMap(Function mapper):与 map 类似，要求返回值必须是Optional
 */
public class TestOptional {

    @Test
    public void test4(){
        Optional<Employee> op = Optional.of(new Employee(101, "张三", 18, 9999.99));

        Optional<String> op2 = op.map(Employee::getName);
        System.out.println(op2.get());

        Optional<String> op3 = op.flatMap((e) -> Optional.of(e.getName()));
        System.out.println(op3.get());
    }

    @Test
    public void test3(){
        Optional<Employee> op = Optional.ofNullable(new Employee());

        if(op.isPresent()){
            System.out.println(op.get());
        }

        Employee emp = op.orElse(new Employee("张三"));
        System.out.println(emp);

        Employee emp2 = op.orElseGet(() -> new Employee());
        System.out.println(emp2);
    }

    @Test
    public void test2(){
        /*Optional<Employee> op = Optional.ofNullable(null);
		System.out.println(op.get());*/

        //		Optional<Employee> op = Optional.empty();
        //		System.out.println(op.get());
    }

    @Test
    public void test1(){
        Optional<Employee> op = Optional.of(new Employee());
        Employee emp = op.get();
        System.out.println(emp);
    }

    @Test
    public void test5(){
        Man man = new Man();

        String name = getGodnessName(man);
        System.out.println(name);
    }

    //需求：获取一个男人心中女神的名字
    public String getGodnessName(Man man){
        if(man != null){
            Godness g = man.getGod();

            if(g != null){
                return g.getName();
            }
        }

        return "苍老师";
    }

    //运用 Optional 的实体类
    @Test
    public void test6(){
        Optional<Godness> godness = Optional.ofNullable(new Godness("林志玲"));

        Optional<NewMan> op = Optional.ofNullable(new NewMan(godness));
        String name = getGodnessName2(op);
        System.out.println(name);
    }

    public String getGodnessName2(Optional<NewMan> man){
        return man.orElse(new NewMan())
            .getGodness()
            .orElse(new Godness("苍老师"))
            .getName();
    }
}
```

## (2) 重复注解与类型注解
Java8 对注解处理提供了两点改进：可重复的注解及可用于类型的注解。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219230306.png)