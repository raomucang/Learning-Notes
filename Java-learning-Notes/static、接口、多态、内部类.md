# day02【static、接口、多态、内部类】

### 第一章 static关键字

##### 1.1 概述

```java
a.static可以修饰成员变量,也可以修饰成员方法
b.被static修饰的成员,我们一般称为静态成员/类成员
c.被static修饰的成员,不属于某个对象,而属于整个类(所有对象共享),我们可以通过类名直接使用他们
```

##### 1.2 定义和使用格式

- 类变量

  ```java
  类变量:被static修饰的成员变量,也叫静态成员变量
  - "类变量不属于任何一个对象,属于类(属于所有对象共享),在内存中只有一个
  - "类变量不建议通过对象访问,建议通过类名直接访问
  /**
   * 中国人类
   */
  public class Chinese {
      String name;
      String job;
      //静态成员变量,国籍,不属于任何一个对象的
      static String nation = "中华人民共和国";
  }

  public class TestDemo {
      public static void main(String[] args) {
          //1.创建第一个中国人
          Chinese jackChen = new Chinese();
          jackChen.name = "成龙";
          jackChen.job = "武打明星";
          //输出
          System.out.println(jackChen.name);
          System.out.println(jackChen.job);
          System.out.println(jackChen.nation);
          System.out.println("================");
          //2.创建第二个中国人
          Chinese liuYan = new Chinese();
          liuYan.name = "柳岩";
          liuYan.job = "球星";
          //输出
          System.out.println(liuYan.name);
          System.out.println(liuYan.job);
          System.out.println(liuYan.nation);
          //干一件事,柳岩移民了,移到了米国
          System.out.println("===============");
          liuYan.nation = "小米共和国";
          System.out.println("柳岩的国籍:"+liuYan.nation); //小米共和国
          System.out.println("成龙的国籍:"+jackChen.nation); //小米共和国
      }
  }
  ```

- 内存图:

  ![](static.png)

  ​

- 静态方法

  ```java
  静态方法: 被static修饰的成员方法,也叫类方法
  - "静态方法不建议通过对象调用,而建议通过类名直接调用
  /**
   * 计算器类
   */
  public class Calculate {

      public static int getSum(int a, int b) {
          return a + b;
      }
  }
  public class TestDemo {
      public static void main(String[] args) {
          //1.创建对象去调用,可以但是不建议
          Calculate cc = new Calculate();
          System.out.println(cc.getSum(10, 20));
          //2.建议通过类名直接调用
          System.out.println(Calculate.getSum(10, 20));
      }
  }
  ```

##### 1.3 静态和非静态之间的相互调用***

```java
静态和静态之间  可以相互访问
非静态与非静态之间 可以相互访问
非静态可以静态
- "但是静态不可以访问非静态

(了解)为什么有以上结论呢??? 因为静态和非静态的生命周期不同
静态随着类的加载而存在的!!!
非静态随着对象的创建而存在的!!!
所以:静态 生命周期是早于 非静态的  
静态出现的早   而 非静态出现的晚
(秦始皇)			(我们)

```

##### 1.4 建议调用格式**

```java
静态变量:
	可以 对象名.静态变量名
	"建议 类名.静态变量名
静态方法: 
	可以 对象名.静态方法名();
	"建议 类名.静态方法名();
```

### 第二章 接口****

##### 2.1 概述

```java
接口也是一种引用类型,
		接口是一种极端的"抽象类",接口中只能有抽象方法(JDK7)
         接口中新增了默认方法(default关键字)和静态方法(static关键字)(JDK8)
         接口中又新增了私有方法(JDK9) 
```

##### 2.2 定义格式***

```java
public interface 接口名{
 	//1.抽象方法
  	//2.默认方法
  	//3.静态方法
  	//4.私有方法(JDK9)
}
/**
 * 接口的定义格式
 */
public interface MyInterface {
    //1.抽象方法
    [public abstract] void abs1();
    [public abstract] void abs2();
    //2.默认方法
    [public] default void d1(){
        System.out.println("接口中的默认方法d1...");
    }
    [public] default void d2(){
        System.out.println("接口中的默认方法d2...");
    }
    //3.静态方法
    [public] static void s1(){
        System.out.println("接口中的静态方法s1...");
    }
    [public] static void s2(){
        System.out.println("接口中的静态方法s2...");
    }
    //4.私有方法(了解)
}
"注意:由[]括起来的表示可以省略,但是省略不代表没有,默认添加
```

