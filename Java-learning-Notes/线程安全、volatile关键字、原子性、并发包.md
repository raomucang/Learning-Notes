## day08【线程安全、volatile关键字、原子性、并发包、死锁、线程池】

#### ==第一章 线程安全==

##### 1.1 线程安全问题出现的原因

```java
a.单线程永远不会有任何线程安全问题
b.多线程也不一定会有线程安全问题
c.必须满足:多个线程同时执行,执行同一个任务,操作同一个共享数据,此时才有可能有安全问题
```

##### ==1.2 线程安全问题的演示:卖票案例==

- 代码演示

```java
/**
 * 卖票任务
 */
public class Task implements Runnable {
    int count = 100;
    @Override
    public void run() {
        while (true) {
            if (count > 0) {
                try {
                    Thread.sleep(20);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "卖出第" + count + "张票");
                count--;
            } else {
                break;
            }
        }
    }
}

public class TestDemo {
    public static void main(String[] args) {
        //1.创建任务对象
        Task t = new Task();
        //2.创建线程1
        Thread t1 = new Thread(t,"小丽");
        //3.创建线程2
        Thread t2 = new Thread(t,"小美美");
        //4.创建线程3
        Thread t3 = new Thread(t, "古力娜扎");

        t1.start();
        t2.start();
        t3.start();
        //安全问题出现了:
        //a.导致出现重复数据
        //b.导致出现非法数据
    }
}
```

- 执行结果

```java
小丽卖出第100张票
小美美卖出第100张票
古力娜扎卖出第100张票
===========================
古力娜扎卖出第1张票
小美美卖出第0张票
小丽卖出第-1张票
```

- 产生重复数据的原因:

```java
当某个线程卖出xx张票之后,没有来得及总票数减1,CPU就切换到其他线程,导致其他线程也会卖出xx张票
```

- 产生非法数据0和-1的原因:

```java
只剩下最后一张票时,三个线程都判断完毕,进入卖票任务中,就会出现非法数据
```

##### 1.3 线程同步

- 什么是线程同步: 

  ```java
  让一段代码要么一次性执行完毕,要么不能进入执行
  ```

##### 1.4 三种线程同步的方式

- 同步代码块

  ```java
  格式:
  	synchronized(锁对象){
       	需要同步的代码 
      }

  /**
   * 卖票任务
   */
  public class Task implements Runnable {
      int count = 100;
      Object obj = new Object();
      @Override
      public void run() {
          while (true) {
            	//解决方案1:同步代码块
              synchronized (obj){
                  if (count > 0) {
                      System.out.println(Thread.currentThread().getName() + "卖出第" + count + "张票");
                      count--;
                  }
              }
          }
      }
  }
  ```

- 同步方法

  ```java
  格式:
  	public synchronized void 方法名(){
        	需要同步的代码 
      }

  /**
   * 卖票任务
   */
  public class Task implements Runnable {
      int count = 100;
      @Override
      public void run() {
          while (true) {
              sellTicket();
          }
      }
      //同步方法
      public synchronized void sellTicket() {
          if (count > 0) {
              System.out.println(Thread.currentThread().getName() + "卖出第" + count + "张票");
              count--;
          }
      }
  }

  注意:同步方法需要锁对象吗??? 需要!!! 
    	同步方法默认使用当前对象this作为锁对象,不需要我们提供
  ```

- Lock锁(最新的JDK1.5提供)

  ```java
  格式:
  	Lock lock = new ReentrantLock();
  	lock.lock();//加锁,获取锁
  		需要同步的代码;
  	lock.unlock();//解锁,释放锁

  /**
   * 卖票任务
   */
  public class Task implements Runnable {
      int count = 100;
      //创建一把Lock对象
      Lock lock = new ReentrantLock();
      @Override
      public void run() {
          while (true) {
              lock.lock();//获取锁
                  if (count > 0) {
                      System.out.println(Thread.currentThread().getName() + "卖出第" + count + "张票");
                      count--;
                  }
              lock.unlock();//释放锁
          }
      }
  }
  ```

#### 第二章 volatile关键字  

##### 2.1. 看程序说结果

- 示例代码

```java
public class VolatileThread extends Thread {
	// 定义成员变量
	private boolean flag = false ;
	public boolean isFlag() { return flag;}
	@Override
	public void run() {
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		// 将flag的值更改为true
		this.flag = true ;
		System.out.println("flag=" + flag);
	}
}
public class TestVolatileDemo {
    public static void main(String[] args) {
        // 创建VolatileThread线程对象
        VolatileThread volatileThread = new VolatileThread() ;
        volatileThread.start();


        //主线程中的代码
        while(true) {
            if(volatileThread.isFlag()) {
                System.out.println("执行了======");
            }
        }
    }
}
```

