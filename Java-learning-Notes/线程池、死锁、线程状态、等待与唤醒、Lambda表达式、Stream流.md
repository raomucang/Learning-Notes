# day09【线程池、死锁、线程状态、等待与唤醒、Lambda表达式、Stream流】

### ==第一章 线程池==

##### 5.1 线程池的思想

```java
线程池实际上就是Java中一种容器,该容器保存多个线程,这些线程可以反复使用,省去了频繁创建和销毁线程的麻烦,从而提高性能
线程池所带来的好处:
a.减少资源消耗(减少创建和销毁的次数)
b.提高响应速度
c.增加线程的管理性
```

##### 5.2 线程池介绍

- 线程池的顶层接口

  ```java
  java.util.concurrent.Executor(线程执行器)
  ```

- 线程池的顶层接口子接口

  ```java
  java.util.concurrent.ExecutorService(顶层接口的子接口)
  线程池中有两个提交任务的方法:
  public Future<?> submit(Runnable task); 提交Runnable任务(只需要执行任务,没法返回任务的结果)
  public Future<?> submit(Callable task); 提交Callable任务(这是有返回值的任务)
  ```

- 创建一个线程池的实现类,是非常复杂的,我们使用Java提供的线程池工具类(静态工厂)

  ```java
  java.util.concurrent.Executors(工具类/静态工厂)
  ```

- 工具类Executors中创建线程池实现类方式

  ```java
  public static ExecutorService newFixedThreadPool(int nThreads);创建一个个数固定的线程池 
  ```

##### 5.3 线程池的使用

```java
 public class TestExecutorDemo {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
//         ============使用Runnable任务===========
        //1.创建一个线程池对象
        ExecutorService threadPool = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 10; i++) {
            //2.创建Runnable任务对象
            MyTask mt = new MyTask();
            //3.提交任务
            threadPool.submit(mt);
        }
        //4.关闭线程池
        threadPool.shutdown();

//         ============使用Callable任务===========
        //1.创建一个线程池对象
        ExecutorService threadPool = Executors.newFixedThreadPool(3);
        //2.创建Callable任务
        MyCall mc = new MyCall();
        //3.提交
        Future<String> future = threadPool.submit(mc);
        //4.取出future中的结果
        String result = future.get(); //get方法具有阻塞效果
        System.out.println("线程的执行结果为:" + result);
        //5.关闭线程池
        threadPool.shutdown();
    }
}
```

##### 5.4 线程池的练习

```java
/**
 * 需求: 使用线程池方式执行任务,返回1-n的和
 */
public class TestExecutorDemo02 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        //1.线程池
        ExecutorService threadPool = Executors.newFixedThreadPool(1);
        //2.使用匿名内部类创建Callable的实现类对象
        Callable<Integer> callable = new Callable<Integer>() {
            @Override
            public Integer call() throws InterruptedException {
                int sum = 0;
                for (int i = 1; i < 101; i++) {
                    Thread.sleep(30);
                    sum += i;
                }
                return sum;
            }
        };
        //3,提交任务
        Future<Integer> future = threadPool.submit(callable);
        //4.从future中取出结果
        Integer result = future.get();
        System.out.println("线程的执行结果为:"+result);
        //5.关闭线程池
        threadPool.shutdown();
    }
}
```

### ==第二章 死锁==

##### 6.1 什么是死锁

```java
死锁:程序中使用多把锁,造成不同的线程相互等待,程序无法继续执行...
```

##### 6.2 产生死锁的条件

```java
a.至少2把锁
b.至少2个线程
c.至少有同步代码块的2层嵌套
```

##### 6.3 死锁演示

```java
public class TestDeadLockDemo {
    public static void main(String[] args) {
        //1.创建两把锁对象
        Object a = new Object();
        Object b = new Object();

        //2.创建两个线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (a){
                        System.out.println("线程1获取了a锁,还要获取b锁");
                        synchronized (b) {
                            System.out.println("线程1又获取的b");
                            System.out.println("线程1的任务执行了....");
                        }
                    }
                }
                
            }
        }).start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (b) {
                        System.out.println("线程2获取了b锁,还要获取a锁");
                        synchronized (a) {
                            System.out.println("线程2又获取了a锁");
                            System.out.println("线程2的任务执行了..");
                        }
                    }
                }
            }
        }).start();
    }
}

死锁:只能尽量避免
```



