
# 0. 反射

Reflection（反射）是被视为动态语言的关键，反射机制允许程序在执行期借助于ReflectionAPI 取得任何类的内部信息，并能直接操作任意对象的内部属性及方法

Java 反射机制提供的功能：

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的成员变量和方法
- 生成动态代理

类的加载过程：当程序主动使用某个类时，如果该类还未被加载到内存中，则系统会通过如下三个步骤来对该类进行初始化。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219215228.png)



# 1. 如何创建Class的实例

（1）过程：源文件经过编译（`javac.exe`）以后，得到一个或多个 `.class` 文件。`.class` 文件经过运行 `java.exe` 这步，就需要进行类的加载（通过 JVM 的类的加载器），记载到内存中的缓存。每一个放入缓存中的 `.class` 文件就是一个 Class 的实例。

（2）Class 的一个对象，对应着一个运行时类。相当于一个运行时类本身充当了 Class 的一个实例。

（3）java.lang.Class 是反射的源头。  接下来涉及到反射的类都在 java.lang.reflect 子包下。如：Field  Method Constructor  Type Package…当通过 Class 的实例调用 getMethods() --> Method , getConstructors() --> Constructor

（4）实例化 Class 的方法(三种):
``` java
// 1.调用运行时类的.class属性
Class clazz1 = Person.class;
System.out.println(clazz1);

Class clazz2 = Creator.class;
System.out.println(clazz2);
// 2.通过运行时类的对象，调用其getClass()方法
Person p = new Person();
Class clazz3 = p.getClass();
System.out.println(clazz3);
// 3.调用Class的静态方法forName(String className)。此方法报ClassNotFoundException
String className = "com.atguigu.java.Person";
Class clazz4 = Class.forName(className);
System.out.println(clazz4);
```



# 2. Class的实例：应用一

**应用一：可以创建对应的运行时类的对象(重点)**
``` java
//获取运行时类的对象：方法一
@Test
public void test1() throws Exception{
    Class clazz = Class.forName("com.atguigu.review.Animal");
    Object obj = clazz.newInstance();
    Animal a = (Animal)obj;
    System.out.println(a);
}
//调用指定的构造器创建运行时类的对象
@Test
public void test2() throws Exception{
    Class clazz = Animal.class;
    Constructor cons = clazz.getDeclaredConstructor(String.class,int.class);
    cons.setAccessible(true);
    Animal a = (Animal)cons.newInstance("Tom",10);
    System.out.println(a);
}
```


# 3. Class的实例：应用二

**应用二：获取对应的运行时类的完整的类的结构：属性、方法、构造器、包、父类、接口、泛型、注解、异常、内部类等等。**

如 `Method[] m1 = clazz.getMethods()`：获取到对应的运行时类中声明的权限为public的方法（包含其父类中的声明的 public）

`Method[] m2 = clazz.getDeclaredMethods()`：获取到对应的运行时类中声明的所有的方法（①任何权限修饰符修饰的都能获取②不含父类中的）



# 4. Class的实例：应用三

**应用三：调用对应的运行时类中指定的结构（某个指定的属性、方法、构造器）(重点)**

``` java
//调用指定属性
@Test
public void test3() throws Exception{
    Class clazz = Class.forName("com.atguigu.review.Animal");
    Object obj = clazz.newInstance();
    Animal a = (Animal)obj;
    //调用非public的属性
    Field f1 = clazz.getDeclaredField("name");
    f1.setAccessible(true);
    f1.set(a, "Jerry");
    //调用public的属性
    Field f2 = clazz.getField("age");
    f2.set(a, 9);
    System.out.println(f2.get(a));
    System.out.println(a);
    //调用static的属性
    Field f3 = clazz.getDeclaredField("desc");
    System.out.println(f3.get(null));
}

//调用指定的方法
@Test
public void test4() throws Exception{
    Class clazz = Class.forName("com.atguigu.review.Animal");
    Object obj = clazz.newInstance();
    Animal a = (Animal)obj;

    //调用非public的方法
    Method m1 = clazz.getDeclaredMethod("getAge");
    m1.setAccessible(true);
    int age = (Integer)m1.invoke(a);
    System.out.println(age);
    //调用public的方法
    Method m2 = clazz.getMethod("show", String.class);
    Object returnVal = m2.invoke(a,"金毛");
    System.out.println(returnVal);
    //调用static的方法
    Method m3 = clazz.getDeclaredMethod("info");
    m3.setAccessible(true);
    //		m3.invoke(Animal.class);
    m3.invoke(null);	
}
```