- 结果只有一句代码输出:flag=true

##### 2.2. JMM

- JMM是Java虚拟机提出一种Java内存模型.
  - JMM中规定 所有共享变量(成员变量和静态变量) 都保存在主内存
  - 当某个线程要使用该共享变量时,会在当前线程的工作内存中保存一个变量的副本

##### 2.3. 问题分析

```java
a.子线程从主内存中获取flag的值(false)
b.主线程也会从主内存中获取flag的值(false)
过了一秒中....
c.子线程修改工作内存中flag的值(true),并更新到主内存
d.主线程由于是死循环,速度是非常快速,所以没有机会去主内存中更新falg的值,所以一直是false,不会输出"执行了"
```

##### 2.4. 问题解决方案

- 加锁

  ```java
  public class TestVolatileDemo {
      public static void main(String[] args) {
          // 创建VolatileThread线程对象
          VolatileThread volatileThread = new VolatileThread() ;
          volatileThread.start();
  ```


          //主线程中的代码
          while(true) {
              synchronized (volatileThread){
                  if (volatileThread.isFlag()) {
                      System.out.println("执行了======");
                  }
              }
          }
      }
  }
  加锁后的执行流程:
  a.获取锁对象
  b.会清空主线程的工作内存
  c.主线程中获取flag由于工作内存中已经清空了,只能又从主内存中拷贝过来
  d.而1秒钟之后子线程修改的了falg值并更新到主线程了
  e.所以主线程中就可以获取更新后falg的值(true)
  f.那么输出"执行了"  
  ```

- volatile关键字

  ```java
  volatile关键字,称为可见关键字
  用来修饰成员变量,该成员变量变为可见

  public class VolatileThread extends Thread {
      // 给我们的共享变量falg,添加volatile修饰,那么该共享变量对所有子线程就可见
      private volatile boolean flag = false ;
      public boolean isFlag() { return flag;}
      @Override
      public void run() {
          try {
              Thread.sleep(1000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
          // 将flag的值更改为true
          this.flag = true ;
          System.out.println("flag=" + flag);
      }
  }

  public class TestVolatileDemo {
      public static void main(String[] args) {
          // 创建VolatileThread线程对象
          VolatileThread volatileThread = new VolatileThread();
          volatileThread.start();

          //主线程中的代码
          while (true) {
                  if (volatileThread.isFlag()) {
                      System.out.println("执行了======");
                  }
          }
      }
  }

  执行流程:
  a.子线程获取主内存的flag(false),放入其工作内存中
  b.主线程也获取主内存的flag(false),放入其工作内存中
  c.一秒之后,子线程更新了工作内存falg的值(true)更新到主内存中(true)
  d.由于flag是可见的,所有主线程的工作内存中的falg就是失效,重新从主内存中更新最新的flag(true)
  e.那么"执行了"就会输出  
  ```

- 加锁synchronized和可见性volatile的区别

  ```java
  a.volatile只能用于修饰成员变量或者类变量,synchronized修饰成员方法或者代码块
  b.synchronized加锁效率是比较低的,volatile并没有加锁只是增加共享变量的可见性所以效率依然很高
  c.volatile只是改变共享变量的可见性,但并不能保证原子性(不能保证多线程的安全),而synchronized是互斥操作,所以可以保证原子性(多线程保证安全的)
  ```

  ​


#### 第三章 原子性

##### 3.1. 看程序说结果

```java
public class VolatileAtomicThread implements Runnable {
    // 定义一个int类型的遍历
    private int count = 0 ;
    @Override
    public void run() {
// 对该变量进行++操作，1000次
        for(int x = 0 ; x < 1000 ; x++) {
            count++ ;//1.获取值 2.增加值 3.更新值
          	//count = count + 1
            System.out.println("count =========>>>> " + count);
        }
    }
}

public class TestVolatileAtomicThreadDemo {
    public static void main(String[] args) {
        //1.创建任务
        VolatileAtomicThread t = new VolatileAtomicThread();
        //2.创建100个线程
        // 开启100个线程对count进行++操作
        for(int x = 0 ; x < 100 ; x++) {
            new Thread(t).start();
        }
    }
}

结果:count =========>>>> 99998 少于 100000
```

##### 3.2. 问题原理说明 

```java
由于count++不是原子操作,是分为三步进行的:
a.先从主内存中获取count的副本,放入工作内存中
b.在工作内存中对count进行++操作
c.再将工作内存的count更新到主内存
由于以上三步不是原子操作,可能会被其他线程打断

```

![](atomic.png)

##### 3.3. volatile原子性测试

- volatile只具有可见性,也就说当主内存的共享数据改变了,会通知所有的子线程工作内存的副本进行更新

- 但是,它不具有原子性,也就说如果我们在工作内存中对变量的操作不是原子操作,那么volatile不能保持其原子性

  ```java
  public class VolatileAtomicThread implements Runnable {
      // 给成员变量添加了volatile关键字,但是依然不能保证原子性
      private volatile int count = 0;

      @Override
      public void run() {
  // 对该变量进行++操作，100次
          for (int x = 0; x < 1000; x++) {
              count++;
              System.out.println("count =========>>>> " + count);
          }
      }
  }

  public class VolatileAtomicThreadDemo {
      public static void main(String[] args) {
  // 创建VolatileAtomicThread对象
          VolatileAtomicThread volatileAtomicThread = new VolatileAtomicThread();
  // 开启100个线程对count进行++操作
          for (int x = 0; x < 100; x++) {
              new Thread(volatileAtomicThread).start();
          }
      }
  }

  最后的输出结果:
  count =========>>>> 99992
  count =========>>>> 99993
  count =========>>>> 99895
  count =========>>>> 99994
  count =========>>>> 99995
  依然不是100000,没有保证中的原子性
  ```

  ​

##### 3.4. 问题解决方案

- 加锁

  ```java
  public class VolatileAtomicThread implements Runnable {
      // 定义一个int类型的遍历
      private int count = 0;

      @Override
      public void run() {
  // 对该变量进行++操作，100次
          for (int x = 0; x < 1000; x++) {
              synchronized (this) {
                  count++;
                  System.out.println("count =========>>>> " + count);
              }
          }
      }
  }
  只要给count++加锁,那么只有获取到锁的线程执行完毕count++的所有操作之后,其他才能执行
  也就是说我们count++变成原子操作
  ```

- 原子类

  - AtomicInteger原子类

    构造方法:

    ```java
    public AtomicInteger()： 初始化一个默认值为0的原子型Integer
    public AtomicInteger(int initialValue)：初始化一个指定值的原子型Integer
    ```

    成员方法

    ```java
    int getAndIncrement(): 相当于 变量++,只是这个++是原子操作 
    int incrementAndGet(); 相当于 ++变量,只是这个++也是原子操作
    ```


  ###### 使用原子类改造案例

  ```java
  public class VolatileAtomicThread implements Runnable {
      // 定义一个int类型的遍历
  //    private int count = 0;
      private AtomicInteger count = new AtomicInteger();
      @Override
      public void run() {
  // 对该变量进行++操作，100次
          for (int x = 0; x < 1000; x++) {
                  count.getAndIncrement();//相当于 后++
                  System.out.println("count =========>>>> " + count);
          }
      }
  }
  ```


#### ==第四章 并发包==

##### ==4.1 ConcurrentHashMap== 

```java
a.HashMap,多线程情况下是不安全的
//所有线程对象,共享同一个Map
public class Const {
    public static HashMap<String,String> map = new HashMap<>();
}
//线程,线程中给Map添加50万个键值对
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 500000; i++) {
            Const.map.put(this.getName() + (i + 1), this.getName() + i + 1);
        }
        System.out.println(this.getName() + " 结束！");
    }
}
//测试类,创建两个子线程,一共向Map添加100万个键值对,但是线程执行完毕之后发现,Map中键值对往往小于100万
public class TestDemo {
    public static void main(String[] args) throws InterruptedException {
        MyThread a1 = new MyThread();
        MyThread a2 = new MyThread();
        a1.setName("线程1-");
        a2.setName("线程2-");
        a1.start();
        a2.start();

        //主线程休眠5秒,给以上两个子线程足够时间,让他们执行完毕
        for (int i = 0; i < 10; i++) {
            Thread.sleep(1000);
            System.out.println(i);
        }
        //打印Map中键值对的个数
        System.out.println("Map大小：" + Const.map.size());
    }
}
输出结果:
Map大小：957435


