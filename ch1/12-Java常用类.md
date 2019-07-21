
# 1. String类

不可变的字符序列（如：String str = "atguigu"; str += "javaEE"）底层使用 char[] 存放。String 是 final 的。

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219213317.png)

String 例子的过程图：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/20190219213346.png)

(1) 关注于 String 常用的方法

(2) String 类与基本数据类型、包装类；与字符数组、字节数组

1. 字符串 与基本数据类型、包装类之间转换 
   - ①字符串 –> 基本数据类型、包装类：调用相应的包装类的parseXxx(String str); 
   - ②基本数据类型、包装类 –> 字符串：调用字符串的重载的valueOf()方法

2. 字符串与字节数组间的转换 
   - ①字符串 –> 字节数组：调用字符串的 getBytes() 
   - ②字节数组 –> 字符串：调用字符串的构造器
3. 字符串与字符数组间的转换 
   - ①字符串 –> 字符数组：调用字符串的 toCharArray(); 
     ②字符数组 –> 字符串：调用字符串的构造器
4. String与StringBuffer的转换 
   - ①String –> StringBuffer：使用 StringBuffer 的构造器：`new StringBuffer(String str);` 
   - ②StringBuffer –> String：使用 StringBuffer 的 toString() 方法

StringBuffer类：可变的字符序列

StringBuilder类：可变的字符序列，jdk5.0 新加入的，效率更高，线程不安全。

常用的方法：

``` xml
添加：append(...)  
删除 delete(int startIndex, int endIndex)  
修改：setCharAt(int n ,char ch)  
查询：charAt(int index)		
插入:insert(int index, String str)  
反转reverse()  
长度：length()
```

注：String 类的不可变性

**String类方法：**

``` java
import org.junit.Test;

public class TestString {
	/*
	 * 1.字符串 与基本数据类型、包装类之间转换
	 * ①字符串 --->基本数据类型、包装类:调用相应的包装类的parseXxx(String str);
	 * ①基本数据类型、包装类--->字符串:调用字符串的重载的valueOf()方法
	 * 
	 * 2.字符串与字节数组间的转换
	 * ①字符串---->字节数组:调用字符串的getBytes()
	 * ②字节数组---->字符串：调用字符串的构造器
	 * 
	 * 3.字符串与字符数组间的转换
	 * ①字符串---->字符数组：调用字符串的toCharArray();
	 * ②字符数组---->字符串:调用字符串的构造器
	 */
	@Test
	public void test5(){
		//1.字符串 与基本数据类型、包装类之间转换
		String str1 = "123";
		int i = Integer.parseInt(str1);
		System.out.println(i);
		String str2 = i + "";
		str2 = String.valueOf(i);
		System.out.println(str2);
		System.out.println();
		//2.字符串与字节数组间的转换
		String str = "abcd123";
		byte[] b = str.getBytes();
		for(int j = 0;j < b.length;j++){
			System.out.println((char)b[j]);
		}
		String str3 = new String(b);
		System.out.println(str3);
		System.out.println();
		//3.字符串与字符数组间的转换
		String str4 = "abc123中国人";
		char[] c = str4.toCharArray();
		for(int j = 0;j < c.length;j++){
			System.out.println(c[j]);
		}
		String str5 = new String(c);
		System.out.println(str5);
	}
	
	/*
	 * 	public String substring(int startpoint)
		public String substring(int start,int end):返回从start开始到end结束的一个左闭右开的子串。start可以从0开始的。
		pubic String replace(char oldChar,char newChar)
		public String replaceAll(String old,String new)
		public String trim()：去除当前字符串中首尾出现的空格，若有多个，就去除多个。
		public String concat(String str):连接当前字符串与str
		public String[] split(String regex)：按照regex将当前字符串拆分，拆分为多个字符串，整体返回值为String[]

	 */
	@Test
	public void test4() {
		String str1 = "北京尚硅谷教育北京";
		String str2 = "上海尚硅谷教育";
		String str3 = str1.substring(2, 5);
		System.out.println(str3);
		System.out.println(str1);
		String str4 = str1.replace("北京", "东京");
		System.out.println(str4);// 东京尚硅谷教育东京
		System.out.println(str1);// 北京尚硅谷教育北京
		String str5 = "   abc   d  ";
		String str6 = str5.trim();
		System.out.println("----" + str6 + "----");
		System.out.println("----" + str5 + "----");
		String str7 = str1.concat(str2);
		System.out.println(str1);
		System.out.println(str7);
		System.out.println();
		
		String str8 = "abc*d-e7f-ke";
		String[] strs = str8.split("-");
		for(int i = 0;i < strs.length;i++){
			System.out.println(strs[i]);
		}

	}

	/*
	 * public int length() public char charAt(int
	 * index):返回在指定index位置的字符。index从0开始 public boolean equals(Object
	 * anObject)：比较两个字符串是否相等。相等返回true。否则返回false public int compareTo(String
	 * anotherString) public int indexOf(String s)：返回s字符串在当前字符串中首次出现的位置。若没有，返回-1
	 * public int indexOf(String s ,int
	 * startpoint)：返回s字符串从当前字符串startpoint位置开始的，首次出现的位置。 public int
	 * lastIndexOf(String s):返回s字符串最后一次在当前字符串中出现的位置。若无，返回-1 public int
	 * lastIndexOf(String s ,int startpoint) public boolean startsWith(String
	 * prefix):判断当前字符串是否以prefix开始。 public boolean endsWith(String
	 * suffix)：判断当前字符串是否以suffix结束。 public boolean regionMatches(int
	 * firstStart,String other,int otherStart ,int length):
	 * 判断当前字符串从firstStart开始的子串与另一个字符串other从otherStart开始，length长度的字串是否equals
	 */
	@Test
	public void test3() {
		String str1 = "abccdefghijkbcc";
		String str2 = "bcc";
		String str3 = "jkbcc";
		System.out.println(str2.length());
		System.out.println(str1.charAt(10));
		System.out.println(str1.equals(str2));
		System.out.println(str2.equals("abcc"));
		System.out.println(str1.compareTo(str2));
		System.out.println(str1.indexOf(str2, 5));
		System.out.println(str1.lastIndexOf(str2));
		System.out.println(str1.startsWith("abcd"));
		System.out.println(str1.regionMatches(10, str3, 0, str3.length()));

	}

	/*
	 * String:代表不可变的字符序列。底层使用char[]存放。
	 * String 是final的。
	 */
	@Test
	public void test1(){
		String str1 = "JavaEE";
		String str2 = "JavaEE";
		String str3 = new String("JavaEE");
		String str4 = "JavaEE" + "Android";
		String str5 = "Android";
		String str6 = str1 + str5;
		str5 = str5 + "Handoop";
		String str7 = str6.intern();
		String str8 = "JavaEEAndroid";
		System.out.println(str1 == str2);//true
		System.out.println(str1 == str3);//false
		System.out.println(str1.equals(str3));//true
		
		System.out.println(str4 == str6);//false
		System.out.println(str4.equals(str6));//true
		System.out.println(str7 == str4);//true
		System.out.println(str4 == str8);//true
		
		Person p1 = new Person("AA");
		Person p2 = new Person("AA");
		System.out.println("^_^"+ (p1.name == p2.name));//true
	}
}

class Person{
	String name;
	Person(String name){
		this.name = name;
	}
}
```
**String小Demo：**

