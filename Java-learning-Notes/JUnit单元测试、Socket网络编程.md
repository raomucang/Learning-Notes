## day12【JUnit单元测试、网络编程】

### 第一章.Junit单元测试

##### 1.什么是单元测试

```java
单元测试通俗讲就是取代main方法的一种技术
```

##### 2.Junit的使用步骤

- 下载

  ```java
  www.junit.org

  junit是第三方小框架,但是由于使用频率很高,所以绝大多数开发工具都内置了,IDEA也内置了Junit
  ```


- ==具体使用步骤==

  ```java
  a.先编写被测试类(业务类)
  b.编写测试类(用来测试业务类)
  c.写一个普通方法.加上@Test这个注解  
  ```

- 运行测试

  ```java
  选中要测试的方法右键运行即可
  选中整个测试类右键运行即可

  /**
   * 计算器类
   */
  public class Calculate {
      public int getSum(int a, int b) {
          return a + b;
      }
  }
  public class TestCalculate {
      @Test
      public void test01() {
          Calculate cc = new Calculate();
          int sum = cc.getSum(10, 20);
          System.out.println("结果为:" + sum);
      }
  ```


      @Test
      public void test02() {
          Calculate cc = new Calculate();
          int sum = cc.getSum(100, 200);
          System.out.println("结果为:" + sum);
      }
  }
  注意:测试方法一般来说都是无参数无返回值
  ```

##### 3.单元测试中其他四个注解

- Junit4.x

  ```java
  @Before 该方法在每个测试方法执行之前执行
  @After 该方法在每个测试方法执行之后执行

  @BeforeClass 该方法在所有测试方法执行之前执行
  @AfterClass 该方法在所有测试方法执行之后执行
  ```

- Junit5.x

  ```java
  @BeforeEach：相当于4.x中@Before
  @AfterEach： 相当于4.x中@After
  @BeforeAll:  相当于4.0中@BeforeClass
  @AfterAll:   相当于4.0中@AfterClass  
  ```

### 第二章 网络编程入门

##### 1.软件架构介绍

```java
a.C/S模式: Client(客户端)/Server(服务器端)
  	比如: 淘宝,京东,QQ

b.B/S模式:Browser(浏览器)/Server(服务器端)
   比如: 淘宝,京东,QQ
```

##### 2.网络通信协议介绍

```java
网络通信协议: 互联网上所有的计算机进行数据交互需要遵循的规则
```

##### 3.常用的通信协议

```java
IP协议: 规定每台连接到网络的计算机IP地址
TCP协议:传输控制协议,特点:面向有链接(通过"三次握手"建立连接)
  		TCP协议优点: 数据安全,且完整
  		TCP协议缺点: 性能较低
	
UDP协议:用户数据报协议,特点:面向无连接
		UDP协议优点:性能高
		UDP协议缺点:数据不安全,不完整 
```

##### ==4.网络编程的三要素==

```java
网络通信协议
IP地址:互联网上每一台计算机的唯一标识(相当于计算机的"身份证号")
端口号:一台计算机上不同应用程序的唯一标识(计算机中一共有0-65535端口号,一般来说我们只用1024以上的)
```

##### 扩展:

##### 扩展:

- IP地址的未来:

  ```java
  目前用的IP协议称为IPv4,规定IP是32位二进制:0010 1010 0110 1010 1010 0110 1010 0110
    									192		.	168		.	100		.222
  IPV4一共有多少种: 大概43亿种(面临枯竭)
  最新的IP协议称为IPV6,规定IP是128位二进制:
  IPV6一共有多少种: 号称可以给全球每一粒沙子分配一个IP,且不重复
  ```

- IP地址的获取和测试(Dos命令)

  ```java
  ipconfig:查看本机IP地址
  ping 对方的IP: 查看本机和对方是否网络通畅 
  ```

- 特殊的IP地址

  ```java
  本机回环地址:127.0.0.1(本机),localhost
  ```

### 第三章 UDP通信

##### 1.UDP协议概述

```java
特点:面向无连接
优点:性能高
缺点:数据不安全,不完整
```

##### 2.UDP的发送端类和数据包类

```java
DatagramSocket: UDP协议中发送数据和接收数据的类
		构造方法:
		public DatagramSocket();无参构造,系统会随机分配一个端口,只能用于发送端
		public DatagramSocket(int port);有参构造,指定某个端口,一般用接收端
		成员方法:
		public void send(DatagramPacket dp);//发送数据报
		public void receive(DatagramPacket dp);//接收数据报
		public void close();//释放资源