b.HashTable,多线程安全的一种map
//我们只是修改了共享的Map为HashTable,此时线程就是安全的
public class Const {
    public static Hashtable<String,String> map = new Hashtable<>();
}
输出结果:
Map大小：1000000

c.HashMap和HashTable相比
	HashMap是线程不安全的,HashTable是线程安全的,因为HashTable中一切操作数据的方法都加锁!!!

d.ConcurrentHashMap也能保证多线程的安全
	HashTable加锁是全局锁(把底层整个哈希表全部加锁)
  	ConcurrentHashMap加锁是局部锁(指针对要操作的桶进行局部加锁),所以效率比HashTable要高
public class Const {
    //ConcurrentHashMap底层是线程安全的
    public static ConcurrentHashMap<String,String> map = new ConcurrentHashMap<>();
}  	

输出结果:
Map大小：1000000
耗时结果:
Hashtable耗时 2600毫秒
ConcurrentHashMap耗时 1800毫秒
```

##### 4.2 CountDownLatch 

```java
CountDownLatch:允许一个或多个线程等待其他线程完成操作

构造方法:
	public CountDownLatch(int count);需要传入一个计数器
	
成员方法:
	public void await() throws InterruptedException// 让当前线程等待
     public void countDown();//让计数器-1  

public class Thread1 extends Thread {
    CountDownLatch down;
    public Thread1(CountDownLatch down){
        this.down = down;
    }
    @Override
    public void run() {
        System.out.println("A");
        //让当前线程等待即可
        try {
            down.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("C");
    }
}

public class Thread2 extends Thread {
    CountDownLatch down;
    public Thread2(CountDownLatch down){
        this.down = down;
    }
    @Override
    public void run() {
        System.out.println("B");
        //当前线程执行完毕之后,要计数器-1
        down.countDown();
    }
}

/**
 * 线程1输出A之后,等待线程2输出B,再回来执行输出C
 */
public class TestCountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        //0.创建一个CountDownLatch对象
        CountDownLatch down = new CountDownLatch(1);
        //1.创建两个线程
        Thread1 t1 = new Thread1(down);
        Thread2 t2 = new Thread2(down);
        //2.启动
        t1.start();
        //让线程1先执行
        Thread.sleep(1000);
        t2.start();
    }
}
```

##### 4.3 CyclicBarrier 

```java
CyclicBarrier:让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门