# 5. 动态代理：反射的应用

**代理设计模式的原理： **

> 使用一个代理将对象包装起来，然后用该代理对象取代原始对象。任何对原始对象的调用都要通过代理，代理对象决定是否以及何时将方法调用转到原始对象上。

**（1）静态代理**

要求被代理类和代理类同时实现相应的一套接口；通过代理类的对象调用重写接口的方法时，实际上执行的是被代理类的同样的方法的调用。

**静态代理模式：**

``` java
//静态代理模式
//接口
interface ClothFactory {
	void productCloth();
}

// 被代理类
class NikeColthFactory implements ClothFactory {

	@Override
	public void productCloth() {
		System.out.println("Nike工厂生产了一批衣服");
	}
}

// 代理类
class ProxFactory implements ClothFactory {
	ClothFactory cf;

	public ProxFactory(ClothFactory cf) {
		this.cf = cf;
	}

	@Override
	public void productCloth() {
		System.out.println("代理类开始执行，收代理费$1000");
		cf.productCloth();
	}
}

public class TestClothProduct {
	public static void main(String[] args) {
		NikeColthFactory nike = new NikeColthFactory();// 创建被代理类的对象
		ProxFactory prxoy = new ProxFactory(nike);// 创建代理类的对象
		prxoy.productCloth();
	}
}
```
**（2）动态代理**

在程序运行时，根据被代理类及其实现的接口，动态的创建一个代理类。当调用代理类的实现的抽象方法时，就发起对被代理类同样方法的调用。

涉及到的技术点：

- ①提供一个实现了 InvocationHandler 接口实现类，并重写其 invoke() 方法			

- ②`Proxy.newProxyInstance(obj.getClass().getClassLoader(),obj.getClass().getInterfaces(),h);`

  obj：被代理类对象 ；

   h：实现了 InvocationHandler 接口的实现类的对象

**动态代理的使用：**

``` java
//动态代理的使用，体会反射是动态语言的关键
interface Subject {
    void action();
}

// 被代理类
class RealSubject implements Subject {
    public void action() {
        System.out.println("我是被代理类，记得要执行我哦！么么~~");
    }
}

class MyInvocationHandler implements InvocationHandler {
    Object obj;// 实现了接口的被代理类的对象的声明

    // ①给被代理的对象实例化②返回一个代理类的对象
    public Object blind(Object obj) {
        this.obj = obj;
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj
                                      .getClass().getInterfaces(), this);
    }
    //当通过代理类的对象发起对被重写的方法的调用时，都会转换为对如下的invoke方法的调用
    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable {
        //method方法的返回值时returnVal
        Object returnVal = method.invoke(obj, args);
        return returnVal;
    }

}

public class TestProxy {
    public static void main(String[] args) {
        //1.被代理类的对象
        RealSubject real = new RealSubject();
        //2.创建一个实现了InvacationHandler接口的类的对象
        MyInvocationHandler handler = new MyInvocationHandler();
        //3.调用blind()方法，动态的返回一个同样实现了real所在类实现的接口Subject的代理类的对象。
        Object obj = handler.blind(real);
        Subject sub = (Subject)obj;//此时sub就是代理类的对象

        sub.action();//转到对InvacationHandler接口的实现类的invoke()方法的调用

        //再举一例
        NikeClothFactory nike = new NikeClothFactory();
        ClothFactory proxyCloth = (ClothFactory)handler.blind(nike);//proxyCloth即为代理类的对象
        proxyCloth.productCloth();



    }
}
```


# 6. 动态代理与AOP

动态代理与 AOP：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219220115.png)

AOP 代理的方法：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219220133.png)

