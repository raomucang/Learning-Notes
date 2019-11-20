## day14【反射、注解、动态代理、JDK8新特性】

### 第一章.反射

##### 1.类的加载

```java
.java源文件	--javac-->	.class字节码文件  --java--> JVM执行

一个类(.class文件)是何时加载到JVM的方法区中的???
a.当一个类第一次使用到时,才会加载到内存中 
b.第二次以后不需要再次加载类,因为已经在内存中了


"类的加载除了把.class加载到内存,还干了什么事??????"
	当类(class文件)加载到内存之后 ,JVM会自动为其生成一个字节码文件对象,我们称为之Class对象 
	该字节码文件对象(Class对象)中包含了字节码文件中所有的信息
```

##### 2.什么是反射

```java
反射就是在运行时期,获取到某个类的Class对象,从而解剖它,从中取出构造方法,成员方法,成员变量等,进而可以使用他们,这就是反射!!!
```

##### 3.反射在实际开发中的应用

```java
a.开发IDE(比如IDEA,Eclipse)
b.设计和学习框架必备(比如:mybatis,spring,springmvc,...)  
```

##### 4.反射中万物皆对象的概念

```java
Class类型 ---> 字节码文件对象的类型
Field类型 ---> 成员变量的类型
Method类型 --> 成员方法的类型
Constructor类型 ---> 构造方法的类型

newInstance -->  创建实例(创建对象)
invoke --> 执行/调用  

体验一下反射中的语法:
	创建对象:
		正常语法: new 构造方法(参数);
		反射语法: 构造方法.newInstance(参数);  
	调用方法:
		正常语法: 对象名.成员方法(参数);
		反射语法: 成员方法.invoke(对象名,参数);
	成员变量:
		正常语法: (赋值)对象名.成员变量名 = 值; (取值)数据类型 变量名 = 对象名.成员变量名;
		反射语法: (赋值)成员变量.set(对象名,值);(取值)数据类型 变量名 = 成员变量.get(对象名);
```

##### ==5.反射的第一步获取字节码文件对象(Class对象)==

```java
/**
 * 反射的第一步:获取Class对象
 */
public class GetClassDemo {
    public static void main(String[] args) throws ClassNotFoundException {
        //1.通过类名的直接访问静态成员class
        Class clazz1 = Dog.class;
        System.out.println(clazz1);
        //2.通过该类的某个对象,也可以获取Class对象
        Dog d = new Dog();
        Class clazz2 = d.getClass();
        System.out.println(clazz2);
        //3.通过Class类的静态方法
        Class clazz3 = Class.forName("com.itheima.demo02_GetClass.Dog");
        System.out.println(clazz3);
        //注意:以上只是三种获取Class对象的方式,但是获取的是同一个对象
        System.out.println(clazz1 == clazz2);
        System.out.println(clazz1 == clazz3);
        System.out.println(clazz2 == clazz3);
    }
}
```

##### 6.Class对象中的三个方法

```java
public String getSimpleName();//获取当前Class对象所代表的类的名字(仅仅是类名)
public String getName(); //获取当前Class对象所代表的类的全限定名字(包名+类名)
public Object newInstace();//创建当前Class对象所代表的类的对象

public class ClassMethodDemo {
    public static void main(String[] args) throws Exception {
        //1.获取Class对象
        Class clazz = Dog.class;
        //2.获取简单名
        String simpleName = clazz.getSimpleName();
        System.out.println(simpleName);
        //3.获取全限定名
        String name = clazz.getName();
        System.out.println(name);
        //4.创建当前Class对象所代表的类的对象
        Object obj = clazz.newInstance();
        System.out.println(obj);
    }
}
```

##### ==7.通过反射获取构造方法&&使用构造方法创建对象==