### ==第三章 线程的状态==

##### 1.线程的六种状态

![ ](ThreadState.png)



- 新建状态(New)

  ```java
  刚刚创建但是没有调用start方法的线程
  ```

- 可运行状态(Runnable)

  ```java
  刚刚创建的线程调用start方法之后
  ```

- 受(锁)阻塞状态(Blocked)

  ```java
  当前线程需要锁对象,但是锁对象被其他线程持有
  ```

- 限时等待状态(Timed_waiting)

  ```java
  我们也可以称为"休眠",在线程中调用Thread.sleep(毫秒值)方法
  ```

- ==无限等待状态(Waiting)==

  - 线程如何进入Waiting(无限等待状态)

    ```java
    a.当前线程先持有锁对象
    b.调用锁对象的wait方法
    c.当前线程会自动释放锁对象,然后进入无限等待状态
    ```

  - 其他线程如何唤醒Waiting状态的线程

    ```java
    a.其他线程下先持有锁对象(必须无限等待线程释放的那个锁对象)
    b.调用锁对象的notify方法
    c.被唤醒的线程先进入锁阻塞状态,直到其他线程释放锁对象,被唤醒线程再次持有锁对象,才进入可运行状态
    ```

- 消亡状态(Terminated)

  ```java
  当线程的任务执行完毕 
  ```

### ==第四章 等待唤醒机制==

##### 1.等待唤醒机制(Wait和Notify)

```java
public class WaitAndNotifyDemo {
    public static void main(String[] args) throws InterruptedException {
        //1.创建一把锁对象
        Object obj = new Object();
        //2.创建线程,演示进入无限等待..
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("线程1开始执行了...");
                synchronized (obj) {
                    System.out.println("线程1抢到锁了..");
                    System.out.println("线程1进入无限等待了..");
                    try {
                        obj.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("线程1从无限等待中醒来了...");
                }
            }
        }).start();

        for (int i = 0; i < 5; i++) {
            Thread.sleep(500);
            System.out.println("i = " + i);
        }

        //3.创建线程,唤醒无限等待的线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("线程2开始执行了..");
                synchronized (obj) {
                    System.out.println("线程2拿到锁对象了...");
                    System.out.println("把那傻叉叫醒了...");
                    obj.notify();
                    try {
                      	//注意事项:线程2唤醒线程1之后,线程1并不能立刻进入可运行状态
                      	//因为线程1此时没有锁对象,会进入锁阻塞状态
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("线程2结束了...");
                  	//直到线程2执行完毕,释放锁对象,线程1才能再次获取锁对象,进入可运行状态
                }
            }
        }).start();
    }
}
```

##### 2.生产者与消费者问题(代码演示)

```java
public class TestProducerAndConsumer {
    public static void main(String[] args) {
        //0.创建一个锁对象
        Object obj = new Object();
        //0.创建一个盘子对象
        PanZi pz = new PanZi();
        //1.生产者线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (obj) {
                        //判断是否有包子
                        if (pz.flag) {
                            //生产者无限等待
                            try {
                                obj.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                        //没有包子
                        System.out.println("生产者发现没有包子,开始做包子...");
                        pz.flag = true;
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println("生产者说:包子好了,吃货来吧...");
                        obj.notify();
                    }
                }
            }
        }).start();

        //2.消费者线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    synchronized (obj) {
                        //判断是否没有包子
                        if (!pz.flag){
                            try {
                                obj.wait();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                        //有包子
                        System.out.println("包子真好吃...");
                        pz.flag = false;
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println("消费者说:吃完了赶紧给老子再做一个...");
                        obj.notify();
                    }
                }
            }
        }).start();
    }
}
```