##### 2.3 接口的使用**

```java
a.接口和抽象类一样,都不能创建对象
b.接口天生也是做爹的,被子类"实现"的,"实现"的作用等同于继承
格式:
	public class 实现类 implements 接口名{
      	//a.实现类必须重写接口中的所有抽象方法
      	//b.实现类可以选择性重写接口中的默认方法,但是重写后不能有default
      	//c.静态方法没有重写的概念,即使实现类写了一个和接口中声明一样的静态方法,也不称为重写
    }
代码实现:
/**
 * 接口的实现类
 */
public class MyClass implements MyInterface{
    //a.实现类必须重写接口中的所有抽象方法
    @Override
    public void abs1() {
        System.out.println("实现类重写的抽象方法1");
    }
    @Override
    public void abs2() {
        System.out.println("实现类重写的抽象方法2");
    }
    //b.实现类可以选择性重写接口中的默认方法,但是重写后不能有default
    @Override
    public void d1() {
        System.out.println("实现类中重写的默认方法1");
    }
    //c.静态方法没有重写的概念,即使实现类写了一个和接口中声明一样的静态方法,也不称为重写
    public static void s1(){
        System.out.println("实现类中的静态方法s1...");
    }
}
```

##### 2.4 接口的多实现**

```java
接口的多实现: 一个实现类 可以同时实现多个接口
			一个子类,只能继承一个抽象类
格式:
	public class 实现类 implements 接口1,接口2,...{
      	//a.实现类必须重写所有接口中的所有抽象方法
      			//如果有相同的抽象方法,那么实现类只需要重写一次
      	//b.实现类选择性重写所有接口中的默认方法
      			//如果有相同的默认方法,实现类必须重写一次
      	//c.静态方法没有重写的概念
      			//如果有相同的静态方法,那也没有冲突,
      			//因为静态方法通过哪个接口名调用的,就是调用那个接口中静态方法
    }
//接口1
public interface Inter1 {
    public abstract void abs1();

    public abstract void abs();

    public default void d1() {
        System.out.println("接口1默认方法d1...");
    }

    public default void d() {
        System.out.println("接口1默认方法d...");
    }

    public static void s1() {
        System.out.println("接口1静态方法s1...");
    }

    public static void s() {
        System.out.println("接口1静态方法s...");
    }
}
//接口2
public interface Inter2 {
    public abstract void abs2();

    public abstract void abs();

    public default void d2() {
        System.out.println("接口2默认方法d2...");
    }

    public default void d() {
        System.out.println("接口2默认方法d...");
    }

    public static void s2() {
        System.out.println("接口2静态方法s2...");
    }

    public static void s() {
        System.out.println("接口2静态方法s...");
    }
}
//实现类
public class MyClass implements Inter1, Inter2 {
    //a.实现类必须重写所有接口中的所有抽象方法
    @Override
    public void abs1() {

    }
    @Override
    public void abs2() {

    }
    //如果有相同的抽象方法,那么实现类只需要重写一次
    @Override
    public void abs() {

    }
    //b.实现类选择性重写所有接口中的默认方法
    //如果有相同的默认方法,实现类必须重写一次
    @Override
    public void d() {
        System.out.println("重写后的d方法");
    }
    //c.静态方法没有重写的概念
    //如果有相同的静态方法,那也没有冲突,
    //因为静态方法通过哪个接口名调用的,就是调用那个接口中静态方法
    public static void s() {
        System.out.println("实现类中的静态方法s...");
    }
}
```

##### 2.5 实现和继承的优先级问题