``` java
interface Human {
    void info();

    void fly();
}

// 被代理类
class SuperMan implements Human {
    public void info() {
        System.out.println("我是超人！我怕谁！");
    }

    public void fly() {
        System.out.println("I believe I can fly!");
    }
}

class HumanUtil {
    public void method1() {
        System.out.println("=======方法一=======");
    }

    public void method2() {
        System.out.println("=======方法二=======");
    }
}

class MyInvocationHandler implements InvocationHandler {
    Object obj;// 被代理类对象的声明

    public void setObject(Object obj) {
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
        throws Throwable {
        HumanUtil h = new HumanUtil();
        h.method1();

        Object returnVal = method.invoke(obj, args);

        h.method2();
        return returnVal;
    }
}

class MyProxy {
    // 动态的创建一个代理类的对象
    public static Object getProxyInstance(Object obj) {
        MyInvocationHandler handler = new MyInvocationHandler();
        handler.setObject(obj);

        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj
                                      .getClass().getInterfaces(), handler);
    }
}

public class TestAOP {
    public static void main(String[] args) {
        SuperMan man = new SuperMan();//创建一个被代理类的对象
        Object obj = MyProxy.getProxyInstance(man);//返回一个代理类的对象
        Human hu = (Human)obj;
        hu.info();//通过代理类的对象调用重写的抽象方法

        System.out.println();

        hu.fly();

        //*********
        NikeClothFactory nike = new NikeClothFactory();
        Object obj1 = MyProxy.getProxyInstance(nike);
        ClothFactory cloth = (ClothFactory)obj1;
        cloth.productCloth();
    }
}
```
效果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219220227.png)

反射例子：
``` java
public class TestReflection {

    //调用指定的方法
    @Test
    public void test4() throws Exception{
        Class clazz = Class.forName("com.atguigu.review.Animal");
        Object obj = clazz.newInstance();
        Animal a = (Animal)obj;

        //调用非public的方法
        Method m1 = clazz.getDeclaredMethod("getAge");
        m1.setAccessible(true);
        int age = (Integer)m1.invoke(a);
        System.out.println(age);
        //调用public的方法
        Method m2 = clazz.getMethod("show", String.class);
        Object returnVal = m2.invoke(a,"金毛");
        System.out.println(returnVal);
        //调用static的方法
        Method m3 = clazz.getDeclaredMethod("info");
        m3.setAccessible(true);
        //		m3.invoke(Animal.class);
        m3.invoke(null);

    }

    //调用指定属性
    @Test
    public void test3() throws Exception{
        Class clazz = Class.forName("com.atguigu.review.Animal");
        Object obj = clazz.newInstance();
        Animal a = (Animal)obj;
        //调用非public的属性
        Field f1 = clazz.getDeclaredField("name");
        f1.setAccessible(true);
        f1.set(a, "Jerry");
        //调用public的属性
        Field f2 = clazz.getField("age");
        f2.set(a, 9);`
            System.out.println(f2.get(a));
        System.out.println(a);
        //调用static的属性
        Field f3 = clazz.getDeclaredField("desc");
        System.out.println(f3.get(null));
    }

    //调用指定的构造器创建运行时类的对象
    @Test
    public void test2() throws Exception{
        Class clazz = Animal.class;
        Constructor cons = clazz.getDeclaredConstructor(String.class,int.class);
        cons.setAccessible(true);
        Animal a = (Animal)cons.newInstance("Tom",10);
        System.out.println(a);
    }

    //获取运行时类的对象：方法一
    @Test
    public void test1() throws Exception{
        Class clazz = Class.forName("com.atguigu.review.Animal");
        Object obj = clazz.newInstance();
        Animal a = (Animal)obj;
        System.out.println(a);
    }
}
```

如何获取：
``` java
public class TestReflection {
    //关于类的加载器：ClassLoader
    @Test
    public void test5() throws Exception{
        ClassLoader loader1 = ClassLoader.getSystemClassLoader();
        System.out.println(loader1);

        ClassLoader loader2 = loader1.getParent();
        System.out.println(loader2);

        ClassLoader loader3 = loader2.getParent();
        System.out.println(loader3);

        Class clazz1 = Person.class;
        ClassLoader loader4 = clazz1.getClassLoader();
        System.out.println(loader4);

        String className = "java.lang.String";
        Class clazz2 = Class.forName(className);
        ClassLoader loader5 = clazz2.getClassLoader();
        System.out.println(loader5);

        //掌握如下
        //法一：
        ClassLoader loader = this.getClass().getClassLoader();
        InputStream is = loader.getResourceAsStream("com\\atguigu\\java\\jdbc.properties");
        //法二：
        //		FileInputStream is = new FileInputStream(new File("jdbc1.properties"));

        Properties pros = new Properties();
        pros.load(is);
        String name = pros.getProperty("user");
        System.out.println(name);

        String password = pros.getProperty("password");
        System.out.println(password);

    }
    //如何获取Class的实例（3种）
    @Test
    public void test4() throws ClassNotFoundException{
        //1.调用运行时类本身的.class属性
        Class clazz1 = Person.class;
        System.out.println(clazz1.getName());

        Class clazz2 = String.class;
        System.out.println(clazz2.getName());

        //2.通过运行时类的对象获取
        Person p = new Person();
        Class clazz3 = p.getClass();
        System.out.println(clazz3.getName());

        //3.通过Class的静态方法获取.通过此方式，体会一下，反射的动态性。
        String className = "com.atguigu.java.Person";


        Class clazz4 = Class.forName(className);
        //		clazz4.newInstance();
        System.out.println(clazz4.getName());

        //4.（了解）通过类的加载器
        ClassLoader classLoader = this.getClass().getClassLoader();
        Class clazz5 = classLoader.loadClass(className);
        System.out.println(clazz5.getName());

        System.out.println(clazz1 == clazz3);//true
        System.out.println(clazz1 == clazz4);//true
        System.out.println(clazz1 == clazz5);//true
    }

