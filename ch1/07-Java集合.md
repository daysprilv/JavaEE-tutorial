
# 1. 集合概述

对象的存储：

- ①数组（基本数据类型  & 引用数据类型）  
- ②集合（引用数据类型）

数组存储数据的弊端：长度一旦初始化以后，就不可变；

真正给数组元素赋值的个数没有现成的方法可用。

Java 集合的概述：

- 一方面，面向对象语言对事物的体现都是以对象的形式，为了方便对多个对象的操作，就要对对象进行存储。另一方面，使用 Array 存储对象方面具有一些弊端，而 Java 集合就像一种容器，可以动态地把多个对象的引用放入容器中。

- Java 集合类可以用于存储数量不等的多个对象，还可用于保存具有映射关系的关联数组。

- Java 集合可分为 Collection 和 Map 两种体系

  - Collection 接口：
    - Set：元素无序、不可重复的集合，类似高中的“集合”
    - List：元素有序，可重复的集合，”动态”数组

  - Map 接口：具有映射关系“key-value对”的集合类似于高中的“函数”y=f（x）（x1，y1）（x2，y2）



# 2. Collection接口

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219200057.png)

Collection 接口 ：

``` java
方法：
①add(Object obj),addAll(Collection coll),size(),clear(),isEmpty();

②remove(Object obj),removeAll(Collection coll),retainAll(Collection coll),equals(Object obj),contains(Object obj),containsAll(Collection coll),hashCode()

③iterator(),toArray();
```

Collection 方法测试：
``` java
public class TestCollection {
    @Test
    public void testCollection3() {
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(new String("AA"));
        coll.add(new Date());
        coll.add("BB");
        coll.add(new Person("MM", 23));

        Collection coll1 = new ArrayList();
        coll1.add(123);
        coll1.add(new String("AA"));
        // 10.removeAll(Collection coll):从当前集合中删除包含在coll中的元素。
        coll.removeAll(coll1);
        System.out.println(coll);
        //11.equals(Object obj):判断集合中的所有元素是否完全相同
        Collection coll2 = new ArrayList();
        coll2.add(123);
        coll2.add(new String("AA1"));
        System.out.println(coll1.equals(coll2));
        //12.hashCode():
        System.out.println(coll.hashCode());
        System.out.println();
        //13.toArray() :将集合转化为数组
        Object[] obj = coll.toArray();
        for(int i = 0;i < obj.length;i++){
            System.out.println(obj[i]);
        }
        System.out.println();
        //14.iterator():返回一个Iterator接口实现类的对象,进而实现集合的遍历！
        Iterator iterator = coll.iterator();
        //方式一：不用
        /*System.out.println(iterator.next());
		System.out.println(iterator.next());
		System.out.println(iterator.next());*/
        //方式二：不用
        //		for(int i = 0;i < coll.size();i++){
        //			System.out.println(iterator.next());
        //		}
        //方式三：使用
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }

    @Test
    public void testCollection2() {
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(new String("AA"));
        coll.add(new Date());
        coll.add("BB");
        // Person p = new Person("MM",23);
        coll.add(new Person("MM", 23));
        System.out.println(coll);
        // 6.contains(Object obj):判断集合中是否包含指定的obj元素。如果包含，返回true，反之返回false
        // 判断的依据：根据元素所在的类的equals()方法进行判断
        // 明确：如果存入集合中的元素是自定义类的对象。要求：自定义类要重写equals()方法！
        boolean b1 = coll.contains(123);
        b1 = coll.contains(new String("AA"));
        System.out.println(b1);
        boolean b2 = coll.contains(new Person("MM", 23));
        System.out.println(b2);
        // 7.containsAll(Collection coll):判断当前集合中是否包含coll中所有的元素
        Collection coll1 = new ArrayList();
        coll1.add(123);
        coll1.add(new String("AA"));

        boolean b3 = coll.containsAll(coll1);
        System.out.println("#" + b3);
        coll1.add(456);
        // 8.retainAll(Collection coll):求当前集合与coll的共有的元素，返回给当前集合
        coll.retainAll(coll1);
        System.out.println(coll);
        // 9.remove(Object obj):删除集合中的obj元素。若删除成功，返回true。否则，返回false
        boolean b4 = coll.remove("BB");
        System.out.println(b4);

    }

    @Test
    public void testCollection1() {
        Collection coll = new ArrayList();
        // 1.size():返回集合中元素的个数
        System.out.println(coll.size());
        // 2.add(Object obj):向集合中添加一个元素
        coll.add(123);
        coll.add("AA");
        coll.add(new Date());
        coll.add("BB");
        System.out.println(coll.size());
        // 3.addAll(Collection coll):将形参coll中包含的所有元素添加到当前集合中
        Collection coll1 = Arrays.asList(1, 2, 3);
        coll.addAll(coll1);
        System.out.println(coll.size());
        // 查看集合元素
        System.out.println(coll);
        // 4.isEmpty():判断集合是否为空
        System.out.println(coll.isEmpty());
        // 5.clear():清空集合元素
        coll.clear();
        System.out.println(coll.isEmpty());
    }
}
```
## (1) List接口