```java
一个类,继承一个父类同时实现多个接口
格式:
public class 实现类/子类 extends 父类 implements 接口1,接口2,...{
  	//实现类必须先写继承父类 后写实现接口
  	//因为继承的优先级 比 实现的优先级要高!!!
  	//结论:如果父类中的正常方法和接口中的默认方法相同,那么实现类不必要重写,默认优先调用父类的方法
  	//	如果实现类重写了该方法,调用实现类重写后的该方法
}

public class Fu {
    public void show() {
        System.out.println("Fu中show方法...");
    }
}
public interface FuInterface {
    public abstract void abs();
    public default void show() {
        System.out.println("接口中的show方法...");
    }
}

public class MyClass extends Fu implements FuInterface {
    @Override
    public void abs() {
        System.out.println("实现类中的abs..");
    }

    @Override
    public void show() {
        System.out.println("实现类中的show....");
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //1.创建子类对象
        MyClass mc = new MyClass();
      	//实现类调用show方法时,如果没有重写,优先调用父类的(不会调用接口的)
      	//如果重写了,那么调用重写后的该方法
        mc.show();
    }
}
```

##### 2.6 接口的多继承【了解】

```java
类和类之间,称为继承,是单继承
类和接口之间,称为实现,是多实现
接口和接口之间,称为继承,是多继承
格式:
	public interface 子接口 extends 父接口1,父接口2..{
      	//子接口中拥有所有父接口中的内容
    }

实现类
	public class 实现类 implements 子接口{
		//该实现相当于实现了多个父接口
	}
	
面试题:
	Java中支持多继承吗????
     类之间不支持多继承,只能单继承
     接口之间可以多继承
     类和接口之间不叫继承,称为实现,但是可以多实现

```

##### 2.7 接口中其他成员特点**

```java
a.接口中无法定义成员变量,但是可以定义常量,常量必须使用以下格式:
		[public static final] 数据类型 常量名 = 值; //[]表示可以省略,但是省略不代表没有,默认就有
b.接口中没有构造方法,也不能创建对象
		抽象类中是有构造方法,但是不能创建对象
c.接口中，没有静态代码块。		
```

##### 2.8 抽象类和接口的练习

```java
- 定义一个缉毒狗类
a.定义父类:狗类(定义狗共性内容)
b.定义接口:缉毒接口(定义额外的,扩展的,后添的功能)
c.定义目标类:继承父类,实现接口
/**
 * 父类:狗类,定义所有狗的共性内容
 */
public abstract class Dog {

    private int age;

    private String kind;

    public abstract void eat();

    public abstract void sleep();
}
/**
 * 缉毒接口
 */
public interface JiDuInterface {
    public abstract void jiDu();
}
/**
 * 实现类:缉毒狗类
 */
public class SuperDog extends Dog implements JiDuInterface {
    @Override
    public void eat() {
        System.out.println("必须大口吞,1分钟吃完...");
    }

    @Override
    public void sleep() {
        System.out.println("必须站着睡,随时待命...");
    }

    @Override
    public void jiDu() {
        System.out.println("会咬人,寻找毒品...");
    }
}
```

### 第三章 多态

##### 3.1 多态的概述

```java
什么是多态:
	一个对象的不同形态
比如: H20(水)
  		高温下:气态
  		常温下:液态
  		低温下:固定
比如: 一只狗
		关到动物园: 动物
		养在家里头: 宠物
		放在饭桌上: 食物	
```

##### 3.2 多态的前提[重点]

```java
a.必须有继承关系/实现关系
b.必须有方法的重写
```

##### 3.3 多态的体现[重点]

```java
多态的体现:
	语言描述: 父类类型的变量名 指向了 子类类型的对象
	代码描述: Animal d = new Dog();
			Animal d = new Cat();
示例代码:
public class TestDemo {
    public static void main(String[] args) {
        //1.创建Dog对象使用多态
        //a.直接赋值的多态
        Animal an = new Dog();
        //b.间接赋值的多天
        Dog dd = new Dog();
        Animal an1 = dd;
        //c.方法调用时的多态
        Dog dd2 = new Dog();
        show(dd2);
    }
    public static void show(Animal an){
		//Animal an = new Dog();
    }
}
```

##### 3.4 多态调用方法的特点[重点]

```java
"编译看父类
"运行看子类
总结:编译看父,运行看子,必须子父类都有的方法才能运行成功!!!
  
public class Person {
    public void hello() {
        System.out.println("你好~~~");
    }
}
public class Student extends Person {
    public void hello() {
        System.out.println("昨天作业写完了吗。。。。");
    }
}

public class TestDemo {
    public static void main(String[] args) {
        //1.创建一个学生对象,使用多态接收
        Person p = new Student();
        //2.多态调用方法
        p.hello();
        //多态调用方法时,编译阶段看父类,运行阶段看子类
    }
}
```

