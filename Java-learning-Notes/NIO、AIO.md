# day13-NIO、AIO

### 第一章 Java中各种IO的概述

##### ==1.同步与异步==

```java
同步: 当调用某个IO功能时,可能立即返回,也可能直到有结果才返回,如果是立即返回,那么后期我们需要通过其他的额外代码判断结果或者获取结果
异步: 当调用某个IO功能时,立刻返回,那么后期我们也不需要通过额外代码自己获取结果,等结果返回了,自动回通过"函数回调"通知我们已经有结果
```

##### ==2.阻塞与非阻塞==

```java
阻塞: 当调用某个IO功能时,没有立刻返回,直到有结果,才返回结果程序往下执行 
非阻塞: 当调用某个IO功能时,立刻返回结果,程序直接向下执行
```

##### 3.BIO,NIO,AIO的介绍

```java
BIO(Blocked IO): 阻塞的IO 					"阻塞,同步"
NIO(New IO): 新的IO 						"非阻塞,同步"
AIO(Asynchronized IO): 异步的IO(NIO2)		"非阻塞,异步"
```

### 第二章 NIO-Buffer类(了解)

##### 1.Buffer的介绍和种类

- 什么是Buffer

  ```java
  其实本质上就是一个数组
  ```

- Buffer的一般操作步骤

  ```java
  a.写入数据的到Buffer中
  b.调用Buffer的flip方法
  c.从Buffer中读取数据
  d.调用 clear() 方法或者 compact() 方法
  ```

- Buffer的种类

  ```java
  "ByteBuffer"
  CharBuffer
  DoubleBuffer
  FloatBuffer
  IntBuffer
  LongBuffer
  ShortBuffer
  ```

##### 2.ByteBuffer的三种创建方式

```java
public static ByteBuffer allocate(int capacity); //从Java堆空间申请,我们一般称为间接缓冲区
public static ByteBuffer allocateDirect(int capacity); //从系统空间中申请,我们一般称为直接缓冲区
public static ByteBuffer wrap(byte[] bs); //从Java堆空间申请,我们一般称为间接缓冲区

注意: 间接缓冲区的创建和销毁效率要高于直接缓冲区
	 间接缓冲区的工作效率要低于直接缓冲区	 
/**
 * 创建Buffer的三种方式
 */
public class BufferDemo01 {
    public static void main(String[] args) {
        //1.从堆内存中申请空间
        ByteBuffer buffer1 = ByteBuffer.allocate(127);
        //2.直接从系统内存中申请空间
        ByteBuffer buffer2 = ByteBuffer.allocateDirect(10);
        //3.通过自己创建byte数组
        byte[] bs = new byte[10];
        ByteBuffer buffer3 = ByteBuffer.wrap(bs);
    }
}
```

##### 3.ByteBuffer的三种添加数据方式

```java
public void put(byte b);//添加一个字节
public void put(byte[] bs);//添加一个字节数组
public void put(byte[] bs,int off,int len);//添加一个字节数组的一部分
/**
 * 创建Buffer添加数据的三种方式
 */
public class BufferDemo02 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        //2.添加一个字节
        buffer.put((byte) 10);
        buffer.put((byte) 20);
        //3.添加一个字节数组
        byte[] bs = {30,40,50};
        buffer.put(bs);
        //4.添加字节数组的一部分
        byte[] bs1 = {60,70,80,90};
        buffer.put(bs1,1,2);
        System.out.println(Arrays.toString(buffer.array()));
    }
}

```

##### 4.ByteBuffer的容量-capacity

```java
public int capacity();获取当前字节缓冲区的容量
public class BufferDemo03 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println("容量:"+buffer.capacity());
    }
}
注意:缓冲区的capacity和当前缓冲区有无数据或者有多少数据无关
```

##### 5.ByteBuffer的限制-limit

```java
public int limit();//获取缓冲区中第一个不能访问的索引的位置
public void limit(int newLimit);//修改缓冲区中第一个不能访问的索引的位置

/**
 * Buffer的limit方法
 */
public class BufferDemo05 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println(buffer.limit());//limit == capacity
        //2.添加一个字节
        buffer.put((byte) 10);
        buffer.put((byte) 20);
        System.out.println(buffer.limit());//limit == capacity
        //3.添加一个字节数组
        byte[] bs = {30,40,50};
        buffer.put(bs);
        System.out.println(buffer.limit());//limit == capacity
        //4.修改limit的位置
        buffer.limit(6); //limit = 6 position = 5
        buffer.put((byte)111);
        buffer.put((byte)112);
        System.out.println(Arrays.toString(buffer.array()));
    }
}
```

