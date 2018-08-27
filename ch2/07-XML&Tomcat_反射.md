HelloMyServlet 类：

``` java
public class HelloMyServlet {
	public void add(){
		System.out.println("空参add方法");
	}
	
	public void add(int i,int j){
		System.out.println("带有连个add方法");
		System.out.println(i+j);
	}
	
	public int add(int i){
		return i+10;
	}
}
```

正常使用：

``` java
@Test
public void f1(){
    //调用helloMyServlet中的方法
    HelloMyServlet a = new HelloMyServlet();
    a.add();
    a.add(10, 20);
}
```



现在可以使用反射：

``` java
@Test
public void f2() throws Exception{
    Class clazz = Class.forName("com.itheima.HelloMyServlet");
    //通过字节码对象创建一个实例对象(相当于调用空参的构造器)
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    a.add();
}

@Test
//调用空参的add方法
public void f3() throws Exception{
    Class clazz = Class.forName("com.itheima.HelloMyServlet");
    //通过字节码对象创建一个实例对象(相当于调用空参的构造器)
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    //获取方法对象
    Method method = clazz.getMethod("add");
    //让方法对象执行   obj调用这个方法的实例,相当于a  ,args是该方法执行时候所需要的参数   a.add()
    method.invoke(a);//相当于 a.add()
}

@Test
//调用有两个参数的add方法
public void f4()throws Exception{
    //获取class对象
    //Class clazz = Class.forName("com.itheima.HelloMyServlet");
    Class clazz=HelloMyServlet.class;
    //通过clazz对象创建一个实例
    HelloMyServlet a =(HelloMyServlet) clazz.newInstance();
    //获取有两个参数的add方法
    Method m = clazz.getMethod("add",int.class,int.class);
    //执行方法
    m.invoke(a, 10,30);//相当于 a.add(10,30);
}
```