**存储有序的，可以重复的元素，相当于“动态”数组**

``` java
新增的方法： 
 	删除remove(int index) 
 	修改set(int index,Object obj) 
    获取get(int index)
    插入add(int index,Object obj)

添加进List集合中的元素（或对象）所在的类一定要重写equals()方法
	- ArrayList（主要的实现类）
	- LinkedList（更适用于频繁的插入、删除操作）
	- Vector（古老的实现类、线程安全的，但效率要低于ArrayList）
```



## (2) Set接口：

**存储无序的，不可重复的元素。---相当于高中的“集合”概念**

Set 使用的方法基本上都是 Collection 接口下定义的。 

添加进 Set 集合中的元素所在的类一定要重写 equals() 和 hashCode()。要求重写 equals() 和 hashCode() 方法保持一致。

1. 无序性：无序性！= 随机性。真正的无序性，指的是元素在底层存储的位置是无序的。 
2. 不可重复性：当向 Set 中添加进相同的元素的时候，后面的这个不能添加进去。

HashSet（主要的实现类）

LinkedHashSet（是 HashSet 的子类，当我们遍历集合元素时，是按照添加进去的顺序实现的；频繁的遍历，较少的添加、插入操作建议选择此）

TreeSet（可以按照添加进集合中的元素的指定属性进行排序）

- 要求 TreeSet 添加进的元素必须是同一个类的！

- 两种排序方式： 

  - 自然排序： 

    ①要求添加进 TreeSet 中的元素所在的类 implements Comparable 接口 

    ②重写 compareTo(Object obj)，在此方法内指明按照元素的哪个属性进行排序 

    ③向 TreeSet 中添加元素即可。若不实现此接口，会报运行时异常

  - 定制排序： 

    ①创建一个实现 Comparator 接口的实现类的对象。在实现类中重写 Comparator 的 compare(Object o1, Object o2 )方法 

    ②在此 compare() 方法中指明按照元素所在类的哪个属性进行排序 

    ③将此实现 Comparator 接口的实现类的对象作为形参传递给 TreeSet 的构造器中 

    ④向 TreeSet 中添加元素即可。若不实现此接口，会报运行时异常 

    要求重写的 compareTo() 或者 compare() 方法与 equals() 和 hashCode() 方法保持一致。



# 3. Map接口

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219200840.png)

- Map 与 Collection 并列存在。用于保存具有映射关系的数据：Key-Value 
- Map 中的 key 和 value 都可以是任何引用类型的数据
- Map 中的 key 用 Set 来存放，不允许重复，即同一个 Map 对象所对应的类，须重写 hashCode() 和 equals() 方法
- 常用 String 类作为 Map 的“键”
- key 和 value 之间存在单向一对一关系，即通过指定的key总能找到唯一的、确定的 value

存储“键-值”对的数据，相当于高中的函数 y = f(x)， (x1, y1)  (x2, y2)