```java
public Constructor getConstructor(Class... 参数);//获取Class对象中的某个"public"构造方法
public Constructor getDeclaredConstructor(Class... 参数);//获取Class对象中的某个"任意修饰符的"构造方法

public class GetConstructorDemo {
    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        //1.获取Class对象
        Class clazz = Cat.class;
        //2.获取其中"public"的构造方法
        Constructor con1 = clazz.getConstructor();
        System.out.println(con1);

        Constructor con2 = clazz.getConstructor(int.class, String.class);
        System.out.println(con2);

        Constructor con3 = clazz.getConstructor(int.class);
        System.out.println(con3);

        //3.如果是非公有构造
        Constructor con4 = clazz.getDeclaredConstructor(String.class);
        System.out.println(con4);

        //4.通过获取到的构造方法就可以创建对象
        System.out.println("------------------");
        Object obj1 = con1.newInstance();
        System.out.println(obj1);

        Object obj2 = con2.newInstance(10, "旺财");
        System.out.println(obj2);

        Object obj3 = con3.newInstance(10);
        System.out.println(obj3);

        //4.私有构造不能直接创建对象,必须先设置暴力访问权限
        con4.setAccessible(true);
        Object obj4 = con4.newInstance("加菲猫");
        System.out.println(obj4);
    }
}
注意:如果是私有构造方法,反射中必须先设置暴力访问权限,才可以调用
```

##### ==8.通过反射获取成员方法&&调用成员方法==

```java
public Method getMethod(String methodName,Class... 参数);//获取Class对象中某个"public"成员方法
public Method getDeclaredMethod(String methodName,Class... 参数);//获取Class对象中某个"任意修饰符的"成员方法

public class GetMethodDemo {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        //1.获取Class对象
        Cat cc = new Cat(10,"加肥猫");
        Class clazz = cc.getClass();
        //2.获取成员方法
        Method method1 = clazz.getMethod("eat");
        System.out.println(method1);

        Method method2 = clazz.getMethod("eat",String.class);
        System.out.println(method2);

        Method method4 = clazz.getMethod("getSum",int.class,int.class);
        System.out.println(method4);
        //3.如果非公有的方法
        Method method3 = clazz.getDeclaredMethod("eat",String.class,String.class);
        System.out.println(method3);
        //4.执行成员方法
        System.out.println("------------");
        method1.invoke(cc);
        method2.invoke(cc, "shi");
        //私有方法不能直接调用,先设置暴力访问权限
        method3.setAccessible(true);
        method3.invoke(cc, "shi", "liao");

        //扩展:如果方法有返回值,那么我们需要接受一下
        Object obj = method4.invoke(cc, 10, 20);
        System.out.println(obj);
    }
}
注意:
	a.如果是私有成员方法,必须先设置暴力访问权限,才能调用/执行
	b.如果方法有返回值,那么invoke时接收一下,如果没有返回值invoke时没必要接收
```

##### 9.通过反射获取成员属性(了解即可)

```java
public Field getField(String fieldName);//获取Class对象中某个"public"成员变量
public Field getDeclaredField(String fieldName);//获取Class对象中某个"任意修饰符的"成员变量

public class GetFieldDemo {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, IllegalAccessException {
        //1.获取Class对象
        Class clazz = Class.forName("com.itheima.demo06_Field.Cat");
        //2.获取"public"成员变量
//        Field field = clazz.getField("age");
//        System.out.println(field);
        //3.获取"任意修饰符的"成员变量
        Field field = clazz.getDeclaredField("age");
        System.out.println(field);
        //4.使用成员变量
        Cat cc = new Cat();
        System.out.println(cc);
        //私有成员变量,先必须设置暴力访问权限
        //赋值
        field.setAccessible(true);
        field.set(cc,10);
        System.out.println(cc);
        //取值
        Object obj = field.get(cc);
        System.out.println(obj);
    }
}
注意:
	成员变量一般是私有的,必须先设置暴力访问权限才能取值或者赋值
```

##### 10.扩展_反射中的其他方法

