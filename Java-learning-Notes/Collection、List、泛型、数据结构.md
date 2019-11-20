# day05-Collection、List、泛型、数据结构

### 第一章 Collection集合

##### 1.集合的介绍&集合和数组的区别

- 什么是集合

  集合就是Java中一种容器(可以保存多个数据)

- 数组: 也是Java中的一种容器

- 集合和数组的区别******:

  ```java
  a.数组的长度一旦定义,不可变,而集合的长度是随时可以变化
  b.数组中可以保存任意同一类型的数据,而集合只能保存引用类型的数据
  ```

##### ==2.集合框架的继承体系==*******

​	集合的根接口:Collection

​	两个子接口:	 List				Set

​	List接口的实现类: ArrayList,LinkedList,Vector

​	Set接口的实现类: HashSet,LinkedHashSet,TreeSet

##### 3.Collection中的通用方法

```java
增: public boolean add(E 元素);  
删: public boolean remove(Object 元素);
改: 无
查: 无
其他:
	public void clear(); 清空集合
	public int size(); 获取集合长度
	public boolean isEmpty();判断集合是否为空集合
	public boolean contains(Object obj);判断集合中是否包含某个元素
	public Object[] toArray(); 集合转数组
	
public class TestCollectionDemo {
    public static void main(String[] args) {
        //1.创建集合对象
        Collection<String> cc = new ArrayList<String>();
        //2.增加
        cc.add("java");
        cc.add("php");
        cc.add("python");
        System.out.println(cc);
        //3.删除
        boolean b = cc.remove("123");
        System.out.println(b);
        System.out.println(cc);
        //4.其他方法
        //清空
        cc.clear();
        System.out.println(cc);
        //长度
        System.out.println(cc.size());
        // 判断是否为空集合
        System.out.println(cc.isEmpty());
        // 判断是否包含
        boolean contains = cc.contains("php");
        System.out.println(contains);
        // 转成数组
        Object[] arr = cc.toArray();
        System.out.println(arr[0]);
        System.out.println(arr[1]);
        System.out.println(arr[2]);
    }
}	
```

### 第二章 Iterator迭代器

##### 1.集合迭代器的介绍和使用

- 什么是迭代器:

  ```java
  它是Java中的一种类型:Iterator类型
  其作用是:帮助我们遍历集合
  ```

- ==使用迭代器遍历集合==

  ```java
  public class TestIteratorDemo {
      public static void main(String[] args) {
          //1.创建集合(随便哪一个Collection实现类)
          Collection<String> cc = new ArrayList<String>();
          cc.add("java");
          cc.add("php");
          cc.add("python");
          cc.add("ui");
          //2.使用迭代器遍历集合
          //a.获取cc集合的迭代器对象
          Iterator<String> it = cc.iterator();
          //b.使用迭代器遍历集合的循环代码
          while (it.hasNext()) {
              String next = it.next();
              System.out.println(next);
          }
      }
  }
  ```

- 迭代器的注意事项(2个异常):

  ```java
  异常1:
  	NoSuchElementException 没有此元素异常
  	原因:当调用hasNext返回false,然后再掉next方法,就会引发此异常
  异常2:
  	ConcurrentModificationException 并发修改异常
      原因:在使用迭代器遍历集合的过程中,对集合进行增删元素操作
      注意:一般来说,迭代器就是单纯的遍历,不要在遍历过程中做其他任何操作(虽然修改是可以的)
  ```

##### 2.迭代器的原理(画图)

```java
迭代器底层采用了指针原理
```

![](Iterator.png)

##### ==3.增强for循环== 

```java
增强for循环实际上是一种语法糖: 是迭代器的简便格式
格式:
	for(数据类型 变量名 : 集合/数组){
      	System.out.println(变量名)
    }

public class TestForeachDemo {
    public static void main(String[] args) {
        //1.创建一个数组
        int[] nums = {1,2,3,4,5,6};
        for (int num : nums) {
            System.out.println(num);
        }
        System.out.println("============");
        //2.创建一个集合
        Collection<String> cc = new ArrayList<String>();
        cc.add("java");
        cc.add("php");
        cc.add("python");

        for (String c : cc) {
            System.out.println(c);
        }
    }
}
注意: 由于增强for循环的底层是迭代器,所以使用增强for循环遍历集合的过程中也不能对集合进行元素增删操作
```

### 第三章 泛型(了解)

##### 1.什么是泛型

```java
泛型的概念: 泛型其实是一种不确定的引用类型
泛型的格式: <E>,<P>,<Q>,<MVP>
```