DatagramPacket: UDP协议中保存数据的类
		构造方法
		public DatagramPacket(byte[] buf, int length, InetAddress address, int port)
		public DatagramPacket(byte[] buf, int length)
  		成员方法:
		public int getLength();获取数据报中数据的长度
```

##### 3.UDP通信案例

```java
/**
 * UDP发送端
 */
public class UDPSender {
    public static void main(String[] args) throws IOException {
        //1.创建数据报发送器对象
        DatagramSocket ds = new DatagramSocket();

        //2.创建数据报对象
        byte[] bs = "你好,中午吃什么?".getBytes();
        DatagramPacket dp = new DatagramPacket(bs,bs.length,InetAddress.getByName("127.0.0.1"),1234);

        //3.调用ds的发送方法
        ds.send(dp);
        System.out.println("数据报发送完毕...");
        //4.释放资源
        ds.close();
    }
}
/**
 * UDP接收端
 */
public class UDPReceiver {
    public static void main(String[] args) throws IOException {
        //1.创建UDP数据报接收器对象
        DatagramSocket ds = new DatagramSocket(1234);

        //2.创建UDP数据报对象
        DatagramPacket dp = new DatagramPacket(new byte[1024], 1024);

        //3.调用ds的接收数据方法
        System.out.println("等待发送端的数据...");
        ds.receive(dp); //receive是阻塞,直到收到数据才能向下执行
        System.out.println("接收到了发送端的数据...");
        //打印接收到的数据
        int len = dp.getLength();//接收到的字节数
        byte[] bs = dp.getData();//获取数据报中的字节数组
        System.out.println("发送端发来贺电:" + new String(bs, 0, len));

        //4.释放资源
        ds.close();
    }
}
```



### 第四章 TCP通信

##### 1.TCP通信分为客户端和服务器

```java
客户端:一般是指个人电脑,手机,Ipad
服务器:一般是公司配置比较高的电脑
```

##### 2.TCP中的两个重要的类

```java
客户端的代表类: Socket(套接字)
服务器的代表类: ServerSocket(服务器套接字)
```

##### 3.Socket类的介绍和使用

- 构造方法

  ```java
  public Socket(String ip,int port); 创建客户端对象,指定该客户端需要连接的服务器IP和端口号
  此构造非常牛逼:
  	该构造方法会根据参数,自动连接服务器(TCP协议的三次握手),如果连接成功,那么对象才创建成功
  	如果连接失败,对象不会创建成功而是直接抛出异常
  ```

- 常用方法

  ```java
  public OutputStream getOutputStream();//获取连接通道中的输出流
  public InputStream getInputStream();//获取连接通道中的输入流

  public void shutDownOutput();//关闭连接通道中的输出流
  public void shutDownInput();//关闭连接通道中的输入流

  public void close();//关闭客户端
  ```

##### 4.ServerSocket类的介绍和使用

- 构造方法

  ```java
  public ServerSocket(int port);//创建服务器对象,指定服务器运行的端口号
  ```

- 常用的成员方法

  ```java
  public void close();//关闭服务器
  public Socket accept();//接收某个客户端的连接
  ```

##### ==5.简单的TCP通信实现(单向通信)==

```java
/**
 * TCP通信客户端
 */