key 是不可重复的，使用 Set 存放。value 可以重复的，使用 Collection 来存放的。一个 key-value 对构成一个 entry(Map.Entry)，entry 使用 Set 来存放。

添加、修改 put(Object key,Object value) 删除 remove(Object key) 获取 get(Object key) size() / keySet() values() entrySet()

- HashMap：主要的实现类，可以添加 null 键，null 值

- LinkedHashMap：是 HashMap 的子类，可以按照添加进 Map 的顺序实现遍历

- TreeMap：需要按照 key 所在类的指定属性进行排序。要求 key 是同一个类的对象。对 key 考虑使用自然排序 或 定制排序

- Hashtable：是一个古老的实现类，线程安全的，不可以添加 null 键，null 值不建议使用。

  - 子类：Properties：常用来处理属性文件 

  - 附：Properties 的使用 

    ``` java
    Properties pros = new Properties(); 
    pros.load(new FileInputStream(new File(“jdbc.properties”))); 
    String user = pros.getProperty(“user”); 
    System.out.println(user); 
    String password = pros.getProperty(“password”); 
    System.out.println(password);
    ```

- LinkedHashMap：是HashMap的子类，可以按照添加进Map的顺序实现遍历
- TreeMap：需要按照key所在类的指定属性进行排序。要求key是同一个类的对象。对key考虑使用自然排序 或 定制排序
- Hashtable：是一个古老的实现类，线程安全的，不可以添加null键，null值不建议使用。
  - 子类：Properties：常用来处理属性文件

  - 附：Properties的使用

    ``` java			
    Properties pros = new Properties();
    pros.load(new FileInputStream(new File("jdbc.properties")));
    String user = pros.getProperty("user");
    System.out.println(user);
    String password = pros.getProperty("password");
    System.out.println(password);	
    ```

    
# 4. Iterator接口

用来遍历集合 Collection 元素。

迭代器测遍历测试：

``` java
public class TestIterator {
	//面试题：
	@Test
	public void testFor3(){
		String[] str = new String[]{"AA","BB","DD"};
		for(String s : str){
			s =  "MM";//此处的s是新定义的局部变量，其值的修改不会对str本身造成影响。
			System.out.println(s);
		}		
		
		for(int i = 0;i < str.length;i++){
			System.out.println(str[i]);
		}
	}
	@Test
	public void testFor2(){
		String[] str = new String[]{"AA","BB","DD"};
		for(int i = 0;i < str.length;i++){
			str[i] = i + "";
		}
		
		for(int i = 0;i < str.length;i++){
			System.out.println(str[i]);
		}
	}
	
	//***********************************************
	//使用增强for循环实现数组的遍历
	@Test
	public void testFor1(){
		String[] str = new String[]{"AA","BB","DD"};
		for(String s:str){
			System.out.println(s);
		}
	}
	
	//使用增强for循环实现集合的遍历
	@Test
	public void testFor(){
		Collection coll = new ArrayList();
		coll.add(123);
		coll.add(new String("AA"));
		coll.add(new Date());
		coll.add("BB");
		coll.add(new Person("MM", 23));
		
		for(Object i:coll){
			System.out.println(i);
		}
	}
	
	//错误的写法
	@Test
	public void test2(){
		Collection coll = new ArrayList();
		coll.add(123);
		coll.add(new String("AA"));
		coll.add(new Date());
		coll.add("BB");
		coll.add(new Person("MM", 23));
		
		Iterator i = coll.iterator();
		
		while((i.next())!= null){
			//java.util.NoSuchElementException
			System.out.println(i.next());
		}
	}
	//正确的写法：使用迭代器Iterator实现集合的遍历
	@Test
	public void test1(){
		Collection coll = new ArrayList();
		coll.add(123);
		coll.add(new String("AA"));
		coll.add(new Date());
		coll.add("BB");
		coll.add(new Person("MM", 23));
		
		Iterator i = coll.iterator();
		while(i.hasNext()){
			System.out.println(i.next());
		}
	}
}
```



# 5. Collections工具类

操作 Collection 及 Map 的工具类，大部分为 static 的方法。