##### 3.5 多态的好处[最重点]

```java
多态的好处:提高了代码的扩展性/灵活性
public abstract class Animal {
    public abstract void eat();
}
public class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("扒着吃...");
    }
}
public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗舔着吃...");
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //1.调用猫的吃
        Cat c = new Cat();
        showAnimalEat(c);
        //2.调用狗的吃
        System.out.println("====");
        Dog d = new Dog();
        showAnimalEat(d);
    }

    //定义一个方法,展示动物的吃饭
    public static void showAnimalEat(Animal an) {
        //Animal an = new Cat();
        //Animal an = new Dog();
        System.out.println("宝贝,来吃一个...");
        an.eat();
    }
}
```

##### 3.6 多态的弊端

```java
多态的弊端:只能调用子父类共有的方法,不能调用子类特有的
public abstract class Animal {
    public abstract void eat();
}
public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗舔着吃...");
    }
    //特有方法
    public void lookHome() {
        System.out.println("汪汪,谁,滚!!!");
    }
}
public class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("扒着吃...");
    }
    //特有方法
    public void catchMouse() {
        System.out.println("老鼠,有种别跑....");
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //1.创建猫对象,使用多态
        Animal an = new Cat();
        an.eat();
	    an.catchMouse(); //报错!!!因为父类中没有该方法,编译失败

        //2.创建狗对象,使用多态
        Animal an2 = new Dog();
        an2.eat();
	    an2.lookHome(); //报错!!!因为父类中没有该方法,编译失败
    }
}
```

##### 3.7 多态弊端的解决方案-向下转型

```java
在基础班中:"数据类型转换" 适用于基本类型
	自动转换: "窄类型" 转成 "宽类型"
      		 double d = 1;
	强制转换: "宽类型" 转成 "窄类型"
      		 int a = (int)3.14;

在就业班中:"转型" 适用于引用类型
	向上转型: "子类型" 转成 "父类型"
      		Animal an = new Cat();
	向下转型: "父类型" 转回 "子类型"
      		Cat cc = (Cat)an;

public class TestDemo {
    public static void main(String[] args) {
        //1.创建猫对象,使用多态
        Animal an = new Cat();
        an.eat();
      	//an不能直接调用Cat的特有方法catchMouse()
        //将an进行向下转型
        Cat cc = (Cat)an;
        cc.catchMouse();
        System.out.println("====");
        //2.创建狗对象,使用多态
        Animal an2 = new Dog(); 
        an2.eat();
      	//an2不能直接调用Dog的特有方法lookHome();
        //将an2进行向下转型
        Dog dd = (Dog) an2;
        dd.lookHome();
    }
}
```

##### 3.8 转型可能出现的异常

```java
向下转型的过程中可能出现一种异常: ClassCastException(类型转换异常)
public static void main(String[] args){
    //3.向下转型可能会出现类型转换异常
    Animal an1 = new Dog();
    Cat c = (Cat)an1;//ClassCastException
    //Dog cannot be cast to Cat
    c.catchMouse();  
}
```

##### 3.9 instanceof关键字的介绍

```java
Java提供一个关键字,实际上是一个运算符:instanceof
 怎么使用???
  	对象名 instanceof 类名
 计算结果:
	如果对象名所指向的对象,属于右边这种类型,那么返回true
	如果对象名所指向的对象,不属于右边这种类型,那么返回false
使用instaceof,在转型之前进行判断,保证转型不会出现类型转换异常
public static void main(String[] args){
	//3.向下转型可能会出现类型转换异常
	Animal an1 = new Cat();
	//Cat c = (Cat)an1;//ClassCastException
	//Dog cannot be cast to Cat
	//c.catchMouse();
	//4.使用向下转型之前,必须先使用instanceof运算符进行判断
	if (an1 instanceof Dog) {
		//说明,an1指向对象是Dog类型
		Dog d = (Dog) an1;
		d.lookHome();
	}
	if (an1 instanceof Cat) {
		//说明,an1指向对象是Cat类型
		Cat c = (Cat) an1;
		c.catchMouse();
	}
}
```

### 第四章 内部类

##### 4.1 什么是内部类

