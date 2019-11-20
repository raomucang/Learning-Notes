# day03 【权限修饰符、代码块、常用API】

### 第一章 权限修饰符

##### 1.1 权限修饰符的介绍

```java
a.什么是权限修饰:
	控制其修饰的成员的访问范围
b.Java中有四大权限修饰符:
	public > protected > 不写(默认/default) > private  
```

##### 1.2 不同权限修饰符的访问能力

```java
						public		protected		不写(默认/default)		private
在本类中					 OK		    OK				OK					OK
在本包其他类中				   OK		  OK			  OK				   NO
在其他包其他类中(继承本类)	    OK 		   OK			   NO					NO
在其他包其他类中			  OK		 NO				 NO					   NO             

总结:我们开发中能用到的修饰符只有2个
a.一般来说成员变量必须使用private
b.一般来说成员方法必须使用public
c.构造方法一般使用public,也有使用private(单例模式中用到)
```

### 第二章 代码块

##### 2.1 构造代码块

```java
a.什么是构造代码块:
		就是写在类中方法外的代码块(Java中一对大括号就称为代码块)
b.什么时候执行构造代码块:
		当我们每次创建对象时,都会执行构造代码块
c.构造代码块和构造方法的优先级:
		每次创建对象都是先执行构造代码块,然后再执行构造方法
public class Dog {
    private int age;
    private String name;
    //这就是构造代码块
    {
        System.out.println("构造代码块执行了...");
    }
    public Dog() {
        System.out.println("无参构造...");
    }
    public Dog(int age, String name) {
        System.out.println("全参构造...");
        this.age = age;
        this.name = name;
    }
}
//测试类
public class TestDogDemo {
    public static void main(String[] args) {
        //1.创建Dog对象
        Dog d1 = new Dog();
        Dog d2 = new Dog();
        Dog d3 = new Dog();
    }
}
//输出结果
构造代码块执行了...
无参构造...
构造代码块执行了...
无参构造...
构造代码块执行了...
无参构造...		
```

##### 2.2 静态代码块

```java
a.什么是静态代码块:
		就是写在类中方法外的代码块,但是该代码块由关键字static修饰
b.静态代码块什么时候执行:
		第一次时候使用到该类时(第一次加载该类时),该类中的静态代码块会执行一次
		而且只会执行一次(第二次以及以后就不会执行了)
c.静态代码块和构造方法的优先级
		同一个类中,静态代码块的优先级高于构造方法,甚至高于main方法
		
public class Dog {
    private int age;
    private String name;
    //这就是静态代码块
    static {
        System.out.println("静态代码块执行了...");
    }
    public Dog() {
        System.out.println("无参构造...");
    }
    public Dog(int age, String name) {
        System.out.println("全参构造...");
        this.age = age;
        this.name = name;
    }
}

//测试类
public class TestDogDemo {
    public static void main(String[] args) {
        //1.创建Dog对象
        Dog d1 = new Dog();
        Dog d2 = new Dog();
        Dog d3 = new Dog();
    }
}
//输出结果
静态代码块执行了...
无参构造...
无参构造...
无参构造...
  
//测试类
public class TestDogDemo {
    static {
        System.out.println("........");
    }
    public static void main(String[] args) {
        System.out.println("!!!!!!!!!");
    }
}
//输出结果
........
!!!!!!!!!		
```

### ==第三章 Object类==***

##### 3.1 Object类的介绍

```java
类Object是所有类的祖宗类
每一个类都是Object的子类,所以所有对象都拥有Object类中方法(包括数组)
```

##### 3.2 toString方法**