### 第五章 Lambda表达式

##### 2.1 函数式编程的思想

```java
函数式思想主要强调三个方面:
a.输入量(参数)
b.计算方案(方法体)
c.输出量(返回值)
而不注重具体的形式(省略面向对象中定义方法的各种格式)
目的:让代码更加简洁  
```

##### 2.2 冗余的Runnable代码

```java
public class TestLambdaDemo {
    public static void main(String[] args) {
        //创建线程,使用面向对象的思想
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("线程的名字:"+Thread.currentThread().getName());
            }
        }).start();
        //分析:传统面向对象中冗余代码
        //1.为了节省创建一个新的类,我们不得不用匿名内部类
        //2.由于面向对象的语法束缚,我们不得不用Runnable这个接口
        //3.由于面向对象的语法束缚,我们不得不创建接口的实现类对象
        //4.由于面向对象的语法束缚,我们不得不重写叫run的方法
        //5.以上代码似乎只有任务才是我们自己想要的
      	//  而其他都是我们不得不按照面向对象的要求写的固定格式(冗余代码)
    }
}
```

##### 2.3 函数式编程Lambda的体验

```java
//使用Lambda修改创建线程
new Thread(()->{System.out.println("线程的名字:"+Thread.currentThread().getName());}).start();
```

##### 2.4 Lambda标准格式介绍

```java
"(参数)->{方法体;return 返回值;}
相当于是对面向对象中方法的简洁格式

/**
 * 做饭接口
 */
public interface Cook {
    void cook();
}
public class TestLambdaDemo {
    public static void main(String[] args) {
        //调用
        //使用实现类对象
//        method(new Cook的实现类());
        //使用匿名内部类对象
        method(new Cook() {
            @Override
            public void cook() {
                System.out.println("香喷喷的大米饭做好了...");
            }
        });
        //使用Lambda
        method(()->{System.out.println("香喷喷的大米饭做好了...");});
    }
    /**
     * 定义一个方法:请一个会做饭的过来做饭
     */
    public static void method(Cook cc) {
        cc.cook();
    }
}
```

##### 2.5 Lambda的参数和返回值

```java
public class Dog {
    int age;
    String name;
    int legs;
    public Dog(int age, String name, int legs) {
        this.age = age;
        this.name = name;
        this.legs = legs;
    }
    @Override
    public String toString() {....}
}

public class TestLambdaDemo {
    public static void main(String[] args) {
        //1.比较器排序 工具类Arrays
        Integer[] nums = {5, 7, 9, 8, 1, 4, 2, 3, 6};
        //2.排序
//        Arrays.sort(nums, new Comparator<Integer>() {
//            @Override
//            public int compare(Integer o1, Integer o2) {
//                return o2 - o1;
//            }
//        });
        //3.使用Lambda修改
        Arrays.sort(nums, (Integer o1, Integer o2) -> { return o2 - o1; });
        System.out.println(Arrays.toString(nums));

        //4.比较自定义类型
        Dog[] dogs = new Dog[4];
        dogs[0] = new Dog(1, "jack", 3);
        dogs[1] = new Dog(2, "marry", 2);
        dogs[2] = new Dog(4, "ady", 1);
        dogs[3] = new Dog(3, "tom", 5);

        //排序,
//        Arrays.sort(dogs, new Comparator<Dog>() {
//            @Override
//            public int compare(Dog o1, Dog o2) {
//                //按照Dog腿数,降序
//                return o2.legs-o1.legs;
//            }
//        });

        //使用Lambda简化
        Arrays.sort(dogs,(Dog o1, Dog o2) -> { return o2.legs-o1.legs; });

        for (Dog dog : dogs) {
            System.out.println(dog);
        }
    }
}
```

##### 2.6 Lambda的省略格式

