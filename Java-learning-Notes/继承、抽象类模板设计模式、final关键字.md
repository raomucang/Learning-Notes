# day01 【复习回顾、继承、抽象类模板设计模式、final关键字】

### 第一章 复习***

##### 1.1 如何定义类

```java
public class 类名{
  //成员变量
  数据类型 变量名;
  //成员方法
  public 返回值类型 方法名(参数){
   	方法体;
    return 返回值;
  }
  //构造方法
}
```

##### 1.2 如何通过类创建对象

```java
类名 对象名 = new 构造方法名(参数);
```

##### 1.3 封装

- 封装的步骤

  - 给成员变量加上private修饰符
  - 为成员变量提供get/set方法

- 封装的代码实现

  ```java
  public class 类名{
    private int age; 
    //get
    public int getAge(){
      return age;
    }
    //set
    public void setAge(int age){
      this.age = age;
    }
    //生成get/set方法的快捷键: alt+insert,选择get/set即可
  }
  ```

##### 1.4 构造器(构造方法)

```java
构造方法:
a.没有返回值,连void也没有
b.构造方法的名字必须和所在的类名完全一样
c.当我们不提供任何构造时,系统会默认生成一个无参构造
d.当我们提供了构造,系统就不再提供无参构造
public 类名(){
	//无参构造
}
public 类名(参数){
	给成员变量赋值对应的参数
	//有参构造
}
```

##### 1.5 this关键字

- this的作用:

  - 区分局部变量和成员变量同名的情况

    ```java
    Java默认采用就近原则,如果有局部变量和成员变量同名时,我们直接写变量名,默认访问局部变量(就近),
    我们可以使用this.变量名,指定访问成员变量
    ```

- this的本质

  - this就是一个对象,当前方法的调用者对象

    ```java
    谁调用了方法,方法中的this就指代谁
    ```

- this在代码中的应用

  - 在set方法中

    ```java
    public void setAge(int age){
      	this.age = age;
    }
    ```

  - 在有参数的构造方法中

    ```java
    public Dog(int age,String name){
      	this.age = age;
      	this.name = name;
    }
    ```

### 第二章 继承(面向对象的三大特性:封装,继承,多态)

- 引入案例

  ```java
  假如我们要定义如下类: 学生类,老师类和班主任类，分析如下：
  1. 学生类 属性:姓名,年龄 行为:吃饭,睡觉,学习
  2. 老师类 属性:姓名,年龄，薪水 行为:吃饭,睡觉，教书
  3. 班主任 属性:姓名,年龄，薪水 行为:吃饭,睡觉，管理  
  ```

##### 2.1 继承的概念**

```java
在一个已知类的基础上创建一个新的类的过程称为继承
已知类:称为父类/超类/基类,英文名:SuperClass
新的类:称为子类/派生类,英文名:SubClass
```

##### 2.2 继承的格式***

```java
public class 父类{
 	 //成员变量
     //成员方法  
}
public class 子类 extends 父类{
  	//子类继承父类之后,可以继承并直接使用父类中的非私有成员
  	//结论:父类非私有的子类可以继承
  	//结论:父类私有的子类不能继承,但是拥有!!
}
```

##### 2.3 继承的案例

```java
/**
 * 父类:人类
 */
public class Human {
    private String name;
    private int age;
    public void eat() {
        System.out.println("吃吃吃...");
    }
    public void sleep() {
        System.out.println("睡睡睡...");
    }
}
/**
 * 子类:学生类
 */
public class Student extends Human {
    public void study() {
        System.out.println("键盘敲烂,月薪过万..");
    }
}
/**
 * 子类:班主任
 */
public class AssistantTeacher extends Human {
    private double salary;//薪水
    public void manager() {
        System.out.println("负责报名,收费等....");
    }
}
/**
 * 测试类
 */
public class TestDemo {
    public static void main(String[] args) {
        //1.创建对象
        Student s1 = new Student();
        s1.setAge(10);
        s1.setName("小明");
        System.out.println(s1.getAge());
        System.out.println(s1.getName());
        s1.eat();
        s1.sleep();
        s1.study();
        //2.创建对象
        AssistantTeacher at = new AssistantTeacher();
        System.out.println(at.getAge());
        System.out.println(at.getSalary());
        System.out.println(at.getName());
        at.eat();
        at.sleep();
        at.manager();
    }
}
//其他的子类,我们课下自己完成...
```

- 继承好处总结

  ```java
  a.提高代码的复用性
  b.为多态提供了前提
  ```

##### 2.4 子类不能继承的内容

```java
a.子类无法继承父类的构造方法
b.父类的中私有成员,子类也是无法继承的(只是拥有,但是不能直接使用,不称为继承)
```

##### 2.5 继承后的特点——成员变量**

```java
a.如果子父类的成员变量不同名,访问时没有任何问题
b.如果子父类的成员变量同名,访问时根据就近原则,"优先"访问子类的成员变量
c.如果方法中的局部变量和本类的成员变量和父类的成员变量同名时,
	优先访问局部变量
	this.变量名 访问本类的成员变量
	super.变量名 访问父类的成员变量
	所以: this用于区分局部和本类的成员变量同名
		super用于区分本类和父类成员变量同名
/**
 * 父类
 */
public class Fu {
    int num = 10;
}
/**
 * 子类
 */
public class Zi extends Fu{
    int num = 99;
    public void showNum() {
        //访问同名的变量num,由于就近原则
        int num = 111;
        System.out.println(num); // 访问谁??? 就近原则 局部变量111最近
        System.out.println(this.num); //访问谁?? 本类的成员99变量
        System.out.println(super.num); //访问谁??父类的成员10变量
    }
}
```