```java
public String toString(); 
该方法的作用: "返回该对象的字符串表示。"
字符串的格式: "包名.类名@地址值"
在实际开发中: "我们都会重写toString,不要返回地址值,返回该对象中成员变量的值" 
  
实际上我们toString方法都不需要调用:
	我们直接打印对象名时,实际上JVM会自动调用对象的toString方法,打印出其返回值
	也就是说以下写法是等价的:
			System.out.println(对象名);
			System.out.println(对象名.toString());
总结:
	"如果我们自己的类重写了toString方法,我们只需要直接打印对象名就可以打出来所有成员变量的值
		
  
public class Cat {
    int age;
    String name;
    public Cat() {
    }
    public Cat(int age, String name) {
        this.age = age;
        this.name = name;
    }
    //重写toString
    //Object类中默认返回值的是: 包名.类名@地址值
    //我们一般想返回每个成员变的值,所以我们重写toString
    @Override
    public String toString() {
        return "Cat{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //1.创建Cat对象
        Cat cc = new Cat(10, "加菲猫");
//        String s = cc.toString();
//        System.out.println("s:" + s);//com.itheima.demo02_toString.Cat@4554617c
        System.out.println(cc); //等价于System.out.println(cc.toString());
        //2.再来一个对象
        Cat ccc = new Cat(20, "夜猫");
//        String ss = ccc.toString();
//        System.out.println("ss:" + ss);//com.itheima.demo02_toString.Cat@74a14482
        System.out.println(ccc);//等价于System.out.println(ccc.toString());
    }
}
```

##### 3.3 equals方法**

```java
public boolean equals(Object obj);
该方法的作用: "判断其他某个对象是否与此对象“相等”。"
默认判断的是: "两个对象的地址值"(从Object的equals源码中可以得出结论)
在实际开发中: "我们通常会重写equals方法,不要比较对象的地址值,而要比较对象的成员变量的值"  
  
总结:
	"我们自己定义类,如果想要比较成员变量的值,只需要重写equals方法,"
	"以后调用equals方法即可比较成员变量是否相同"
public class Dog {
    int age;
    String name;

    public Dog() {
    }

    public Dog(int age, String name) {
        this.age = age;
        this.name = name;
    }
    //重写equals方法
    //比较两个对象的成员变量的值
  
//	这是idea自动生成的equals方法
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Dog dog = (Dog) o;
        return age == dog.age &&
                Objects.equals(name, dog.name);
    }
//    这是我们自己编写的equals方法
//    public boolean equals(Object obj) {
//        //比较两个对象的属性值
//        //当前对象this
//        //参数对象obj
//        Dog d = (Dog)obj;
//        return this.age == d.age && this.name.equals(d.name);
//    }
}
  
```

##### 3.4 扩展(equals和==的区别)***

```java
对于引用类型
	==	比较是地址值
	equals 默认比较是的也是地址值,如果重写了equals方法,比较成员变量值

对于基本类型
	== 比较的数值
	equals 基本类型没有equals方法!!!!
```

##### 3.5 Objects类

```java
Objects是一个工具类(是指所有的成员都是静态的,方便程序员直接通过类名调用)
在Objects中有一个判断两个对象是否相同的方法:
	   public static boolean equals(Object a, Object b){
          	return (a == b) || (a != null && a.equals(b));
          	//该方法首先判断两个对象的地址值,再判断a是否非空!!,最后判断我们需要的a和b是否相等
          	//比我们直接判断 a.equals(b) 更加严谨,程序更加健壮
        }

"面试题: 做任何面试题时,第一步一定要进行数据合法性判断"
	   "a.任何对象必须先判非空  b.如果是数组之类,还需要判断是否越界 c.对象字符串一般还会判断是否为""
```

### 第四章 Date类**

##### 4.1 Date类的介绍

```java
a.Date类  表示一个时间点,可以精确到毫秒(1秒=1000毫秒)		
```

##### 4.2 Date类的构造方法

```java
Date类的构造方法:
public Date(); 创建一个代表当前系统时间的Date对象
public Date(long time); 创建一个代表距离标准时间time毫秒后的时间的Date对象
		国际规定标准(零时区): 1970.01.01 00:00:00 (英国)
  				   (东八区): 1970.01.01 08:00:00 (中国)	
public class TestDateDemo {
    public static void main(String[] args) {
        //1.创建Date对象(当前系统时间)
        Date now = new Date();
        //Mon Nov 04 10:34:04 CST 2019
        System.out.println(now);
        //2.创建Date对象(距离标准时间后xx毫秒的时间)
        Date d = new Date(0L);
        //Thu Jan 01 08:00:00 CST 1970
        System.out.println(d);
    }
}   
```

##### 4.3 Date类的常用方法