```java
"a.获取Class对象的方式: 三种
"b.反射获取构造方法:
    public Constructor getConstructor(Class... 参数);//获取"public"修饰的某个构造
    public Constructor getDeclaredConstructor(Class... 参数);//获取"任意"修饰的某个构造

    public Constructor[] getConstructors();//获取"public"修饰的所有构造
    public Constructor[] getDeclaredConstructors();//获取"任意"修饰的所有构造
"c.反射获取成员方法:
	public Method getMethod(String methodName,Class... 参数);//获取"public"修饰的某个方法
	public Method getDeclaredMethod(String methodName,Class...参数);//获取"任意"修饰的某个方法
		
	public Method[] getMethods();//获取"public"修饰的所有成员方法(包括父类继承的)
	public Method[] getDeclaredMethods();//获取"任意"修饰的所有成员方法(不包括父类继承的)
d.反射获取成员变量
	public Field getField(String fieldName);//获取某个"public"成员变量
	public Field getDeclaredField(String fieldName);//获取某个"任意修饰符的"成员变量
	
	public Field[] getFields();//获取所有的"public"成员变量
	public Field[] getDeclaredFields();//获取所有的"任意修饰符的"成员变量
```

### 第二章 注解

##### 1.JDK1.5新特性--注解

```java
什么是注解:
	一种标记,一种"注释"(给编译器看的)
```

##### 2.注解的三个作用

```java
a.编译时检查
	比如@Override 方法重写注解
b.为框架提供配置信息
c.生成帮助文档
```

##### 3.常用两个注解介绍

```java
@Override 方法重写注解
@FunctionalInterface 函数式接口注解
@Test 单元测试注解(Java提供的,而是第三方Junit提供)
@Deprecated 过时注解  
@Author  作者名注解
@Version 版本号注解
```

##### ==4.自定义注解==

```java
自定义类:class
自定义接口:interface
自定义枚举:enum
自定义注解:@interface  
  
格式:
	public @interface 注解名{
      
    }
/**
 * 自定义注解
 */
public @interface MyAnnotation {
  
}
```

##### ==5.给自定义注解添加属性==

```java
注解中只能添加属性
"格式:"
	public @interface 注解名{
     		数据类型 属性名1(); 
      		数据类型 属性名2(); 
    }
"注意:"
	a.注解的数据类型是有要求,必须是以下几种之一:
		i.八中基本类型均可
		ii.String类型,enum类型,其他注解类型,Class类型
		iii.以及以上所有类型的一维数组
	 b.属性可以给其赋值默认值,也可以不赋值
     	数据类型 属性名1() [default 默认值]; 
			比如: String age() default "18";
		如果数据类型是数组类型,那么default后面的默认值需要使用{}括起来
			比如:int[] nums() default {10,20,30};
		如果数组默认值只有一个元素,那么大括号可以省略
			比如:int[] nums() default 10;
```

##### 6.自定义注解练习

```java
/**
 * 带属性的自定义注解
 */
public @interface MyAnno {
    //i.基本类型
    int age() default 10;
    //ii.四种引用类型
    String name();//default "jack";
    //iii.数组
    String[] authors();// default "rose";
}
//使用自定义注解
@MyAnno(name = "jack",authors = {"a","b","c"})
public Demo(@MyAnnotation int age, @MyAnnotation String name) {
    this.age = age;
    this.name = name;
}
```

##### 7.使用注解时的注意事项

```java
a.使用注解必须保证每个属性都有值
b.如果该属性有默认值,那么使用时可以赋值也可以不赋值
c.如果该属性没有设置默认值,那么使用时必须赋值
d.如果属性是数组类型,那么赋值时,值必须使用{}括起来(如果只有一个元素可以省略大括号)
```

##### ==8.自定义注解中的特殊属性名value==