public class TCPClient {
    public static void main(String[] args) throws IOException {
        //1.创建一个客户端对象
        Socket socket = new Socket("127.0.0.1", 11111);
        System.out.println("成功连接..");
        //2.获取通道中的输出流
        OutputStream out = socket.getOutputStream();
        //3.调用out的write方法,给服务器发数据
        out.write("你好,我是你大爷!!!".getBytes());
        System.out.println("成功发送...");
        //4.释放资源
        out.close();
        socket.close();
        System.out.println("成功关闭...");
    }
}

/**
 * TCP通信服务器端
 */
public class TCPServer {
    public static void main(String[] args) throws IOException {
        //1.创建一个服务器对象
        ServerSocket server = new ServerSocket(11111);
        System.out.println("服务器启动成功...");
        //2.获取连接到服务器的客户端对象
        System.out.println("等待客户端连接..");
        Socket socket = server.accept();//accept具有阻塞功能,直到有客户端连接
        System.out.println("有客户端连接:"+socket.getInetAddress().getHostAddress());
        //3.获取通道中的输入流
        InputStream in = socket.getInputStream();
        //4.调用in的read方法,读取客户端发送的数据
        byte[] bs = new byte[1024];
        int len = in.read(bs);
        System.out.println("客户端说:" + new String(bs, 0, len));
        //5.释放资源
        in.close();
        socket.close();
        server.close();
        System.out.println("服务器关闭成功...");
    }
}
```

##### ==6.简单的TCP通信实现(双向通信)==

```java
/**
 * TCP通信客户端
 */
public class TCPClient {
    public static void main(String[] args) throws IOException {
        //1.创建一个客户端对象
        Socket socket = new Socket("127.0.0.1", 11111);
        System.out.println("成功连接..");
        //2.获取通道中的输出流
        OutputStream out = socket.getOutputStream();
        //3.调用out的write方法,给服务器发数据
        out.write("你好,我是你大爷!!!".getBytes());
        System.out.println("成功发送...");
        //==================以下是双向通信新增代码====================
        //4.获取通道中的输入流
        InputStream in = socket.getInputStream();
        //5.调用in的read方法,读取数据
        byte[] bs = new byte[1024];
        int len = in.read(bs);
        System.out.println("服务器反馈信息:" + new String(bs, 0, len));
        //==================以上是双向通信新增代码====================
        //4.释放资源
        in.close();
        out.close();
        socket.close();
        System.out.println("成功关闭...");
    }
}

/**
 * TCP通信服务器端
 */
public class TCPServer {
    public static void main(String[] args) throws IOException {
        //1.创建一个服务器对象
        ServerSocket server = new ServerSocket(11111);
        System.out.println("服务器启动成功...");
        //2.获取连接到服务器的客户端对象
        System.out.println("等待客户端连接..");
        Socket socket = server.accept();//accept具有阻塞功能,直到有客户端连接
        System.out.println("有客户端连接:"+socket.getInetAddress().getHostAddress());
        //3.获取通道中的输入流
        InputStream in = socket.getInputStream();
        //4.调用in的read方法,读取客户端发送的数据
        byte[] bs = new byte[1024];
        int len = in.read(bs);
        System.out.println("客户端说:" + new String(bs, 0, len));
        //=====================以下是双向通信新增代码=========================
        //5.获取通道中的输出流
        OutputStream out = socket.getOutputStream();
        //6.调用out的writer方法,发送数据
        out.write("您好,您的消息收到了,安息吧...".getBytes());
        System.out.println("成功给客户端反馈信息...");
        //=====================以上是双向通信新增代码=========================
        //7.释放资源
        out.close();
        in.close();
        socket.close();
        server.close();
        System.out.println("服务器关闭成功...");
    }
}
```



### 第五章 综合案例:文件上传

##### 1.文件上传案例分析(画图演示)

![](TCP文件上传.png)

##### 2.文件上传案例实现(代码演示)

```java
/**
 * TCP文件上传客户端
 */