```java
成员方法:
public long getTime(); 获取当前Date对象距离标准时间的毫秒值
public void setTime(long time); 修改当前Date对象的时间

public class TestDateDemo {
    public static void main(String[] args) {
        //1.创建Date对象(当前系统时间)
        Date now = new Date();
        //Mon Nov 04 10:34:04 CST 2019
        //2.获取当前Date对象的毫秒值
        long time = now.getTime();
        System.out.println("当前系统时间距离标准时间的毫秒值是:" + time);
        //3.修改当前Date对象的毫秒值
        now.setTime(1000L);
        System.out.println(now);
    }
}
```

### 第五章 DateFormat类**

5.1 DateFormat类的作用

```java
DateFormat类: 对Date类的格式进行转换的类,简称日期格式化类
默认打印Date对象时的格式: Thu Jan 01 08:00:00 CST 1970
我们就可以通过DateFormat类将以上格式改成我们中国人喜欢的格式: xxxx年xx月xx日 xx:xx:xx

在DateFormat中:
	y - 年 M - 月 d - 日	H - (24)时 m - 分  s - 秒
```

##### 5.2 DateFormat类的构造方法

```java
通过查阅API发现,DateFormat是一个抽象类,我们找到他的子类:SimpleDateFormat

SimpleDateFormat的构造方法:
	public SimpleDateFormat(String pattern); 创建一个日历格式化对象,需要传入一个日期模式
```

##### 5.3 DateFormat类的成员方法

```java
public String format(Date date);将日期对象格式化为一个字符串
public Date parse(String time); 将一个字符串解析为一个Date对象

public class TestSimpleDateFormatDemo {
    public static void main(String[] args) throws ParseException {
        //1.创建一个Date对象
        Date now = new Date();
        //2.创建日历格式化类对象
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        //3.调用sdf的格式化日期方法
        String nowTime = sdf.format(now);
        //假设我们喜欢: 1999年10月20日 08:10:20
        System.out.println(nowTime);//输出:2019年11月04日 11:10:02
        //4.调用sdf的解析方法
        Date date = sdf.parse("2025-10-20 10:10:10");
        System.out.println(date);
    }
}
```

### 第六章 Calendar类

```java
Calendar: 日历类,也是和Date类一样,代表某个时间点

Calendar类获取对象的方式:*****************
	public static Calendar getInstance(); 静态方法获取一个日历类对象 

/**
 * YEAR=2019,MONTH=10,DAY_OF_MONTH=4
 * HOUR_OF_DAY=11,MINUTE=25,SECOND=0,
 */
public class TestCalendarDemo {
    public static void main(String[] args) {
        //1.获取的Calendar对象
        Calendar c = Calendar.getInstance();
        System.out.println(c);
    }
}

Calendar类的成员方法:
	public int get(int field); 根据字段的编号,获取日历对象中某个字段的值
	
public class TestCalendarDemo {
    public static void main(String[] args) {
        //1.获取的Calendar对象
        Calendar c = Calendar.getInstance();
        //2.获取日历对象中某个字段的值
        int year = c.get(Calendar.YEAR);
        System.out.println(year);

        int month = c.get(Calendar.MONTH);
        System.out.println(month);

        int day = c.get(Calendar.DAY_OF_MONTH);
        System.out.println(day);
    }
}

	public void set(int field,int value) ;根据字段的编号,修改日历对象中某个字段的值
	public void set(int year,int month,int day);直接修改年月日的方法
	
public class TestCalendarDemo {
    public static void main(String[] args) {
        //1.获取Calendar对象
        Calendar c = Calendar.getInstance();
        printCalendarNYR(c);
        //2.修改Calendar对象
        c.set(Calendar.YEAR,3000);
        c.set(Calendar.MONTH,10);
        c.set(Calendar.DAY_OF_MONTH,10);
        printCalendarNYR(c);
    }
    public static void printCalendarNYR(Calendar c) {
        //2.获取日历对象中某个字段的值
        int year = c.get(Calendar.YEAR);
        System.out.println(year);
        int month = c.get(Calendar.MONTH);
        System.out.println(month);
        int day = c.get(Calendar.DAY_OF_MONTH);
        System.out.println(day);
    }
}
	public void add(int field,int value);根据字段的编号,增加/减少日历对象中某个字段的值
	
public class TestCalendarDemo {
    public static void main(String[] args) {
        //1.获取Calendar对象
        Calendar c = Calendar.getInstance();
        printCalendarNYR(c);
        //2.增加/减少Calendar对象
        c.add(Calendar.YEAR,3000);
        c.add(Calendar.MONTH,-1);
        c.add(Calendar.DAY_OF_MONTH,2);
        printCalendarNYR(c);
    }

    public static void printCalendarNYR(Calendar c) {
        //2.获取日历对象中某个字段的值
        int year = c.get(Calendar.YEAR);
        System.out.println(year);

        int month = c.get(Calendar.MONTH);
        System.out.println(month);

        int day = c.get(Calendar.DAY_OF_MONTH);
        System.out.println(day);
    }
}
```