```java
如果注解中有一个特殊的属性value,那么
a.该注解中只有value这个属性,那么使用该注解时,可以省略value属性名,直接写属性值
/**
 * 注解中带有属性,属性名字叫做value
 */
public @interface Anno {
    String value();// default "xxx";
}
@Anno("jack")
public Demo(@MyAnnotation int age, @MyAnnotation String name) {
    this.age = age;
    this.name = name;
}
b.如果该注解中除了value属性,还有其他属性,但是其他属性都有默认值,且使用时不再重新赋值,那么也可以省略value属性名,直接写属性值 
/**
 * 注解中带有属性,属性名字叫做value
 */
public @interface Anno {
    String value();// default "xxx";
    int aaa() default 10;
}
//使用时其他属性不再重新赋值,此时value也可以省略
@Anno("jack")
public Demo(@MyAnnotation int age, @MyAnnotation String name) {
  this.age = age;
  this.name = name;
}
总结: 使用一个注解时,只需要对value属性值赋值,那么此时可以省略value,直接属性值即可!
```

##### 9.注解的注解--元注解

```java
元注解: 修饰注解的注解
元注解的作用:
	a.规定其修饰的注解的生命周期
	b.规定其修饰的注解的作用目标
	
```

- 两个元注解

```java
第一个元注解:
	@Target(),规定修饰的注解的作用目标,其属性value的值只能是以下几种之一:在ElementType枚举中
		ElementType.TYPE  表示其修饰的注解只能用于类,接口,枚举上
		ElementType.FIELD 表示其修饰的注解只能用于成员变量上
		ElementType.METHOD 表示其修饰的注解只能用于成员方法上
		ElementType.CONSTRUCTOR 表示其修饰的注解只能用于构造方法上
		ElementType.PARAMETER 表示其修饰的注解只能用于方法的参数上
		ElementType.LOCAL_VARIABLE 表示其修饰的注解只能用于局部变量上
		
//@Target(ElementType.TYPE) 
//@Target(ElementType.CONSTRUCTOR)
//@Target(ElementType.METHOD)
//@Target(ElementType.FIELD)
//@Target(ElementType.PARAMETER)
//@Target(ElementType.LOCAL_VARIABLE)
@Target({ElementType.CONSTRUCTOR,ElementType.METHOD})
public @interface MyAnno {
}	
@MyAnno
public class Demo {
    @MyAnno
    int age;
    @MyAnno
    public Demo(@MyAnno int age) {
        this.age = age;
    }
    @MyAnno
    public void show() {
        @MyAnno
        int a = 10;
        System.out.println(a);
    }
}

第二个元注解:
	@Retension 用来规定其修饰注解的生命周期,其value的属性值必须是以下几种之一:RetensionPolicy类中
		RetensionPolicy.SOURCE 表示其修饰的注解,只在源码阶段存在,字节码阶段和运行阶段没有该注解
		RetensionPolicy.CLASS	表示其修饰的注解,只在源码阶段和字节码阶存在,运行时没有该注解	
		RetensionPolicy.RUNTIME	表示其修饰的注解,在源码阶段,字节码阶段以及运行时期都存在,不删除
		
//@Retention(RetentionPolicy.SOURCE)
//@Retention(RetentionPolicy.CLASS)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnno {
}		 
```

##### 10.注解的解析

```java
什么是注解的解析:
	通过编写代码,把某个目标上的注解的属性值取出,并打印到控制
注解解析的步骤:
a.获取注解所在的类的字节码文件对象
b.获取注解所在的类的具体目标
c.判断我们获取的目标上是否有我们需要的注解
d.获取从目标上获取我们需要的注解
e.从注解上获取其属性值

/**
 * 自定义注解
 */
@Retention(RetentionPolicy.RUNTIME)
public @interface Book {
    String name();
    double price();
    String[] authors();
}
public class Demo {
    int age;
    public Demo() {
    }
    @Book(name = "三国演义",price = 99.9,authors = "罗贯中")
    public Demo(int age) {
        this.age = age;
    }
    public void show() {
    }
}
public class TestDemo {
    public static void main(String[] args) throws NoSuchMethodException {
//        a.获取注解所在的类的字节码文件对象
        Class demoClass = Demo.class;
//        b.获取注解所在的类的具体目标
        Constructor con = demoClass.getConstructor(int.class);
//        c.判断我们获取的目标上是否有我们需要的注解
        boolean b = con.isAnnotationPresent(Book.class);
        if (b){
//        d.获取从目标上获取我们需要的注解
           Book book = (Book) con.getAnnotation(Book.class);
//        e.从注解上获取其属性值
            String name = book.name();
            double price = book.price();
            String[] authors = book.authors();
            System.out.println("书名:"+name);
            System.out.println("价格:"+price);
            System.out.println("作者:"+ authors[0]);
        }else{
            System.out.println("该目标上没有Book注解...");
        }
    }
}	
```