``` java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/*
 * 1.模拟一个trim方法，去除字符串两端的空格。

   2.将一个字符串进行反转。将字符串中指定部分进行反转。比如将“abcdefg”反转为”abfedcg”

   3.获取一个字符串在另一个字符串中出现的次数。
            比如：获取“ab”在 “abkkcadkabkebfkabkskab”
            中出现的次数

	4.获取两个字符串中最大相同子串。比如：
   str1 = "abcwerthelloyuiodef";str2 = "cvhellobnm"
   
   5.对字符串中字符进行自然顺序排序。

练习：I am a student!   写一个方法：实现输出 !student a am I

 */
public class StringDemo {
	public static void main(String[] args) {
		String str = "   abc  de  ";
		//str = "    ";
		String str1 = myTrim(str);
		System.out.println(str1);
		
		String str2 = "abcdefg";
		String str3 = reverseString(str2,2,5);
		String str4 = reverseString1(str2,2,5);
		System.out.println(str3);//abfedcg
		System.out.println(str4);
		
		int i = getTime("abkkcadkabkebfkabkskab","abk");
		System.out.println(i);
		
		List<String> strs5 = getMaxSubString("abcwerthelloyuiodef","abcwecvhellobnm");
		System.out.println(strs5);
		
		String str6 = "aediewfn";
		String str7 = sort(str6);
		System.out.println(str7);
	}
	
	//5.对字符串中字符进行自然顺序排序。
	public static String sort(String str){
		char[] c = str.toCharArray();
		Arrays.sort(c);
		return new String(c);
	}
	
	//4.获取两个字符串中最大相同子串。
	public static List<String> getMaxSubString(String str1,String str2){
		String maxStr = (str1.length() > str2.length())? str1 : str2;
		String minStr = (str1.length() < str2.length())? str1 : str2;
		int len = minStr.length();
		List<String> list = new ArrayList<>();
		for(int i = 0;i < len;i++){
			for(int x = 0,y = len - i;y <= len;x++,y++){
				String str = minStr.substring(x, y);
				if(maxStr.contains(str)){
					list.add(str);
				}
			}
			if(list.size() != 0){
				return list;
			}
		}
		return null;
	}
	
	//3.获取一个字符串在另一个字符串中出现的次数。判断str2在str1中出现的次数
	public static int getTime(String str1,String str2){
		int count = 0;
		int len;
		while((len = str1.indexOf(str2)) != -1){
			count++;
			str1 = str1.substring(len + str2.length());
		}
		
		return count;
	}
	
	
	//将一个字符串进行反转。将字符串中指定部分进行反转。(法二)  在考虑使用StringBuffer将此算法优化！
	public static String reverseString1(String str,int start,int end){
		String str1 = str.substring(0, start);
		for(int i = end;i >= start;i--){
			char c = str.charAt(i);
			str1 += c;
		}
		
		str1 += str.substring(end + 1);
		return str1;
	}
	
	
	//2.将一个字符串进行反转。将字符串中指定部分进行反转。比如将“abcdefg”反转为”abfedcg”
	public static String reverseString(String str,int start,int end){
		char[] c = str.toCharArray();//字符串--->字符数组
		return reverseArray(c,start,end);
		
	}
	public static String reverseArray(char[] c,int start,int end){
		for(int i = start,j = end;i < j;i++,j--){
			char temp = c[i];
			c[i] = c[j];
			c[j] = temp;
		}
		//字符数组--->字符串
		return new String(c);
	}
	
	
	//1.模拟一个trim方法，去除字符串两端的空格。
	public static String myTrim(String str){
		int start = 0;
		int end = str.length() - 1;
		while(start < end && str.charAt(start) == ' '){
			start++;
		}
		while(start < end && str.charAt(end) == ' '){
			end--;
		}
		
		return str.substring(start, end + 1);
	}
}
```
**实现字符串反转的三种方法：**