### 第七章 Math类

```java
Math类: "用于执行基本的一写数学运算"
Math类的特点: "也是一个工具类,所有方法都是静态的,并且构造方法是私有的,无法创建对象
Math类中常见的8个方法:
public double abs(double d); //求一个数的绝对值
public double max(double d1,double d2);//求两个数的最大值
public double min(double d2,double d2);//求两个数的最小值
public long round(double d);//四舍五入到整数
public double random();// 生成[0,1)的随机整数
public double ceil(double d); //向上取整
public double floor(double d); //向下取整
public double pow(double d1,double d2);//求次幂

public class TestMathDemo {
    public static void main(String[] args) {
        //1.Math类不能创建对象,因为构造方法私有化
//        Math math = new Math();
        //2.Math中常见的8个方法
//        public double abs(double d); //求一个数的绝对值
        System.out.println(Math.abs(-10));
        System.out.println(Math.abs(10));
//        public double max(double d1,double d2);//求两个数的最大值
        System.out.println(Math.max(3.5, 4.5));
//        public double min(double d2,double d2);//求两个数的最小值
        System.out.println(Math.min(3.5, 4.5));
//        public long round(double d);//四舍五入到整数
        System.out.println(Math.round(3.499));
        System.out.println(Math.round(3.50001));
//        public double random();// 生成[0,1)的随机整数
        System.out.println(Math.random());
//        public double ceil(double d); //向上取整
        System.out.println(Math.ceil(4.001));
//        public double floor(double d); //向下取整
        System.out.println(Math.floor(4.999));
//        public double pow(double d1,double d2);//求次幂
        System.out.println(Math.pow(10, 3));
    }
}
```

### 第八章 System类

```java
System类: "包含几个简单的有用的方法,它也不能创建对象(构造方法私有化)"
  
常用的静态方法:
public static void exit(int status); 终止当前运行的 Java 虚拟机，非零表示异常终止
public static long currentTimeMillis(); 获取当前时间距离标准时间的毫秒值 

public class TestSystemDemo {
    public static void main(String[] args) {
        //1.System.exit(0)方法
        for (int i = 0; i < 10; i++) {
            System.out.println(i);
            System.exit(0); //终止JVM
        }
        //2.获取毫秒值
        long time = System.currentTimeMillis();
        System.out.println(time);
        //3.currentTimeMillis在实际开发中的运算,计算程序耗时
        //String StringBuilder
        long start = System.currentTimeMillis();
        String s = "";
        for (int i = 0; i < 100000; i++) {
            s += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("String拼接耗时:" + (end - start) + "毫秒");//String拼接耗时:27701毫秒

        long start1 = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 100000; i++) {
            sb.append(i);
        }
        long end1 = System.currentTimeMillis();
        System.out.println("StringBuilder拼接耗时:" + (end1 - start1) + "毫秒");//StringBuilder拼接耗时:23毫秒
    }
}
```

### 第九章 BigDecimal类