##### 2.6 继承后的特点——成员方法**

```java
a.如果子父类的成员方法不同名,调用时没有任何问题
b.如果子父类的成员方法同名,调用时根据就近原则,"优先"调用子类的成员方法
c.在测试类,无法直接调用同名方法中的父类的方法!!!!!
  	但是在子类的方法中,可以通过super.Xxx()调用父类的同名方法
/**
 * 父类
 */
public class Fu {
    public void show() {
        System.out.println("Fu类中的show方法...");
    }
}
/**
 * 子类
 */
public class Zi extends Fu {
    public void show() {
        //但是在子类的方法中,可以使用super.方法名()调用父类的方法
        super.show();
        System.out.println("Zi类中的show方法...");
    }
}
/**
 * 测试类
 */
public class TestDemo {
    public static void main(String[] args) {
        //1.创建对象
        Zi zz = new Zi();
        //2.调用同名方法,根据就近原则
        zz.show(); //调用谁??? 调用子类的show
        //强调一下:在测试类中,无法直接越过子类,调用父类的同名方法
        //zz.super.show();这种语法在Java中是不存在的!!!!!
    }
}
```



##### 2.7 重写的概念和应用

```java
a.方法的重载(Overload):
	在同一个类中,方法名相同,参数列表不同(参数个数,参数类型,参数顺序),这些方法称为方法的重载
b.方法的重写(Override):
	在子父类中,出现除了方法体其他一模一样的方法,那么子类中这个方法称为方法的重写
```

```java
什么时候子类需要重写方法:
	当子类继承父类方法之后,发现方法功能不足,那么子类就可以重写该方法,重新实现方法体
/**
 * 父类:动物类
 */
public class Animal {
    public void eat() {
        System.out.println("动物吃....");
    }

    public void sleep() {
        System.out.println("动物睡....");
    }
}
/**
 * 子类:猫类
 */
public class Cat extends Animal {
    //子类继承父类后,发现父类的继承的方法功能上不满足子类的要求,所以我们可以对不满足要求的方法进行重写
    @Override
    public void eat() {
        System.out.println("猫喜欢舔着吃...");
    }

    @Override
    public void sleep() {
        System.out.println("猫蜷曲着睡...");
    }
}

public class TestDemo {
    public static void main(String[] args) {
        //1.创建Cat对象
        Cat jf = new Cat();
        //2.调用方法
        jf.eat();
        jf.sleep();
        //执行了重写后的方法
    }
}
```

##### 2.8 @Override注解

```java
在Java中@开头的我们称为注解
@Override 称为方法重写注解,标记一个方法是重写的方法
@Override 作用:a.标记该方法是重写的(给程序员看的) b.编译检查(给编译器看的) 
```

##### 2.9 继承的注意事项

```java
a.方法重载前提是同一个类中,方法重写前提是有继承关系
b.在重写/覆盖/复写/Override方法时,子类重写方法的权限 >= 父类中方法的权限
	(了解)Java中有四大权限: public > protected > 默认权限 > private 
c.在重写/覆盖/复写/Override方法时,保证返回值类型,方法名,参数列表都要和父类的一模一样
	(注意:方法体肯定不一样,权限可以不一样,但是一般也会写成一样的)
d.父类的私有方法,没有重写的概念,就算子类写了一个和父类一模一样的方法,也不称为重写  
```

##### 2.10 继承后的特点——构造方法**

- 构造方法特点介绍

  ```java
  a.构造方法子类无法继承的
  b.在子类的构造方法内部,是可以调用父类的构造方法的!!!!! 通过: super(); 调用父类的无参构造方法
  c.继承后子类构造方法特点:子类"所有构造器"的"第一行"都会先调用父类的"无参构造器"，再执行自己
  ```

- 构造方法案例演示

  ```java
  public class Person {
      int age;
      String name;

      public Person() {
          System.out.println("Person的无参构造..");
      }

      public Person(int age, String name) {
          System.out.println("Person的满参构造...");
          this.age = age;
          this.name = name;
      }
  }
  public class Student extends Person {
      String id;//学号

      public Student() {
          //子类构造第一行,默认有一句代码 super()
          super(); //表示调用父类的无参构造方法
          System.out.println("学生的无参构造...");
      }

      public Student(String id) {
          //子类构造第一行,默认有一句代码 super()
          super(); //表示调用父类的无参构造方法
          System.out.println("学生的学号构造...");
          this.id = id;
      }
  }

  public class TestDemo {
      public static void main(String[] args) {
          //1.创建学生1
  //        Student s1 = new Student();
          //2.创建学生2
          Student s2 = new Student("10086");
      }
  }
  ```

- 构造方法总结

  - 任何子类的任何构造方法,第一行都有super();表示先调用父类的无参构造方法,然后执行子类的构造

##### 2.11 super(...)和this(...)

- 小案例引入
- super和this的用法格式
- super(....)用法演示
- super(...)案例图解
- this(...)用法演示
- 小结

##### 2.12 继承的特点**

### 第三章 抽象类

##### 3.1 抽象类的概述

##### 3.2 abstract使用格式

- 抽象方法
- 抽象类
- 抽象类的使用

##### 3.3 抽象类的特征和注意事项

##### 3.4  抽象类存在的意义

##### 3.5 第一个设计模式：模板模式(司机开车)

### 第四章 final关键字

##### 一.final关键字

##### 4.1 final的作用介绍

- final修饰类
- final修饰方法
- final修饰局部变量

  思考:如下两种写法，哪种可以通过编译？
- final修饰引用类型的变量
- final修饰成员变量