##### 11.综合案例

```java
请自定义注解,模拟Junit中的@Test注解(不采用右键模拟,采用main方法模拟)
  
/**
 * 自定义注解,模拟Junit的@Test注解
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyTest {
}
public class Demo {
    @MyTest
    public void show1(){
        System.out.println("方法1...");
    }
    @MyTest
    public void show2(){
        System.out.println("方法2...");
    }
    public void show3(){
        System.out.println("方法3...");
    }
    @MyTest
    public void show4(){
        System.out.println("方法4...");
    }
}

public class TestDemo {
    public static void main(String[] args) throws Exception {
        //当我们执行main方法时,要求Demo这个类中有@Myest注解的方法会自动执行
        //1.获取注解所在类的字节码文件对象
        Class demoClass = Demo.class;
        //2.获取可能注解的方法
        Method[] methods = demoClass.getMethods();
        //3.判断目标上是否有注解
        for (Method method : methods) {
            if (method.isAnnotationPresent(MyTest.class)) {
                //4.执行该方法即可
                method.invoke(new Demo());
            }
        }
    }
}
```

### 第三章 动态代理

##### 1.代理模式介绍

```java
正常代理: 一个被代理类 一个代理类,这两个类都需要我们自己写
代理作用:对被代理类进行功能上的增强
```

##### 2.动态代理概述

```java
动态代理: 只需要被代理类,代理类由动态代理技术自动为我们生成! 
```

##### 3.案例引出

```java
public interface SchoolService {
    /**
     * 登录方法
     * @param username
     * @param password
     */
    void login(String username,String password);

    /**
     *  查询班级
     */
    String getAllClasses();

    /**
     * 查询分数
     */
    int getScore();
}

public class SchoolServiceImpl implements SchoolService {
    @Override
    public void login(String username, String password) {
//        System.out.println("登录时间:" + new Date());
//        long start = System.currentTimeMillis();
        System.out.println("正在登录...");
        try {
            Thread.sleep(new Random().nextInt(2000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //假设用户名是 root 密码是 123321
        if (username.equals("root") && password.equals("123321")) {
            System.out.println("登陆成功...");
        } else {
            System.out.println("用户名或者密码错误!!");
        }
//        long end = System.currentTimeMillis();
//        System.out.println("登录耗时:" + (end - start) + "毫秒");
    }

    @Override
    public String getAllClasses() {
//        System.out.println("查询时间:" + new Date());
//        long start = System.currentTimeMillis();
        try {
            System.out.println("正在查询..");
            Thread.sleep(new Random().nextInt(2500));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
//        long end = System.currentTimeMillis();
//        System.out.println("查询耗时:" + (end - start) + "毫秒");
        return "1班2班3班";
    }

    @Override
    public int getScore() {
        System.out.println("正在查询...");
        try {
            Thread.sleep(new Random().nextInt(2000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return new Random().nextInt(101);
    }
}

public class TestDemo {
    public static void main(String[] args) {
        //1.使用服务
//        SchoolServiceImpl impl = new SchoolServiceImpl();
        //2.调用登录
//        impl.login("root","123321");
        //3.调用查询
//        String allClasses = impl.getAllClasses();
//        System.out.println(allClasses);
        //4.使用动态代理技术自动创建
        /**
         * 参数一: 类加载器(这里可以使用当前类或者被代理类的类加载器)
         * 参数二: 被代理类实现的所有接口的Class对象数组  我们可以: new Class[]{接口1.class,接口2.class}  也可以 被代理类.class.getInterfaces()
         * 参数三: 处理器对象,我们需要传入该接口的匿名内部类型对象
         */
        SchoolService obj = (SchoolService)Proxy.newProxyInstance(SchoolServiceImpl.class.getClassLoader(), SchoolServiceImpl.class.getInterfaces(), new InvocationHandler() {
            /**
             * 当我们调用obj(代理对象)的方法时,代理对象会调用invoke方法,并把代理对象本身,方法对象,以及方法的参数传入
             * @param proxy 代理对象本身(obj)
             * @param method 我们调用的代理对象的方法
             * @param args  我们调用代理对象方法时传入的参数
             * @return
             * @throws Throwable
             */
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //如果是getScore方法,我们直接调用被代理对象的方法,不进行时间记录
              	if (method.getName().equals("getScore")) {
                    Object result = method.invoke(new SchoolServiceImpl(), args);
                    return result;
                }

                //增强的功能
                System.out.println("操作时间:"+new Date());
                long start = System.currentTimeMillis();

                //调用被代理对象原有的功能
                Object result = method.invoke(new SchoolServiceImpl(), args);
                //增强的功能
                long end = System.currentTimeMillis();
                System.out.println("操作耗时:" + (end - start) + "毫秒");
                return result;
            }
        });
//        obj.login("root","123321");
//        String allClasses = obj.getAllClasses();
//        System.out.println(allClasses);
        int score = obj.getScore();
        System.out.println("成绩:"+score);
    }
}
```