##### 6.ByteBuffer的位置-position

```java
public int position();获取当前缓冲区第一个可以添加数据的位置
public void position(int newPosition);修改可以操作的第一个位置

/**
 * Buffer的position方法
 */
public class BufferDemo04 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println("当前位置:"+buffer.position());
        //2.添加一个字节
        buffer.put((byte) 10);
        buffer.put((byte) 20);
        System.out.println("当前位置:"+buffer.position());
        //3.添加一个字节数组
        byte[] bs = {30,40,50};
        buffer.put(bs);
        System.out.println("当前位置:"+buffer.position());
        //现在指针position在索引5位置
        //改变position值,向前移动
        buffer.position(0);
        buffer.put((byte)100);
        buffer.put((byte)99);
        //改变position值,向后移动
        buffer.position(7);
        buffer.put((byte)100);
        buffer.put((byte)99);

        System.out.println(Arrays.toString(buffer.array()));
    }
}
```

##### 7.ByteBuffer的标记-mark

```java
public void mark(); 在当前position位置处为标记,标记=position
public void reset();把position移动到mark标记处
/**
 * Buffer的mark和reset方法
 */
public class BufferDemo06 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        //2.添加一个字节
        buffer.put((byte) 10);
        buffer.put((byte) 20);
        System.out.println("当前的position=" + buffer.position());
        //做个标记
        buffer.mark(); // 当mark标记为当前position的值 2

        buffer.put((byte) 30);
        buffer.put((byte) 40);
        System.out.println("当前的position=" + buffer.position());
        System.out.println(Arrays.toString(buffer.array()));
        //3.重置方法
        buffer.reset();//此时position = 标记位置2
        buffer.put((byte) 100);
        System.out.println(Arrays.toString(buffer.array()));
    }
}

注意:reset只会重置当前position的值,其他的值不会改变
```

##### 8. ByteBuffer的其他方法

```java
public int remaining()：获取position与limit之间的元素数(空闲的个数)
public boolean isReadOnly()：获取当前缓冲区是否只读  
public boolean isDirect()：获取当前缓冲区是否为直接缓冲区
public void clear()："还原缓冲区的状态(position=0,limit=capacity,清除mark标记)"
public void flip()："缩小limit的范围(limit=position,position=0,清除mark标记)"
public void rewind()：重绕此缓冲(positon=0,limit不变,清除mark标记)
  
/**
 * Buffer的其他方法(clear和flip)
 */
public class BufferDemo07 {
    public static void main(String[] args) {
        //1.创建一个ByteBuffer
        ByteBuffer buffer = ByteBuffer.allocate(10);
        System.out.println("position = "+ buffer.position()); //0
        System.out.println("limit = "+ buffer.limit());//10

        buffer.put((byte)10);
        buffer.put((byte)20);
        buffer.limit(7);

        System.out.println("position = "+ buffer.position());//2
        System.out.println("limit = "+ buffer.limit());//7

        //调用clear
//        System.out.println("-------clear buffer--------");
//        buffer.clear();
//        System.out.println("position = "+ buffer.position());//0
//        System.out.println("limit = "+ buffer.limit());//10
        //调用flip
        System.out.println("----------flip---------");
        buffer.flip();
        System.out.println("position = "+ buffer.position());//0
        System.out.println("limit = "+ buffer.limit());//2
    }
}
  
```

### 第三章 Channel（通道）(了解)

##### 1. Channel介绍和分类

```java
Channel即通道,传输数据的介质,其并不保存数据,数据由Buffer保存的
Channel相当于我们以前的IO流,但是和IO有很大的不同,其中之一就是IO流是单向的,而Channel是双向

Channel分类:
FileChannel：从文件读取数据
DatagramChannel：读写UDP网络协议数据
SocketChannel：读写TCP网络协议数据
ServerSocketChannel：可以监听TCP
```

##### 2. FileChannel类的基本使用

```java
/**
 * FileChannel的使用
 */
public class ChannelDemo01 {
    public static void main(String[] args) throws IOException {
        //1.创建2个流
        FileInputStream fis = new FileInputStream("G:\\upload\\1.jpg");
        FileOutputStream fos = new FileOutputStream("G:\\uploads\\copy.jpg");
        //2.获取通道
        FileChannel inChannel = fis.getChannel();
        FileChannel outChannel = fos.getChannel();
        //3.复制
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        int len = 0;
        //读数据
        while ((len = inChannel.read(buffer)) != -1) {
            //切换成读模式,把limit设置position,把position变为0
            buffer.flip();
            //写数据
            outChannel.write(buffer);
            //清空缓冲 ,把 position = 0 把 limit = capacity
            buffer.clear();
        }
        //4.释放资源
        outChannel.close();
        inChannel.close();
        fos.close();
        fis.close();
    }
}
```