    /*
	 * java.lang.Class:是反射的源头。
	 * 我们创建了一个类，通过编译（javac.exe）,生成对应的.class文件。之后我们使用java.exe加载（JVM的类加载器完成的）
	 * 此.class文件，此.class文件加载到内存以后，就是一个运行时类，存在在缓存区。那么这个运行时类本身就是一个Class的实例！
	 * 1.每一个运行时类只加载一次！
	 * 2.有了Class的实例以后，我们才可以进行如下的操作：
	 *     1）*创建对应的运行时类的对象
	 *     2）获取对应的运行时类的完整结构（属性、方法、构造器、内部类、父类、所在的包、异常、注解、...）
	 *     3）*调用对应的运行时类的指定的结构(属性、方法、构造器)
	 *     4）反射的应用：动态代理
	 */
    @Test
    public void test3(){
        Person p = new Person();
        Class clazz = p.getClass();//通过运行时类的对象，调用其getClass()，返回其运行时类。
        System.out.println(clazz);
    }


    //有了反射，可以通过反射创建一个类的对象，并调用其中的结构
    @Test
    public void test2() throws Exception{
        Class clazz = Person.class;

        //		Class clazz1 = String.class;

        //1.创建clazz对应的运行时类Person类的对象
        Person p = (Person)clazz.newInstance();
        System.out.println(p);
        //2.通过反射调用运行时类的指定的属性
        //2.1
        Field f1 = clazz.getField("name");
        f1.set(p,"LiuDeHua");
        System.out.println(p);
        //2.2
        Field f2 = clazz.getDeclaredField("age");
        f2.setAccessible(true);
        f2.set(p, 20);
        System.out.println(p);

        //3.通过反射调用运行时类的指定的方法
        Method m1 = clazz.getMethod("show");
        m1.invoke(p);

        Method m2 = clazz.getMethod("display",String.class);
        m2.invoke(p,"CHN");

    }

    //在有反射以前，如何创建一个类的对象，并调用其中的方法、属性
    @Test
    public void test1(){
        Person p = new Person();
        //		Person p1 = new Person();
        p.setAge(10);
        p.setName("TangWei");
        System.out.println(p);
        p.show();
        //		p.display("HK");
    }
}
```