### 第四章 JDK8新特性

##### 1. 方法引用

- 方法引用介绍

  ```java
  把已经存在的方法直接拿过来用
  ```

- 方法引用四种方式

  ```java
  方法引用的符号 ::
  构造方法的引用: 类名::new
  静态方法的引用: 类名::静态方法名
  成员方法的引用: 对象名::成员方法名
  			实际上也可以使用 类名::成员方法名
  数组构造方法的引用:
  			数据类型[]::new
  ```

- 基于静态方法引用的代码演示

  ```java
  public class TestDemo {
      public static void main(String[] args) {
          String[] names = {"jack", "rose", "tom", "jerry"};
          //遍历数组
          //1.fori
          for (int i = 0; i < names.length; i++) {
              System.out.println(names[i]);
          }
          System.out.println("--------------");
          //2.迭代器.foreach
          for (String name : names) {
              System.out.println(name);
          }
          System.out.println("--------------");
          //3.方法引用
  //        Stream.of(names).forEach(new Consumer<String>() {
  //            @Override
  //            public void accept(String s) {
  //                System.out.println(s);
  //            }
  //        });
  //        System.out.println("--------------");
  //        Stream.of(names).forEach(s->System.out.println(s));
          //Consumer接口有一个抽象方法
          //public void accept(String s);
          //
          Stream.of(names).forEach(System.out::println);
      }
  }
  ```

##### 2. Base64

- Base64介绍

- Base64内嵌类和方法描述

  ```java
  Base64中有多个内部类:
  	Decoder: 解码器
  	Encoder: 编码器
  	MimeDecoder: 文件MIME类型的解码器
  	MimeEncoder: 文件MIME类型的编码器
  	UrlDecoder: URL的解码器
  	UrlEncoder: URL的编码器
  ```