##### 3. FileChannel结合MappedByteBuffer实现高效读写

```java
/**
 * FileChannel和MappedByteBuffer的使用
 */
public class ChannelDemo02 {
    public static void main(String[] args) throws IOException {
        //1.创建两个File对象
        RandomAccessFile inFile = new RandomAccessFile("E:\\1.avi", "r");
        RandomAccessFile outFile = new RandomAccessFile("G:\\uploads\\copy.avi", "rw");
        //2.获取通道
        FileChannel inChannel = inFile.getChannel();
        FileChannel outChannel = outFile.getChannel();
        long size = inChannel.size(); //获取目标文件的大小
        //3.映射内存的字节缓冲区
        MappedByteBuffer buffer1 = inChannel.map(FileChannel.MapMode.READ_ONLY, 0, size);
        MappedByteBuffer buffer2 = outChannel.map(FileChannel.MapMode.READ_WRITE, 0, size);
        //4.复制文件
        long start = System.currentTimeMillis();
        byte b = 0;
        while ((b = buffer1.get()) != -1) {
            buffer2.put(b);
        }
        long end = System.currentTimeMillis();
        System.out.println("耗时:"+(end-start)+"毫秒");
        //5.释放资源
        outChannel.close();
        inChannel.close();
        outFile.close();
        inFile.close();
    }
}
注意: MappedByteBuffer只能用于复制2G以下的文件
```

##### 4. SocketChannel和ServerSocketChannel的实现连接

- C/S阻塞模式

  ```java
  /**
   * TCP协议通道的同步阻塞
   */
  public class ChannelClient {
      public static void main(String[] args) throws IOException {
          //1.客户端
          SocketChannel socketChannel = SocketChannel.open();
          //2.连接服务器
          socketChannel.connect(new InetSocketAddress("127.0.0.1", 1111));
          System.out.println("连接服务器成功...");
          //3.给服务器发信息...
          ByteBuffer buffer = ByteBuffer.allocate(10);
          for (int i = 0; i < 10; i++) {
              buffer.put((byte) i);
          }
          //把buffer切换成读取模式
          buffer.flip();//buffer.clear()
          socketChannel.write(buffer);

          //4.释放资源
          socketChannel.close();
      }
  }

  /**
   * TCP协议通道的同步阻塞
   */
  public class ChannelServer {
      public static void main(String[] args) throws IOException {
          //1.创建服务器
          ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
          serverSocketChannel.bind(new InetSocketAddress(1111));
          //2.接收客户端连接
          System.out.println("等待客户端连接..");
          SocketChannel socketChannel = serverSocketChannel.accept();//同步阻塞的
          System.out.println("有客户端连接..");
          //3.接收客户端的信息...
          ByteBuffer buffer = ByteBuffer.allocate(10);
          socketChannel.read(buffer);//read也是同步阻塞的
          System.out.println("客户端的数据:"+ Arrays.toString(buffer.array()));

          //4.释放资源
          socketChannel.close();
          serverSocketChannel.close();
      }
  }
  ```

- C/S非阻塞模式

  ```java
  public class ChannelClientNonBlock {
      public static void main(String[] args) throws IOException {
          while (true) {
              //1.创建客户端通道
              try {
                  SocketChannel socketChannel = SocketChannel.open();
                  //设置通道为非阻塞
  //        socketChannel.configureBlocking(false); //把通道设置为非阻塞
                  //2.连接服务器
                  System.out.println("正在连接服务器...");
                  socketChannel.connect(new InetSocketAddress("127.0.0.1", 2222));//同步阻塞
                  System.out.println("连接成功...");
                  //3.发送信息
                  byte[] bs = "java".getBytes();
                  ByteBuffer buffer = ByteBuffer.wrap(bs);
                  socketChannel.write(buffer);
                  //4.释放资源
                  socketChannel.close();
                  break;
              } catch (IOException ie) {
                  System.out.println("连接失败...2秒后重新连接..");
                  try {
                      Thread.sleep(2000);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
              }
          }

      }
  }
  public class ChannelServerNonBlock {
      public static void main(String[] args) throws IOException, InterruptedException {
          //1.创建服务器通道
          ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
          serverSocketChannel.bind(new InetSocketAddress(2222));
          //2.接收客户端
          //设置服务器通道为非阻塞
          serverSocketChannel.configureBlocking(false);
          //3.轮询
          while (true) {
              System.out.println("等待客户端连接..");
              SocketChannel socketChannel = serverSocketChannel.accept();
              if (socketChannel != null) {
                  System.out.println("有客户端连接..");
                  //4.和客户端通信
                  ByteBuffer buffer = ByteBuffer.allocate(4);
                  socketChannel.read(buffer);
                  System.out.println("客户端消息:"+new String(buffer.array()));
                  //5.结束循环
                  break;
              } else {
                  System.out.println("没有客户端连接..");
                  Thread.sleep(2000);
              }
          }
      }
  }
  ```