```java
BigDecimal类: 比普通的数据,提供了更加精准的数据计算

BigDecimal构造方法:
	public BigDecimal(double d); //这种构造还是有精度问题
	public BigDecimal(String d); //这种构造时没有精度问题

BigDecimal成员方法:
	public BigDecimal add(BigDecimal value) 加法运算
	public BigDecimal subtract(BigDecimal value) 减法运算
	public BigDecimal multiply(BigDecimal value) 乘法运算
	public BigDecimal divide(BigDecimal value) 除法运算

public class TestDemoBigDecimalDemo {
    public static void main(String[] args) {
//        System.out.println(0.09 + 0.01); //0.1
//        System.out.println(1.0 - 0.32);  //0.68
//        System.out.println(1.015 * 100); //101.5
//        System.out.println(1.301 / 100); //0.01301

        //0.09 + 0.01
        BigDecimal b1 = new BigDecimal("0.09");
        BigDecimal b2 = new BigDecimal("0.01");
        BigDecimal add = b1.add(b2);
        System.out.println(add);

        //1.0 - 0.32
        BigDecimal b3 = new BigDecimal("1.0");
        BigDecimal b4 = new BigDecimal("0.32");
        BigDecimal subtract = b3.subtract(b4);
        System.out.println(subtract);

        //1.015 * 100
        BigDecimal b5 = new BigDecimal("1.015");
        BigDecimal b6 = new BigDecimal("100");
        BigDecimal multiply = b5.multiply(b6);
        System.out.println(multiply);

        //1.301 / 100
        BigDecimal b7 = new BigDecimal("1.301");
        BigDecimal b8 = new BigDecimal("100");
        BigDecimal divide = b7.divide(b8);
        System.out.println(divide);
    }
}
```

### 第十章 Arrays类

```java
Arrays类: 它就一个数组的工具类,专门用于操作数组的各种静态方法

常用静态方法:
	public static String toString(int[] a);//将数组元素拼接成字符串返回
	public static void sort(int[] a);//默认升序,排序数组

public class TestArraysDemo {
    public static void main(String[] args) {
        //1.定义一个数组
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        //2.Arrays的toString
        System.out.println(Arrays.toString(arr));

        //3.在定义一个数组
        int[] nums = {5, 2, 6, 8, 3, 1, 9, 4, 7};
        //4.使用Arrays.sort方法
        Arrays.sort(nums);
        System.out.println(Arrays.toString(nums));
    }
}
```

### 第十一章 包装类

```java
包装类: 又称为基本类型对应的引用类型,全称基本数据类型包装类
"每种基本类型对应的包装类:"
	byte	---		Byte
	short	---		Short
	char	---		Character
	int		---		Integer		
	long	---		Long
	float	---		Float
	double	---		Double
	boolean	---		Boolean
```

##### 11.1Integer的介绍

```java
Integer构造方法:
	public Integer(int n);
	public Integer(String n);

Integer静态方法:
	public static Integer valueOf(int i);
	public static Integer valueOf(String i);	

Integer成员方法:
	public int intValue(); //从Integer对象中取出int值

public class TestIntegerDemo {
    public static void main(String[] args) {
        //1.创建Integer对象_使用构造方法
        Integer i1 = new Integer(10);
        System.out.println(i1);

        Integer i2 = new Integer("10");
        System.out.println(i2);

        //2.创建Integer对象_使用静态方法
        Integer i3 = Integer.valueOf(10);
        System.out.println(i3);

        Integer i4 = Integer.valueOf("10");
        System.out.println(i4);

        //3.Integer的成员方法
        Integer i = new Integer(100);
        int value = i.intValue();
        System.out.println(value);
    }
}

```

##### 11.2 装箱和拆箱

- 装箱: 把基本类型 转成 对应的包装类

  ```java
  装箱有两种方式:
  	第一种通过构造方法: public Integer(int n);
  	第二种通过静态方法: public static Integer valueOf(int i);
  ```

- 拆箱: 把包装类 转回 对应的基本类型

  ```java
  拆箱有几种方式:
  	通过包装类的成员方法: public int intValue();
  ```

- 在JDK5之后,我们程序员不需要手动拆箱装箱,引入了自动拆装箱

  ```java
  装箱:
  	Integer i = 10; //底层实际上是通过静态方法valueOf帮助我们装箱的

  拆箱:
  	int num = i; //底层实际上是通过调用intValue方法帮助我们拆箱的


  public class TestIntegerDemo {
      public static void main(String[] args) {
          //1.创建Integer对象_使用构造方法
          Integer i1 = new Integer(10);
          System.out.println(i1);

          Integer i2 = new Integer("10");
          System.out.println(i2);

          //2.创建Integer对象_使用静态方法
          Integer i3 = Integer.valueOf(10);
          System.out.println(i3);

          Integer i4 = Integer.valueOf("10");
          System.out.println(i4);

          //3.Integer的成员方法
          Integer i = new Integer(100);
          int value = i.intValue();
          System.out.println(value);

          //4.自动拆箱装箱
          Integer i5 = 10;
          int num = i5;
          System.out.println(num);
      }
  }

  ```