``` java
public class TestString {
	public static void main(String[] args) {
		String str1 = reverse("helloworld");
		System.out.println(str1);
		
		String str2 = reverse1("helloworld");
		System.out.println(str2);
		
		String str3 = reverse2("helloworld");
		System.out.println(str3);
	}
	
	//方法一
	public static String reverse(String str){
		char[] c = str.toCharArray();
		for(int x=0, y = c.length-1;x < y;x++,y--){
			char temp = c[x];
			c[x] = c[y];
			c[y] = temp;
		}
		return new String(c);
	}
	
	//方法二
	public static String reverse1(String str){
		StringBuffer sb = new StringBuffer(str);
		StringBuffer sb1 = sb.reverse();
		return sb1.toString();
	}
	
	//方法三
	public static String reverse2(String str){
		StringBuffer sb = new StringBuffer();
		for (int i = str.length()-1;i >= 0; i--) {
			sb.append(str.charAt(i));
		}
		return sb.toString();
	}
}
```



# 2. 时间、日期类

(1) System类

currentTimeMillis()：返回当前时间的 long 型值。此 long 值是从1970年1月1日0点0分00秒开始到当前的毫秒数。此方法常用来计算时间差。

(2) Date 类：java.util.Date

``` java
Date  d = new Date();//返回当前时间的Date：Mon May 12 15:17:01 CST 2014

Date d1 = new Date(15231512541241L);//返回形参处此long型值对应的日期

//getTime()：返回当前日期对应的long型值。 toString()
```

(3) SimpleDateFormat：`java.text.SimpleDateFormat`

格式化 ：日期 --> 文本，使用 SimpleDateFormat 的 format() 方法。    

解析：文本 --> 日期，使用 SimpleDateFormat 的 parse() 方法。

①格式化1

``` java
SimpleDateFormat sdf = new SimpleDateFormat();
String date = sdf.format(new Date());
System.out.println(date);//14-5-12 下午3:24
```

②格式化2

``` java
SimpleDateFormat sdf1 = new SimpleDateFormat(“EEE, d MMM yyyy HH:mm:ss Z”); 
date = sdf1.format(new Date()); 
System.out.println(date);//星期一, 12 五月 2014 15:29:16 +0800
```

③解析：

``` java
Date date1 = sdf.parse(“14-5-12 下午3:24”); 
System.out.println(date1); 
date1 = sdf1.parse(“星期一, 12 五月 2014 15:29:16 +0800”); 
// date1 = sdf1.parse(“14-5-12 下午3:24”); 
System.out.println(date1);
```



# 3. Calendar类

(1) 获取实例：

``` java
Calendar c = Calendar.getInstance(); 
```

(2)  get()/set()/add()/date getTime()/setTime()



# 4. Math类



# 5. BigInteger  BigDecimal类