### 第四章 Select(选择器)(了解)

##### 4.1 多路复用(Selector)的概念

```java
使用一个线程,处理多个服务器通道,从而提高内存,CPU的性能(利用率)
```

##### 4.2 选择器Selector_服务器端实现多路注册

```java
Selector selector = Selector.open();//创建选择器
SelectionKey key = channel.register(selector,SelectionKey.OP_ACCEPT);;//将服务器注册到选择器中

public class SelectClient {
    public static void main(String[] args) {

        //1.模拟客户端1
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("7777客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                7777));
                        System.out.println("7777客户端连接成功....");
                        break;
                    } catch (IOException e) {
                        System.out.println("7777异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();

        //2.模拟客户端2
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("8888客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                8888));
                        System.out.println("8888客户端连接成功....");
                        break;
                    } catch (IOException e) {
                        System.out.println("8888异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();

        //3.模拟客户端3
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("9999客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                9999));
                        System.out.println("9999客户端连接成功....");
                        break;
                    } catch (IOException e) {
                        System.out.println("9999异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();

    }
}
public class SelectorDemo01 {
    public static void main(String[] args) throws IOException, InterruptedException {
        //1.创建三个服务器通道
        //创建3个通道，同时监听3个端口
        ServerSocketChannel channelA = ServerSocketChannel.open();
        channelA.configureBlocking(false); //非阻塞
        channelA.bind(new InetSocketAddress(7777));

        ServerSocketChannel channelB = ServerSocketChannel.open();
        channelB.configureBlocking(false);
        channelB.bind(new InetSocketAddress(8888));

        ServerSocketChannel channelC = ServerSocketChannel.open();
        channelC.configureBlocking(false);
        channelC.bind(new InetSocketAddress(9999));
        //2.把三个服务器注册到选择器中
        Selector selector = Selector.open();
        //3.注册
        channelA.register(selector, SelectionKey.OP_ACCEPT);
        channelB.register(selector, SelectionKey.OP_ACCEPT);
        channelC.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            Thread.sleep(1000);
        }
    }
}
```

##### 4.3 Selector中的常用方法

##### 4.4.Selector实现多路连接

##### 4.4 Selector多路信息接收测试