##### 2.泛型的好处

```java
泛型是JDK1.5引入的新特性
泛型的好处:
	a.避免了类型强转的麻烦
	b.将运行时期的ClassCastException，转移到了编译时期变成了编译失败

public class TestGenericDemo {
    public static void main(String[] args) {
        method01();
    }

    //不使用泛型
    public static void method01() {
        //1.创建集合(不使用泛型)
        ArrayList arr = new ArrayList();
        //2.添加
        arr.add("java");
        arr.add("123");
        arr.add(123);
        arr.add(12.3);
        arr.add('K');
        arr.add(true);
        //3.遍历
        for(Object s : arr){
            String ss = (String) s;
            System.out.println(ss.length());
        }
    }

    //使用泛型
    public static void method02() {
        //1.创建集合(使用泛型)
        ArrayList<String> arr = new ArrayList<String>();
        //2.添加
        arr.add("java");
        arr.add("123");
        arr.add("123");
        arr.add("12.3");
        arr.add("K");
        arr.add("true");
        //3.遍历
        for (String s : arr) {
            //a.不需要强制转换
            //b.也不会出现类型转换异常,最多在编译时类型不一致,导致编译失败
            System.out.println(s.length());
        }
    }
}

注意: 在JDK1.5之后,Java强烈建议必须使用泛型
```

##### 3.泛型的定义和使用(非重点)

- ##### 泛型类 

  ```java
  泛型类: 泛型使用在类上
  定义格式:
      public class 类名<E>{
        	E age;
      }
  泛型类上的泛型什么确定??
    	在创建该类型的某个对象的时可以确定其泛型
   //泛型类 	
   public class Dog<E> {
      private E a;

      public E getA() {
          return a;
      }

      public void setA(E a) {
          this.a = a;
      }
  }
  //测试类
  public class TestDogDemo {
      public static void main(String[] args) {
          //1.创建Dog对象(带有泛型)
          //将泛型E确定为Integer
          Dog<Integer> d1 = new Dog<Integer>();
          d1.setA(10);
          System.out.println(d1.getA());

          Dog<String> d2 = new Dog<String>();
          //将泛型E确定为String
          d2.setA("java");
          System.out.println(d2.getA());
      }
  }

  ```

- ##### 泛型方法

  ```java
  泛型方法:泛型使用在方法上
  格式:
  	public <T> void 方法名(数据类型 参数名){

      }
  泛型方法上的泛型什么时候确定??
    	调用该方法,并传入参数时由参数的类型确定泛型的类型
    	
  public class Cat {
      //泛型方法
      public <T> void read(T num) {
          System.out.println("我家的猫正在读书:" + num);
      }
  }
   	
  public class TestCatDemo {
      public static void main(String[] args) {
          //1.创建我家的猫
          Cat cc = new Cat();
          //2.调用方法
          cc.read(10); //确定泛型T为Integer
          cc.read("你好"); //确定泛型T为String
          cc.read(3.14); //确定泛型T为Double
      }
  }
  ```

- ##### 泛型接口

  ```java
  泛型接口: 泛型使用在接口上
  格式:
  	public interface 接口名<E>{
        
      }

  //泛型接口
  public interface MyInterface<E> {
      void show(E s);
      void read(E s);
  }

  泛型接口上的泛型什么时候确定???
  //a.在实现类 实现 接口时,确定泛型的具体类型
  class MyClass1 implements MyInterface<Integer>{
      @Override
      public void show(Integer s) {
      }
      @Override
      public void read(Integer s) {
      }
  }
  //b.在实现类 实现 接口时,不确定泛型,依然保留泛型
  //此时实现类变为泛型类,遵循泛型类的确定方式
  class MyClass2<E> implements MyInterface<E>{
      @Override
      public void show(E s) {
      }
      @Override
      public void read(E s) {
      }
  }
  //c.放弃泛型
  class MyClass3 implements MyInterface{
      @Override
      public void show(Object s) {
      }
      @Override
      public void read(Object s) {
      }
  }

  ```

##### ==4.泛型统配符==

```java
什么是泛型通配符:
	可以代表任意一种泛型的符号,<?>
使用场景:
	当方法的参数需要接受相同的集合,但是集合的泛型不同时,那么参数的集合泛型就可以使用泛型通配符
	
public class TestDemo {
    public static void main(String[] args) {
        ArrayList<String> arr1 = new ArrayList<String>();
        ArrayList<Integer> arr2 = new ArrayList<Integer>();
        ArrayList<Double> arr3 = new ArrayList<Double>();

        //调用
        method(arr1);
        method(arr2);
        method(arr3);
        
//		在Java中这种写法是错误的,集合必须保证泛型一样才能赋值
//        ArrayList<Object> arr = new ArrayList<String>();
    }

    //定义方法,接收以上任意一个集合
    public static void method(ArrayList<?> arr) {

    }
}
```