```java
在一个类A内部定义另外一个类B,此时类B称为内部类,类A称为外部类
```

##### 4.2 内部类的分类以及其特点

```java
根据内部类的所定义的位置不同:
"成员内部类": 定在外部类的类中方法外
public class Person {
    private int age;
    private String name;

    //成员内部类
    class Heart{
        int count;//心跳
        public void jump() {
            //在成员内部的方法中,可以"无条件"访问外部类中的任何成员
            System.out.println("心脏嗷嗷跳...");
            //访问外部类的成员
            System.out.println(age);
            System.out.println(name);
            eat();
            sleep();
        }
    }
    public void eat() {
    }
    public void sleep() {
    }
}

public class TestDemo {
    public static void main(String[] args) {
        //1.创建Person类的成员内部类Heart类对象
        Person p = new Person();
        //成员内部类创建对的格式:
        // 外部类名.内部类名 对象名 = new 外部类().new 内部类();
        Person.Heart h1 = new Person().new Heart();
        //也可以这么些
        Person.Heart h2 = p.new Heart();
    }
}

      
      
"局部内部类": 定义在外部类的类中方法内
public class Person {
    int age;
    String name;
    public void eat() {
        //局部内部类
        class InnerClass{
            int a;
            String b;
            public void show(){

            }
        }
        //特点:只能在局部使用
        InnerClass ic = new InnerClass();

    }
    public void sleep() {
    }
}


```

##### 4.3 匿名内部类[重点]

```java
语法糖(Syntactic sugar): 某个固定格式的简化格式,但是原理是不变的
比如:
	int[] nums = new int[]{1,2,3,4,5};
	语法糖:
	int[] nums = {1,2,3,4,5};
匿名内部类:其实就一种语法糖,一种简便格式
	匿名内部类,是创建抽象类的子类对象和接口的实现类对象的简便格式
	
"a.使用匿名内部类创建抽象类的子类对象"

public abstract class Animal {
    public abstract void eat();
    public abstract void sleep();
}
public class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗吃..");
    }
    @Override
    public void sleep() {
        System.out.println("狗睡..");
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //创建子类对象
        Dog d1 = new Dog();
        d1.eat();
        d1.sleep();
        //使用匿名内部类,快速完成创建抽象类的子类对象
        Animal dd = new Animal(){
            @Override
            public void eat() {
                System.out.println("狗吃..");
            }

            @Override
            public void sleep() {
                System.out.println("狗睡..");
            }
        };
        dd.eat();
        dd.sleep();
    }
}
"b.使用匿名内部类创建接口的实现类对象"
 
public interface JiDuInterface {
    /**
     * 缉毒方法
     */
    void jiDu();

    /**
     * 开枪方法
     */
    void biuBIuBiu();
}

public class JiDuDog implements JiDuInterface {
    @Override
    public void jiDu() {
        System.out.println("有种别跑,放下毒品..");
    }

    @Override
    public void biuBIuBiu() {
        System.out.println("再跑,老子开枪了...");
    }
}
public class TestDemo {
    public static void main(String[] args) {
        //1.创建实现类对象
        JiDuDog dd = new JiDuDog();
        dd.jiDu();
        dd.biuBIuBiu();
        //2.使用匿名内部类,快速创建接口的实现类对象
        JiDuInterface dd2 = new JiDuInterface() {
            @Override
            public void jiDu() {
                System.out.println("有种别跑,放下毒品..");
            }

            @Override
            public void biuBIuBiu() {
                System.out.println("再跑,老子开枪了...");
            }
        };
        dd2.jiDu();
        dd2.biuBIuBiu();
    }
}
```

### 第五章 引用类型使用小结

- 记住: 基本类型能做的引用类型也可以做

##### 5.1 引用类型作为方法参数和返回值

```java
public class TestDemo01 {
    public static void main(String[] args) {
        //基本类型可以作为方法的参数和返回值
        showNum(10);
        int a = 20;
        showNum(a);
        //引用类型也可以作为方法的参数和返回值
        showDog(new Dog());
        Dog dog = new Dog();
        showDog(dog);
    }
    //基本类型作为方法的返回值
    public static int getNum() {
//        return 10;
        int a = 10;
        return a;
    }
    //引用类型作为方法的返回值
    public static Dog getDog() {
//        return new Dog();
        Dog d = new Dog();
        return d;
    }

    //基本类型作为方法参数
    public static void showNum(int num) {
        System.out.println(num);
    }

    //引用类型作为方法参数
    public static void showDog(Dog dd) {
        System.out.println(dd);
    }
}
```