构造方法:
	public CyclicBarrier(int parties, Runnable barrierAction);创建一个屏障,指定线程个数,指定任务
	
成员方法:
	public int await();//通知屏障有线程到达了..
//员工线程
public class PersonThread extends Thread {
    CyclicBarrier barrier;

    public PersonThread(CyclicBarrier barrier) {
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            Thread.sleep(new Random().nextInt(5000));
            System.out.println(getName() + "已到达...");
            //通知屏障有线程达到了...
            barrier.await();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
//开会任务
public class MeetingTask implements Runnable {
    @Override
    public void run() {
        System.out.println("人都来了,开始吧....");
    }
}

public class TestCyclicBarrierDemo {
    public static void main(String[] args) throws InterruptedException {
        //0.创建屏障
        CyclicBarrier barrier = new CyclicBarrier(5, new MeetingTask());
        //1.创建五个人
        PersonThread p1 = new PersonThread(barrier);
        PersonThread p2 = new PersonThread(barrier);
        PersonThread p3 = new PersonThread(barrier);
        PersonThread p4 = new PersonThread(barrier);
        PersonThread p5 = new PersonThread(barrier);
        p1.start();
        p2.start();
        p3.start();
        p4.start();
        p5.start();
    }
}
```

##### 4.4 Semaphore 

```java
Semaphore:主要作用是控制线程的并发数量。 

构造方法:
	public Semaphore(int permits); 指定Semaphore中的许可的数量(同时执行的线程数量)
	public Semaphore(int permits,boolean fair);fait表示公平的,哪个线程先到那么先获取许可

成员方法:
	public void acquire() throws InterruptedException 表示获取许可
	public void release() 归还许可

//子线程
public class MyThread extends Thread {
    Semaphore sp;

    public MyThread(Semaphore sp) {
        this.sp = sp;
    }

    @Override
    public void run() {
        try {
            sp.acquire(); //获取Semaphore中的许可,如果许可不足,那么会等待
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("线程" + getName() + "开始了:" + System.currentTimeMillis());
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("线程" + getName() + "结束了:" + System.currentTimeMillis());
        sp.release(); //归还许可
    }
}

public class TestSemaphoreDemo {
    public static void main(String[] args) {
        //1.创建Semaphore
        Semaphore sp = new Semaphore(2);
        //2.创建五个线程
        MyThread m1 = new MyThread(sp);
        MyThread m2 = new MyThread(sp);
        MyThread m3 = new MyThread(sp);
        MyThread m4 = new MyThread(sp);
        MyThread m5 = new MyThread(sp);
        //3.开始
        m1.start();
        m2.start();
        m3.start();
        m4.start();
        m5.start();
    }
}
```

##### 4.5 Exchanger 

```java
Exchanger:用于线程间通信

构造方法:
	public Exchanger();创建一个线程间通信工具
成员方法:
	public V exchange(V x); 给其他线程传递数据,并获取其他线程转回的数据

//线程A
public class ThreadA extends Thread {
    private Exchanger<String> changer;
    public ThreadA(Exchanger<String> changer) {
        this.changer = changer;
    }
    @Override
    public void run() {
        System.out.println("线程A给线程B送礼物,等待线程B给线程A回礼...");
        //调用Exchange对象的exchange方法即可
        String result = null;
        try {
            result = changer.exchange("一个苹果");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("拿到了线程B的回礼:"+result);
    }
}

//线程B
public class ThreadB extends Thread {
    private Exchanger<String> changer;
    public ThreadB(Exchanger<String> changer) {
        this.changer = changer;
    }
    @Override
    public void run() {
        System.out.println("线程B给线程A送礼,等待线程A给线程B的回礼...");
        //调用Exchange对象的exchange方法即可
        String result = null;
        try {
            result = changer.exchange("一根香蕉");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("拿到了线程A的回礼:"+result);
    }
}

public class TestExchangerDemo {
    public static void main(String[] args) {
        //1.创建线程间通信工具
        Exchanger<String> changer = new Exchanger<String>();
        //2.线程A
        ThreadA ta = new ThreadA(changer);
        ta.start();
        ThreadB tb = new ThreadB(changer);
        tb.start();
    }
}
```

**总结:**

- ==能够解释安全问题的出现的原因==

  多个线程,同时执行一个任务,操作同一个共享数据,才有可能出现安全问题

- ==够使用同步代码块解决线程安全问题==

  ```java
  synchronized(任意对象){
   	需要同步的代码 
  }
  注意:锁对象可以是任意对象,但是必须是同一个对象
  ```

-  ==能够使用同步方法解决线程安全问题==

  ```java
  public synchronized void method(){
     需要同步的代码 
  }
  注意:同步方法也有锁对象,使用this作为锁对象
  	静态的同步方法,使用当前类的字节码文件作为锁对象
  ```

- ==能够使用Lock锁解决线程安全问题==

  ```java
  Lock lock = new ReentrantLock();
  lock.lock();
  	需要同步的代码 
  lock.unlock();
  ```

==能够说出volatile关键字的作用== 

​	保证主内存共享变量(成员变量,静态变量),对于所有线程来说是可见的

​	通俗讲: 当主内存中共享变量发生改变后,所有子线程中工作内存中的副本,都会重新从主内存中获取

能够说明volatile关键字和synchronized关键字的区别

​	volatile 只能保证共享变量的可见性,不能保证操作时的原子性

​	synchronized 加锁机制会清空工作内存的副本,导致重新从主内存中获取,而且可以保证操作的原子性

能够理解原子类的工作机制

​	底层是CAS机制,通俗讲: 从主内存获取数据,修改数据完毕之后,先和主内存数据比较,如果旧值和主内存是一样的,那么才更新到主内存,否则重新获取重新修改,再次比较,依次类推

- ==能够掌握原子类AtomicInteger的使用==
  - 构造方法:
    - public AtomicInteger(); 代表该对象默认值是0
    - public AtomicInteger(int value) 指定该对象的初始值
  - 成员方法:
    - public int getAndIncrement(); 先获取后增加1,相当于 变量++
    - public int incrementAndGet(); 先增加1后获取,相当于 ++变量
- ==能够描述ConcurrentHashMap类的作用==
  - 多线程情况保证安全性和高效性(HashMap不安全,Hashtable效率低)
- 能够描述CountDownLatch类的作用

  - 可以让某个线程,等待多个线程执行完毕之后才继续执行
- 能够描述CyclicBarrier类的作用

  - 可以等待多个线程执行完毕之后,再执行某个任务
- 能够表述Semaphore类的作用

  - 控制最多的线程并发数量
- 能够描述Exchanger类的作用

  - 用于线程间通信(数据交换)