- Base64代码演示

  ```java
  public class Base64Demo {
      public static void main(String[] args) throws UnsupportedEncodingException {
          //1.普通的编码器和解码器
          //编码
          Base64.Encoder encoder = Base64.getEncoder();
          String encodeToString = encoder.encodeToString("我爱中国我也爱Java".getBytes("UTF-8"));
          System.out.println("编码后的字符串:"+encodeToString);
          //解码
          Base64.Decoder decoder = Base64.getDecoder();
          byte[] bs = decoder.decode(encodeToString.getBytes("UTF-8"));
          System.out.println("解码后的字符串:"+new String(bs));

          //2.URl
          Base64.Encoder urlEncoder = Base64.getUrlEncoder();
          String encodeToString1 = urlEncoder.encodeToString("www.baidu.com?username=张三&password=123".getBytes("UTF-8"));
          System.out.println("编码后的字符串:"+encodeToString1);

          Base64.Decoder urlDecoder = Base64.getUrlDecoder();
          byte[] bytes = urlDecoder.decode(encodeToString1.getBytes("UTF-8"));
          System.out.println("解码后的字符串:" + new String(bytes));
          System.out.println("--------------------");
          //3.超大字符串编码
  //        UUID uuid = UUID.randomUUID();
  //        System.out.println(uuid);
          StringBuilder sb = new StringBuilder();
          for (int i = 0; i < 100; i++) {
              sb.append(UUID.randomUUID());
          }
          String encodeToString2 = Base64.getEncoder().encodeToString(sb.toString().getBytes("UTF-8"));
          System.out.println("大型字符串编码后的字符串:"+encodeToString2);
      }
  }
  ```

  ​

### 总结:

```java
"能够通过反射技术获取Class字节码对象
  	类名.class
    对象名.getClass();  
	Class.forName("全限定类名:包名.类名")
"能够通过反射技术获取构造方法对象，并创建对象。
      public Constructor getConstructor(Class... 参数);
	  public Constructor getDeclaredConstructor(Class... 参数);
	  Object obj = Constructor对象.newInstance(构造方法需要实际参数);
"能够通过反射获取成员方法对象，并且调用方法。
  		public Method getMethod(String 方法名,Class...参数);
		public Method getDeclaredMethod(String 方法名,Class...参数);
  		Object obj = Method对象.invoke(实际对象,方法需要的实际参数)
能够通过反射获取属性对象，并且能够给对象的属性赋值和取值。
能够说出注解的作用
"能够自定义注解和使用注解
	自定义注解:
		public @interface 注解名{
         	数据类型 属性名() default 默认值; 
          	数据类型 属性名() default 默认值; 
        }
	使用自定义注解:
		在类上方法上等,使用以下格式:
		@注解名(属性名=属性值,属性名=属性值);

能够说出常用的元注解及其作用
	@Target 规定作用目标,ElementType这个类中
	@Retension 规定声明周期,RetensionPolicy这个类中
能够解析注解并获取注解中的数据
	a.获取Class
	b.获取目标对象(Constructor,Method,Field)
  	c.判断目标对象上是否有目标注解
  	d.取出目标注解
  	e.取出注解中的属性值
能够完成注解的MyTest案例
	a.获取Class
	b.获取目标对象(Constructor,Method,Field)
  	c.判断目标对象上是否有目标注解
能够说出动态代理模式的作用
	代理:增强
	静态代理: 代理类需要自己编写,然后创建对象
	动态代理: 由Proxy代理技术自动生成
能够使用Proxy的方法生成代理对象
Proxy.newProxyInstance(ClassLoader 类加载器,Class[] 被代理类实现的接口的字节码文件数组,new InvovationHandler(){
      public Object invoke(Object 代理对象本身,Method 被调用的方法,Object[] 被调用方法的参数){
			每次调用代理对象的方法,代理对象都会调用该方法,并把方法和参数封装到Method和Object[]中
			对我们需要的方法进行增强,调用原有被代理对象的方法即可
			if(Method.getName().equals("xxx")){
					增强的代码
					method.invoke(被代理对象,被调用方法的参数);
					增强的代码
			}
      }
});
能够使用四种方法的引用(如果需要详细掌握,我们有视频)
	了解四种方法引用:
		System.out::println
能够使用Base64对基本数据、URL和MIME类型进行编解码
```