public class TCPFileUploadClient {
    public static void main(String[] args) throws IOException {
//        1.创建Socket对象
        Socket socket = new Socket("127.0.0.1", 8888);
        System.out.println("连接服务器成功...");
//        2.获取通道中的输出流(1号流)
        OutputStream out = socket.getOutputStream();
//        3.创建文件的字节输入流
        FileInputStream fis = new FileInputStream("G:\\upload\\444.png");
//        4.循环:一边读文件,一边发数据
        System.out.println("正在上传图片...");
        byte[] bs = new byte[1024];
        int len = 0;
        while ((len = fis.read(bs)) != -1) {
            out.write(bs, 0, len);
        }
        System.out.println("图片上传完毕...");
//        5.释放资源
        fis.close();
        out.close();
        socket.close();
        System.out.println("客户端成功关闭...");
    }
}

/**
 * TCP文件上传服务器
 */
public class TCPFileUploadServer {
    public static void main(String[] args) throws IOException {
//        1.创建ServerSocket对象
        ServerSocket server = new ServerSocket(8888);
        System.out.println("服务器启动成功....");
//        2.接收客户端对象
        System.out.println("等待客户端连接...");
        Socket socket = server.accept();
        System.out.println("有客户端连接:" + socket.getInetAddress().getHostAddress());
//        3.获取通道中的输入流(1号流)
        InputStream in = socket.getInputStream();
//        4.创建文件的字节输出流
        FileOutputStream fos = new FileOutputStream("copy.png");
//        5.循环:一边读数据,一边写入文件中
        System.out.println("正在接收文件...");
        byte[] bs = new byte[1024];
        int len = 0;
        while ((len = in.read(bs)) != -1) {
            fos.write(bs, 0, len);
        }
        System.out.println("接收文件成功...");
//        6.释放资源
        fos.close();
        in.close();
        socket.close();
        server.close();
        System.out.println("服务器成功关闭...");
    }
}
```

##### 3.文件上传案例实现的不足之处

```java
a.服务器接收后的文件名是固定的,导致无论上传多少张图片,只会保留最后一张
b.每次启动服务器,只能服务器一个客户端,完毕之后服务器关闭了
c.如果某个客户端上传的文件很大,那么和该客户端进行交互的时间会很长,导致无法接收下一个客户端
d.服务器应该在接收文件完毕之后,给客户端反馈信息(双向通信)
```

##### 4.文件上传案例的优化实现(代码演示)

```ava
问题:
	a.服务器接收后的文件名是固定的,导致无论上传多少张图片,只会保留最后一张
解决:
	a.将当前时间的毫秒值作为文件名,这样每次文件名都是不一样的
	
问题:
	b.每次启动服务器,只能服务器一个客户端,完毕之后服务器关闭了
解决:
	b.永不停止的服务器(给服务器添加一个无线循环即可)
	
问题:
	c.如果某个客户端上传的文件很大,那么和该客户端进行交互的时间会很长,导致无法接收下一个客户端
解决:
	c.给服务器写一个多线程版本,每次来一个客户端开启一个线程与客户端交互,主线程继承等待下一个客户端

问题:
	d.服务器应该在接收文件完毕之后,给客户端反馈信息(双向通信)
解决:
	d.客户端文件上传完毕之后,关闭通道中的输出流,调用socket对象的shutDownOutput方法
```

```java
服务器优化后的代码
/**
 * TCP文件上传服务器
 */