```java
Selector的keys(); 返回所有注册到本选择器的通道
Selector的selectedKeys();返回当前已连接的通道的集合
Selector的select(): 返回一个已经连接的通道(如果没有连接的通道,此方法会阻塞)
  
  public class SelectorDemo01 {
    public static void main(String[] args) throws IOException, InterruptedException {
        //1.创建三个服务器通道
        //创建3个通道，同时监听3个端口
        ServerSocketChannel channelA = ServerSocketChannel.open();
        channelA.configureBlocking(false); //非阻塞
        channelA.bind(new InetSocketAddress(7777));

        ServerSocketChannel channelB = ServerSocketChannel.open();
        channelB.configureBlocking(false);
        channelB.bind(new InetSocketAddress(8888));

        ServerSocketChannel channelC = ServerSocketChannel.open();
        channelC.configureBlocking(false);
        channelC.bind(new InetSocketAddress(9999));
        //2.把三个服务器注册到选择器中
        Selector selector = Selector.open();
        //3.注册
        channelA.register(selector, SelectionKey.OP_ACCEPT);
        channelB.register(selector, SelectionKey.OP_ACCEPT);
        channelC.register(selector, SelectionKey.OP_ACCEPT);

        //4.打印一些关键字数据
//        Set<SelectionKey> keys = selector.keys();
//        System.out.println("已注册通道:" + keys.size());
//
//        Set<SelectionKey> selectionKeys = selector.selectedKeys();
//        System.out.println("已连接通道:" + selectionKeys.size());

        //5.获取连接的那个通道
        while (true) {
            int results = selector.select();//阻塞
            System.out.println("本次连接了" + results + "个通道...");
            Thread.sleep(1000);

            Set<SelectionKey> keys = selector.selectedKeys();

            Iterator<SelectionKey> it = keys.iterator();
            while (it.hasNext()) {
                SelectionKey nextKey = it.next();
                System.out.println("获取通道...");
                ServerSocketChannel channel = (ServerSocketChannel) nextKey.channel();
                System.out.println("等待【" + channel.getLocalAddress() + "通道数据...");
                SocketChannel socketChannel = channel.accept();
                //接收数据
                ByteBuffer inBuf = ByteBuffer.allocate(100);
                socketChannel.read(inBuf);
                inBuf.flip();
                String msg = new String(inBuf.array(), 0, inBuf.limit());
                System.out.println("【服务器】接收到通道【" + channel.getLocalAddress() + "】的信息：" + msg);
                //处理完毕该通道之后一定要从已经连接通道中删除该通道
                it.remove();
            }
        }
    }
}

public class SelectClient {
    public static void main(String[] args) {

        //1.模拟客户端1
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("7777客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                7777));
                        System.out.println("7777客户端连接成功....");

                        socket.write(ByteBuffer.wrap("java".getBytes()));
                        break;
                    } catch (IOException e) {
                        System.out.println("7777异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();


        //2.模拟客户端2
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("8888客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                8888));
                        System.out.println("8888客户端连接成功....");
                        socket.write(ByteBuffer.wrap("PHP".getBytes()));
                        break;
                    } catch (IOException e) {
                        System.out.println("8888异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();


        //3.模拟客户端3
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try (SocketChannel socket = SocketChannel.open()) {
                        System.out.println("9999客户端连接服务器......");
                        socket.connect(new InetSocketAddress("localhost",
                                9999));
                        System.out.println("9999客户端连接成功....");
                        socket.write(ByteBuffer.wrap("python".getBytes()));
                        break;
                    } catch (IOException e) {
                        System.out.println("9999异常重连,等待两秒中重连..");
                        try {
                            Thread.sleep(2000);
                        } catch (InterruptedException e1) {
                            e1.printStackTrace();
                        }
                    }
                }
            }
        }).start();

    }
}
 
```

### 第五章 AIO(异步、非阻塞)(了解)

##### 5.1 AIO概述

```java
AsynchronizedIO: 异步IO
```

##### 5.2 AIO 异步非阻塞连接

```java
public class AIOClient {
    public static void main(String[] args) throws IOException, InterruptedException {
        //1.创建客户端
        AsynchronousSocketChannel asynchronousSocketChannel = AsynchronousSocketChannel.open();
        //2.连接服务器
        asynchronousSocketChannel.connect(new InetSocketAddress("127.0.0.1", 8888), null, new CompletionHandler<Void, Void>() {

            @Override
            public void completed(Void result, Void attachment) {
                //连接成功
                System.out.println("连接成功...");
            }

            @Override
            public void failed(Throwable exc, Void attachment) {
                System.out.println("连接失败...");
                exc.printStackTrace();
            }
        });
        //3.
        System.out.println("继续");
        while (true) {
            Thread.sleep(1000);
        }
    }
}

public class AIOServer {
    public static void main(String[] args) throws IOException, InterruptedException {
        //1.创建服务器通道
        AsynchronousServerSocketChannel asynchronousServerSocketChannel = AsynchronousServerSocketChannel.open();
        //2.绑定端口
        asynchronousServerSocketChannel.bind(new InetSocketAddress(8888));
        //3.接收客户端
        asynchronousServerSocketChannel.accept(null, new CompletionHandler<AsynchronousSocketChannel, Void>() {

            @Override
            public void completed(AsynchronousSocketChannel result, Void attachment) {
                System.out.println("有客户端连接...");
            }

            @Override
            public void failed(Throwable exc, Void attachment) {
                System.out.println("客户端连接失败...");
            }
        });
        //4.
        System.out.println("继续....");
        while (true) {
            Thread.sleep(1000);
        }
    }
}
```

##### 5.3 AIO同步非阻塞

##### 5.4 AIO异步非阻塞

```java
客户端的connect方法以及服务器的accpet方法,可以是同步的,也可以异步的
通道的read方法,也可以是同步的也可以是异常

连接同步+read异步
连接异步+read同步
连接异步+read异步

异步:立刻返回(不是结果),然后需要有一个回调用函数,当连接或者读取数据成功后自动调用(通知我们)
```

总结

```java
能够说出同步和异步的概念
能够说出阻塞和非阻塞的概念
能够创建和使用ByteBuffer
能够使用MappedByteBuffer实现高效读写
能够使用ServerSocketChannel和SocketChannel实现连接并收发信息
能够说出Selector选择器的作用
能够使用Selector选择器
能够说出AIO的特点
```