##### ==5.泛型的上下限==

```java
泛型的上下限:
	泛型上限<? extends Person> :  代表泛型必须是Person本类,或者其子类
	泛型下限<? super Dog> : 代表泛型必须是Dog本类或者其父类
	
经典的案例:
public class TestGenericLimitDemo {
    public static void main(String[] args) {
        Collection<Integer> list1 = new ArrayList<Integer>();
        Collection<String> list2 = new ArrayList<String>();
        Collection<Number> list3 = new ArrayList<Number>();
        Collection<Object> list4 = new ArrayList<Object>();
        //Object
        //  String
        //  Number
        //      Integer
        getElement1(list1);
        getElement1(list2); //NO
        getElement1(list3);
        getElement1(list4); //NO

        getElement2(list1); //NO
        getElement2(list2); //NO
        getElement2(list3);
        getElement2(list4);
    }
    // 泛型的上限：此时的泛型?，必须是Number类型或者Number类型的子类
    public static void getElement1(Collection<? extends Number> coll){}
    // 泛型的下限：此时的泛型?，必须是Number类型或者Number类型的父类
    public static void getElement2(Collection<? super Number> coll){}
}

```

### 第四章 数据结构

##### 1.什么是数据结构

```java
什么是数据结构:
	是指某容器中如何保存数据的 
```

##### 2.常见的4+1种数据结构

```java
a.堆栈结构: 先进后出,后进先出
b.队列结构: 先进先出,后进后出
c.数组结构: 查询快,增删慢!
d.链表结构: 查询慢,增删快!
e.红黑树结构: 查询效率非常恐怖!!  
```

### ==第五章 List接口==

##### 1.List接口的特点

```java
a.有索引
b.有序: Java中有序不是指自然顺序,是指存入和取出的顺序一致 
c.可重复
```

##### 2.List接口中常用的方法以及常用子类

```java
1.List接口中的常用方法:
	a.由于List继承Collection,所有含有Collection中常用的7个方法和迭代器iterator方法
	b.List接口也有特有方法,四个和索引相关的特有方法
		增: public void add(int index,E 元素); //插入元素
		删: public E remove(int index); //删除指定索引下的元素,返回被删除的元素
		改: public E set(int index,E 新元素);//将指定索引出的元素改为新元素,返回被修改前的元素
		查: public E get(int index); //获取指定索引出的元素
		
public class TestListDemo {
    public static void main(String[] args) {
        //1。创建List接口的实现类
        ArrayList<String> arr = new ArrayList<String>();
        //2.增
        arr.add("java1");
        arr.add("java2");
        arr.add("java3");
        arr.add("java4");
        arr.add(1,"php");
        System.out.println(arr);
        //3.删
        String remove = arr.remove(3);
        System.out.println(remove);
        System.out.println(arr);
        //4.改
        String s1 = arr.set(2, "php");
        System.out.println(s1);
        System.out.println(arr);
        //5.查
        String s2 = arr.get(2);
        System.out.println(s2);
    }
}

2.List接口的常用子类
	ArrayList,LinkedList,Vector	
```

##### 3.ArrayList的数据结构以及使用

```java
ArrayList的底层采用的数据结构:数组结构,特点是查询快,增删慢!!!
ArrayList的特有方法: 
			无!!
```

##### 4.LinkedList的数据结构以及使用

```java
LinkedList的底层采用的数据结构:链表结构,特点是查询慢,增删快!!!
LinkedList的特有方法:大量和首尾操作相关的方法
	public void addFirst(E 元素); //添加首
	public void addLast(E 元素);	//添加尾
	
	public E removeFirst(); //删除首,返回被删除元素
	public E removeLast(); //删除尾,返回被删除元素

	public E getFirst(); //获取首
	public E getLast(); //获取尾

	public void push(E 元素); //push:推入 添加元素,添加到首位和addFirst完全一样
	public E pop(); //pop:推出 删除元素,删除首位元素,和removeFisrt完全一样

public class TestLinkedListDemo {
    public static void main(String[] args) {
        //1.创建集合
        LinkedList<String> link = new LinkedList<String>();
        //2.添加
        link.addLast("jack");
        link.addFirst("rose");
        link.addLast("tom");
        link.addFirst("james");
        link.addLast("marry");
        link.addFirst("jerry");
        //集合中现在的元素:
        //["jerry","james","rose","jack","tom","marry"]
        System.out.println(link);
        //3.删除
//        String first = link.removeFirst();
//        System.out.println(first);
//        System.out.println(link);

//        String last = link.removeLast();
//        System.out.println(last);
//        System.out.println(link);
        //4.获取
//        String first = link.getFirst();
//        System.out.println(first);
//        System.out.println(link);

//        String last = link.getLast();
//        System.out.println(last);
//        System.out.println(link);
        //5.push 推入 pop 推出
        link.push("中国");
        System.out.println(link);
        
        String pop = link.pop();
        System.out.println(pop);
        System.out.println(link);
    }
}
```