```java
a.参数的类型可以无条件省略
b.如果参数只有一个,那么小括号可以省略
c.如果大括号中只有一句代码,那么大括号,return关键字,以及后面分号可以同时省略

案例1:
//使用Lambda修改创建线程
new Thread(()->{System.out.println("线程的名字:"+Thread.currentThread().getName());}).start();
//省略格式
new Thread(()->System.out.println("线程的名字:"+Thread.currentThread().getName())).start();

案例2:
//使用Lambda
method(()->{System.out.println("香喷喷的大米饭做好了...");});
//省略格式
method(()->System.out.println("香喷喷的大米饭做好了..."));

案例3:
//3.使用Lambda修改
Arrays.sort(nums, (Integer o1, Integer o2) -> { return o2 - o1; });
//4.省略格式
Arrays.sort(nums, (o1, o2) -> o2 - o1 );

案例4:
//使用Lambda简化
Arrays.sort(dogs,(Dog o1, Dog o2) -> { return o2.legs-o1.legs; });
//省略格式
Arrays.sort(dogs,(o1, o2) -> o2.legs-o1.legs);
```

##### 2.7 强烈注意:Lambda的使用前提

```java
a.Lambda只能用于替代函数式接口(有且仅有一个抽象方法的接口)的匿名内部类
b.Lambda的省略格式只有以上三种情况,其他的一律不能省略(可推导即可省略)
```

### 第六章 Stream流

##### 1.引入:传统的集合操作

```java
案例:对以下集合进行三步操作:
1. 首先筛选所有姓张的人；
2. 然后筛选名字有三个字的人；
3. 最后进行对结果进行打印输出

public class ArrayListDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("张无忌");
        list.add("周芷若");
        list.add("赵敏");
        list.add("张强");
        list.add("张三丰");
//        1. 首先筛选所有姓张的人；
        ArrayList<String> one = new ArrayList<String>();
        for (String name : list) {
            if (name.startsWith("张")) {
                one.add(name);
            }
        }
//        2. 然后筛选名字有三个字的人；
        ArrayList<String> two = new ArrayList<String>();
        for (String name : one) {
            if (name.length() == 3) {
                two.add(name);
            }
        }
//        3. 最后进行对结果进行打印输出
        for (String name : two) {
            System.out.println(name);
        }
    }
}
```

##### 2.循环遍历的弊端分析

```java
代码不够简洁,更加注重形式,固定的格式,使用Stream流的思想,就可以简化以上代码
```

##### 3.体验Stream的优雅写法

```java
//体验Stream流中的优雅代码
list.stream().filter(s -> s.startsWith("张"))
  			.filter(s -> s.length() == 3).forEach(s -> System.out.println(s));
```

##### 4.流式思想的概述

![](流式思想.png)

##### 5.两种获取流的方式

```java
a.集合获取流
	Collection集合获取
	调用: public Stream<T> stream():
b.数组获取流
	调用:public static Stream of(T... args);

public class StreamGetDemo {
    public static void main(String[] args) {
        //1.集合获取流(单列集合)
        ArrayList<String> arr1 = new ArrayList<String>();
        Stream<String> stream1 = arr1.stream();

        HashSet<Integer> arr2 = new HashSet<Integer>();
        Stream<Integer> stream2 = arr2.stream();
        
        //2.数组获取流
        Stream<String> stream3 = Stream.of("jack","marry","tom","rose");
        //或者
        String[] names = {"jack", "marry", "tom", "rose"};
        Stream<String> stream4 = Stream.of(names);
    }
}
```

##### 6.Stream流中的常用方法

```java
//1.数组
String[] names = {"jack", "ady", "rose", "jerry", "tom", "lilei", "hanmeimei"};
//2.获取流
Stream<String> stream1 = Stream.of(names);
```

- 逐个处理:forEach(代码演示)


```java
//3.foreach 逐一处理
//stream1.forEach(new Consumer<String>() {
//  	@Override
// 	public void accept(String s) {
//    	System.out.println(s);
//  	}
//});
stream1.forEach(s -> System.out.println(s));
```

- b.统计个数:count(代码演示)


```java
//4.count 统计个数
long count = stream1.count();
System.out.println(count);
```