##### 5.2 引用类型作为成员变量***

```java
/**
 * 游戏角色:战士类
 */
public class Soldier {
    //成员变量
    String nickname;
    int level;
    int hp;
    int attack;
    //武器
    Weapon wq;
    //成员方法
    public void killPerson() {
        System.out.println("老子叫做"+nickname+",等级是"+level+",血量是"+hp+",攻击力是"+attack+"," +
                "拿着一把名字叫做"+wq.name+"的,颜色为"+wq.color+"的,攻击力为"+wq.attack+","+wq.level+"级武器砍人了,一到暴击99999...");
    }
}
/**
 * 游戏道具:武器类
 */
public class Weapon {
    String name;
    int level;
    int attack;
    String color;
}

public class TestDemo {
    public static void main(String[] args) {
        //模拟一个战士,拿着武器出去砍人
        //1.创建一个战士
        Soldier zzh = new Soldier();
        //2.创建一把武器
        Weapon w = new Weapon();
        //3.给属性赋值
        zzh.nickname = "渣渣辉";
        zzh.level = 99;
        zzh.hp = 10000;
        zzh.attack = 1111;

        w.name = "屠龙宝刀";
        w.level = 99;
        w.attack = 8888;
        w.color = "屎黄色";

        //4.给角色装备上武器
        zzh.wq = w;
        //5.砍人
        zzh.killPerson();
    }
}
```

##### 5.3 总结

```java
能够掌握static关键字修饰的变量调用方式
	被static修饰的成员变量的特点:
	a.该成员变量不属于某个对象,属于所有对象共享,内存中只保存一份
	b.建议通过类名直接访问,不建议通过对象访问(可以!)
能够掌握static关键字修饰的方法调用方式
	被static修饰的成员方法的特点:
	a.建议通过类名直接调用,不建议通过对象调用(可以!!)
    b.方法本类就不属于对象,无论是否有static修饰,方法都不属于对象 
    
能够写出接口的定义格式
	public interface 接口名{
		//抽象方法
		//默认方法
		//静态方法
		//私有方法(JDK9)
	}
能够写出接口的实现格式
	public class 实现类 implements 接口名1,接口名2,..{
		//必须重写所有抽象方法
				//如果有相同抽象方法,只能重写一次
		//选择性重写默认方法
				//如果有相同默认方法,必须重写一次
		//静态方法没有重写的概念(静态方法是通过所在的类名或者接口名直接调用)
				//如果相同的静态方法,没有冲突
	}
能够说出接口中的成员特点
	a.接口中没有成员变量,但是有常量,常量必须使用以下格式:
		[public static final] 数据类型 常量名 = 值;//常量名的命名规范:所有单词全大写,中间使用_分割
	b.接口中没有构造方法(抽象类中有构造方法)
    
    c.接口中没有静态代码块  
    
能够说出多态的前提
	a.必须有继承或者实现
	b.必须有方法的重写
能够写出多态的格式
	父类类型的对象名 指向了 子类类型的对象
	Fu ff = new Zi();
	Animal an = new Cat();
	Person p = new Student();
能够理解多态调用方法的特点
	Animal an = new Cat();
	an.eat();
	编译看父,运行看子
	案例:猫狗吃饭案例,体现多态的好处(扩展性)
      
能够理解多态向上转型和向下转型
	向上转型:
		Animal an = new Dog();//其实就是多态
	向下转型:
		Dog d = (Dog)an;
		
能够说出内部类概念
	定义在一个类的内部的类,称为内部类
能够理解匿名内部类的编写格式
	匿名内部类,可以快速创建抽象类的子类对象和接口的实现类对象
	
    抽象类 对象名 = new 抽象类(){
    	重写抽象类中的所有抽象方法
    };

	接口名 对象名 = new 接口名(){
  		重写接口中的所有抽象方法
	};
	
	匿名内部类的弊端:
		不能在匿名内部类中写特有方法
		
```