public class TCPFileUploadServer {
    public static void main(String[] args) throws IOException, InterruptedException {
//        1.创建ServerSocket对象
        ServerSocket server = new ServerSocket(8888);
        System.out.println("服务器启动成功....");
        while (true) {
            //        2.接收客户端对象
            System.out.println("等待客户端连接...");
            Socket socket = server.accept();
            System.out.println("有客户端连接:" + socket.getInetAddress().getHostAddress());
            //以下代码就是和这个客户端进行交互
            new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        //        3.获取通道中的输入流(1号流)
                        InputStream in = socket.getInputStream();
                        //        4.创建文件的字节输出流
                        String filename = System.currentTimeMillis() + ".png";//
                        FileOutputStream fos = new FileOutputStream(filename);
                        //        5.循环:一边读数据,一边写入文件中
                        System.out.println("正在接收文件...");
                        byte[] bs = new byte[1024];
                        int len = 0;
                        while ((len = in.read(bs)) != -1) {
                            fos.write(bs, 0, len);
                        }
                        System.out.println("接收文件成功...");
                        //==================给客户端反馈信息=================
                        OutputStream out = socket.getOutputStream();
                        out.write("您的文件收到了!".getBytes());
                        //==================给客户端反馈信息=================
                        //6.释放资源
                        out.close();
                        fos.close();
                        in.close();
                        socket.close();
                    } catch (IOException ie) {
                        System.out.println("出错啦" + socket.getInetAddress().getHostAddress());
                    }
                }
            }).start();
        }
//        server.close();
//        System.out.println("服务器成功关闭...");
    }
}
客户端优化后的代码:
/**
 * TCP文件上传客户端
 */
public class TCPFileUploadClient {
    public static void main(String[] args) throws IOException {
//        1.创建Socket对象
        Socket socket = new Socket("127.0.0.1", 8888);
        System.out.println("连接服务器成功...");
//        2.获取通道中的输出流(1号流)
        OutputStream out = socket.getOutputStream();
//        3.创建文件的字节输入流
        FileInputStream fis = new FileInputStream("G:\\upload\\444.png");
//        4.循环:一边读文件,一边发数据
        System.out.println("正在上传图片...");
        byte[] bs = new byte[1024];
        int len = 0;
        while ((len = fis.read(bs)) != -1) {
            out.write(bs, 0, len);
        }
        System.out.println("图片上传完毕...");
        socket.shutdownOutput();
        //=============读取服务器的反馈信息===============
        InputStream in = socket.getInputStream();
        len = in.read(bs);
        System.out.println("服务器说:" + new String(bs, 0, len));
        //=============读取服务器的反馈信息===============

//        5.释放资源
        fis.close();
        out.close();
        socket.close();
        System.out.println("客户端成功关闭...");
    }
}
```

##### 5.模拟BS架构服务器

```java
public class Server {
    public static void main(String[] args) throws IOException {
        //1.创建服务器
        ServerSocket server = new ServerSocket(8888);
        //2.接收客户端
        Socket socket = server.accept();
        //3.获取通道中的输入流
        InputStream in = socket.getInputStream();
        //4.读取数据
//        byte[] bs = new byte[1024];
//        int len = in.read(bs);
//        System.out.println(new String(bs, 0, len));
        //读取一行
        BufferedReader br = new BufferedReader(new InputStreamReader(in));
        String line = br.readLine(); //GET /2.html HTTP/1.1
        //切割
        String[] split = line.split(" ");//GET /2.html HTTP/1.1
        String s = split[1];//  /2.html
        String filename = s.substring(1);
        System.out.println("浏览器要:" + filename);
        //从硬盘上读取文件,发给浏览器
        FileInputStream fis = new FileInputStream(filename);
        OutputStream out = socket.getOutputStream();
        byte[] bs = new byte[1024];
        int len = 0;
        out.write("HTTP/1.1 200 OK\r\n".getBytes());
        out.write("Content-Type:text/html\r\n".getBytes());
        out.write("\r\n".getBytes());
        while ((len = fis.read(bs)) != -1) {
            out.write(bs, 0, len);
        }
        System.out.println("文件已经发送给浏览器");

        //5.释放资源
        in.close();
        socket.close();
        server.close();
    }
}
```

##### 6.扩展:模拟服务器扩展_图片显示问题