### 第六章 集合综合案例(斗地主基础版)

```java
斗地主步骤分析:
1.创建一副牌(54张)
2.洗牌(打乱集合中元素的顺序)
3.轮流发牌(遍历集合,剩下3张给底牌)
4.看牌(打印集合的信息)
  
public class DouDiZhuDemo {
    public static void main(String[] args) {
//        斗地主步骤分析:
//        1.创建一副牌(54张)
        //a.创建集合
        ArrayList<String> cardBox = new ArrayList<String>();
        //b.数值
        String[] nums = {"3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A", "2"};
        //c.花色
        String[] colors = {"♥", "♠", "♦", "♣"};
        //d.拼接
        for (String num : nums) {
            for (String color : colors) {
                //每一张牌
                String card = color + num;
                //添加到集合中
                cardBox.add(card);
            }
        }
        //e.单独添加大小王
        cardBox.add("小王吧");
        cardBox.add("大王吧");

//        2.洗牌(打乱集合中元素的顺序)
//        Collections 是集合工具类,专门操作集合的
        Collections.shuffle(cardBox);

//        3.轮流发牌(遍历集合,剩下3张给底牌)
        ArrayList<String> p1 = new ArrayList<String>();
        ArrayList<String> p2 = new ArrayList<String>();
        ArrayList<String> p3 = new ArrayList<String>();
        ArrayList<String> dp = new ArrayList<String>();
        //发牌
        for (int i = 0; i < cardBox.size() - 3; i++) {
            //取出牌
            String card = cardBox.get(i);
            //发给谁??
            // i = 0 3 6 9 p1
            // i = 1 4 7 10 p2
            // i = 2 5 8 11 p3
            if (i % 3 == 0) {
                p1.add(card);
            } else if (i % 3 == 1) {
                p2.add(card);
            } else {
                p3.add(card);
            }
        }

        //剩下三张给底牌
        dp.add(cardBox.get(51));
        dp.add(cardBox.get(52));
        dp.add(cardBox.get(53));

//        4.看牌(打印集合的信息)
        System.out.println("令狐冲:" + p1);
        System.out.println("任盈盈:" + p2);
        System.out.println("东方败:" + p3);
        System.out.println("底  牌:" + dp);
    }
}
```

### 总结:

```java
集合框架:
	Collection根接口
		方法: add,remove,clear,size,isEmpty,contains,toArray,iterator
	List子接口
    	特点: 有索引,有序,可重复
    	方法: add,remove,clear,size,isEmpty,contains,toArray,iterator
    		 4个特有方法:
				add(int index,E 元素); remove(int index);set(int index,E 元素);get(int index);
		
	ArrayList实现类(性能):
		采用数据结构:数组结构,所以查询快,增删慢
		特有方法: 无!!!

	LinkedList实现类:
		采用数据结构:链表结构,所以查询慢,增删快
		特有方法: 8个首尾相关
			addFirst,addLast,removeFirst,removeLast,getFirst,getLast,push,pop	
			
	Vector实现类(性能低):
		采用数据结构:数组结构,所以查询快,增删慢
		特有方法:无!!!

迭代器:
	是Collection接口中提出一种与索引无关的遍历方式
	使用步骤:
		Iterator<集合泛型> it = 集合对象.iterator();
		
		while(it.hasNext()){
         	集合泛型 元素 = it.next(); 
        }
增强for:
	底层就是迭代器
	使用格式:
		for(数据类型 变量名 : 集合/数组){
          	使用变量名即可
         }
	注意:增强for和迭代器都不允许在遍历集合的过程中进行元素的增删操作,
		否则抛出ConcurrentModificationException
泛型:
	泛型的上下限:
	<?>  任意泛型
	<? extends Xxx> 泛型上限,代表Xxx本类型或者其子类
	<? super Ooo> 泛型下限,代表Ooo本类型或者其父类

```