##### 11.3 各种面试题

```java
面试1: 基本类型与字符串之间的转换

基本类型  --->  字符串
	int a = 1234;
	方式1: a + "" ==> "1234"
	方式2: String.valueOf(a);  

字符串  --->   基本类型
	String age = "18";
	方式1: 首先变成对应的包装类
    		Integer i = new Integer(age);
		    Integer i = Integer.valueOf(age);
		   接下来对包装类进行拆箱操作即可
		    int age = i;(JDK1.5)
	方式2: 直接使用包装类的parseXxx方法即可
		    int num = Integer.parseInt(age);
	
	注意: 各种包装类中均有对应的parseXxx(String s)方法,可以把对应的字符串解析对应的基本类型
			Integer.parseInt("111");
			Double.parseDouble("12.34");
			Boolean.parseBoolean("true");
			除了Boolean的parseBoolean这个方法之外,其他的parse方法,如果参数不能解析,将抛出异常
			而Boolean的parseBoolean如果参数不能解析,直接返回false
			
			
/**
 * 基本类型和字符串之间的转换
 */
public class TestDemo01 {
    public static void main(String[] args) {
        //基本类型  --->  字符串
        int age = 10;
        //a.拼接一个空字符串
        String s1 = age + "";
        //b.通过String的静态方法
        String s2 = String.valueOf(age);

        //字符串  --->   基本类型
        String height = "171.3";

        //a.先变成对应的包装类,再拆箱
        Double dd = Double.valueOf(height);
        double d = dd;

        //b.直接使用对应包装类的parsexxx方法
        double ddd = Double.parseDouble(height);
        System.out.println(ddd);

        //注意事项: 除了Boolean的parseBoolean之外,其他的parseXxx方法必须保证字符串是可以解析成对应的基本类型的,否则将会抛出异常
//        Integer.parseInt("中国");
//        Double.parseDouble("美国");
        boolean b = Boolean.parseBoolean("小日本");
        System.out.println(b);
    }
}			
```

```java
面试题2:
	 如何打印指定位数的小数
	 比如: double price = 120.6938
/**
 * 如何打印指定位数的小数
 */
public class TestDemo02 {
    public static void main(String[] args) {
        //1.有一个小数
        double price = 120.6938;
        //使用printf格式化输出小数
        System.out.printf("%.2f",price);// 120.69
        System.out.printf("%10.2f",price); // 空格空格空格空格120.69
    }
}            
```

```java
面试题3:
	有以下四个Integer包装类对象
	Integer i1 = new Integer(10);
	Integer i2 = new Integer(100);

	Integer i3 = Integer.valueOf(10);
	Integer i4 = Integer.valueOf(100);

	Integer i5 = 10;
	Integer i6 = 100;

	问题:以上六个对象中,哪些对象是同一个对象??
      
public class TestDemo03 {
    public static void main(String[] args) {
        //以下六个对象中,哪些对象是同一个对象??
        Integer i1 = new Integer(10); // 创建新的对象
        Integer i2 = new Integer(100);

        Integer i3 = Integer.valueOf(10);
        Integer i4 = Integer.valueOf(100);

        Integer i5 = 10; //自动装箱,底层通过 Integer.valueOf(10)
        Integer i6 = 100;//自动装箱,底层通过 Integer.valueOf(100)

        //可能是同一个对象的只有:i1 i3 i5 或者 i2 i4 i6
        System.out.println(i1 == i3); // false
        System.out.println(i1 == i5); // false
        System.out.println(i3 == i5); // true

        //结论: 只要是new创建出来的,肯定地址是独一无二
        //如果是Integer.valueOf或者自动装箱获取的对象,那么可能是同一个对象
        //a.值相等并且范围在-128和127之间,那么是同一个对象(因为都是从缓存池中获取的)
        //b.值不相等或者范围不在-128和127之间,那么都是new创建的对象,肯定不是同一个对象

        System.out.println("=======");
        System.out.println(i2 == i4); //false
        System.out.println(i2 == i6); //false
        System.out.println(i4 == i6); //true
    }
}	
```



