- ==过滤:filter(代码演示)==

```java
//5.filter 过滤方法
//        Stream<String> stream2 = stream1.filter(new Predicate<String>() {
//            @Override
//            public boolean test(String s) {
//                //要长度大于4的字符串
//                return s.length() > 4;
//            }
//        });
//Lambda
Stream<String> stream2 = stream1.filter(s -> s.length() > 4);
```

- 取前几个:limit(代码演示)

```java
//6.limit 取前几个
Stream<String> stream3 = stream1.limit(4);
System.out.println(stream3.count());
```

- 跳过前几个:skip(代码演示)

```java
//7.skip 跳过前几个
Stream<String> stream4 = stream1.skip(3);
System.out.println(stream4.count());
```

- ==映射方法:map(代码演示)==

```java
//8.map 映射方法
//        Stream<Integer> stream5 = stream1.map(new Function<String, Integer>() {
//            @Override
//            public Integer apply(String s) { //把字符串映射为字符串首字母的码值
//                return (int)s.charAt(0);
//            }
//        });
//Lambda
Stream<Integer> stream5 = stream1.map(s -> (int) s.charAt(0));
stream5.forEach(i -> System.out.println(i));
```

- 静态方法合并流:concat(代码演示)

```java
Stream<String> one = Stream.of("jack", "rose");
Stream<String> two = Stream.of("tom", "jerry");
Stream<String> all = Stream.concat(one, two);
all.forEach(s-> System.out.println(s));
注意:合并的两个流中的数据类型可以不一致,最终得到流的泛型应该是他们的共同父类
```

##### ==7.练习:集合元素的处理(Stream方式)==

```java
public class StreamTestDemo {
    public static void main(String[] args) {
        List<String> one = new ArrayList<>();
        one.add("迪丽热巴");
        one.add("宋远桥");
        one.add("苏星河");
        one.add("老子");
        one.add("庄子");
        one.add("孙子");
        one.add("洪七公");
        List<String> two = new ArrayList<>();
        two.add("古力娜扎");
        two.add("张无忌");
        two.add("张三丰");
        two.add("赵丽颖");
        two.add("张二狗");
        two.add("张天爱");
        two.add("张三");
//        1. 第一个队伍只要名字为3个字的成员姓名；
//        2. 第一个队伍筛选之后只要前3个人；
        Stream<String> oneStream = one.stream().filter(s -> s.length() == 3).limit(3);
//        3. 第二个队伍只要姓张的成员姓名；
//        4. 第二个队伍筛选之后不要前2个人；
        Stream<String> twoStream = two.stream().filter(s -> s.startsWith("张")).skip(2);

//        5. 将两个队伍合并为一个队伍；
        Stream<String> allStream = Stream.concat(oneStream, twoStream);
//        6. 根据姓名创建 Person 对象；
        //将姓名(String) 映射为Person类型
        Stream<Person> personStream = allStream.map(s -> new Person(s));
//        7. 打印整个队伍的Person对象信息。
        personStream.forEach(p-> System.out.println(p));
    }
}

```

##### 8.总结:函数拼接和终结方法

```java
函数拼接方法:调用该方法后,返回的还是流对象
	filter,limit,skip,map,concat
"拼接方法,由于返回的还是流对象,所以支持链式编程	

终结方法:调用该方法后,返回的不是流对象或者无返回值
	foreach,count
"终结方法,一个流只能调用一次,调用完之后流就关闭了	
```

##### 9.收集Stream的结果

```java
public class StreamCollectDemo {
    public static void main(String[] args) {
        //1.数组
        String[] names = {"jack", "ady", "rose", "jerry", "tom", "lilei", "hanmeimei"};
        //2.获取流
        Stream<String> stream1 = Stream.of(names);
        //3.收集流中的数据
      	//收集到List集合中
        List<String> list = stream1.collect(Collectors.toList());
        System.out.println(list);
		//收集到Set集合中
        Set<String> set = stream1.collect(Collectors.toSet());
        System.out.println(set);
    }
}
```

