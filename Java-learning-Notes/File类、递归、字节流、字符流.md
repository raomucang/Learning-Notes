## day10【File类、递归、IO流、字节流、字符流】

### 一.File类

##### 1.File类的作用

```java
File类是Java代表文件和文件夹的类
```

##### 2.File类的构造

```java
"public File(String pathname);以指定的路径创建File对象
public File(String parent,String child); 以指定的父子路径创建File对象
public File(File parent,String child); 以指定的父File对象和子路径创建File对象

public class FileConstructorDemo {
    public static void main(String[] args) {
        //1.创建File方式一
        File f1 = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa\\1.txt");
        System.out.println(f1);

        //2.创建File方式二
        File f2 = new File("C:\\Users\\Administrator\\Desktop\\temp","aaa\\1.txt");
        System.out.println(f2);

        //3.创建File方式三
        File parent = new File("C:\\Users\\Administrator\\Desktop\\temp");
        File f3 = new File(parent,"aaa\\1.txt");
        System.out.println(f3);
    }
}
```

##### ==3.相对路径和绝对路径的概念==

```java
绝对路径: 以盘符开头的路径 比如:"C:\\Users\\Administrator\\Desktop\\temp\\aaa\\1.txt"
相对路径: 如果我们要描述的文件在当前项目下,那么我们可以直接省略项目的路径
		比如: 
		我们的项目是: "E:\黑马就业班122期\heima122"
         现在的根目录下有一个文件: "1.txt"
         那么我们怎么表示这个1.txt呢???
         第一种: File f = new File("E:\黑马就业班122期\heima122\1.txt");  
		第二种: File f = new File("1.txt");

public class TestFileDemo {
    public static void main(String[] args) {
        //1.创建项目跟目录下aaa文件夹下的1.txt
        File f1 = new File("E:\\黑马就业班122期\\heima122\\aaa\\1.txt");
        //或者使用相对路径
        File f2 = new File("aaa\\1.txt");
    }
}
```

##### 4.File类的获取方法

```java
public String getAbsolutePath(); 获取File对象的绝对路径
public String getPath(); 获取创建File对象时传入的路径
public String getName(); 仅仅获取File对象的名字(不带路径)
public long length():获取文件的大小(单位字节),无法获取文件夹的大小
  
public class FileGetMethod {
    public static void main(String[] args) {
        //1.创建File对象
        File f1 = new File("aaa\\1.txt");
        //2.获取方法
        //a.获取绝对路径
        String absolutePath = f1.getAbsolutePath();
        System.out.println(absolutePath);
        //b.获取创建对象是传入的路径
        String path = f1.getPath();
        System.out.println(path);
        //c.获取名字(不带路径)
        String name = f1.getName();
        System.out.println(name);
        //d.length
        long len = f1.length();
        System.out.println(len);
    }
}  
```

##### 5.File类的判断方法

```java
public boolean exists();判断该File对象所代表的文件或者文件夹是否存在!!!
public boolean isFile(); 判断是否为文件
public boolean isDirectory(); 判断是否为文件夹
注意:如果文件不存在,那么判断方法返回的都会是false,没有意义

public class FileIsMethod {
    public static void main(String[] args) {
        //1.创建File对象
        File f1 = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa\\5.txt");
        //2.判断方法
        boolean isFile = f1.isFile();
        boolean isDirectory = f1.isDirectory();
        System.out.println("是文件吗?"+isFile);
        System.out.println("是文件夹吗?"+isDirectory);
        //3.是否存在
        boolean exists = f1.exists();
        System.out.println("该文件或者文件夹存在吗?"+exists);
    }
}
```

##### 6.File类的创建删除方法

```java
public boolean createNewFile();//创建一个文件(只能创建文件)
public boolean mkdir(); //创建单级文件夹(只能创建文件夹)
public boolean mkdirs(); //创建多级文件夹(只能创建文件夹)
public boolean delete();//删除文件或者空文件夹(非空文件夹不能删除成功!!)

public class FileCreateMethod {
    public static void main(String[] args) throws IOException {
        //1.创建File对象
        File f1 = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa\\bbb");
        //2.创建
        boolean b2 = f1.mkdir();
        System.out.println("是否创建成功:"+b2);
        boolean b = f1.createNewFile();
        System.out.println("是否创建成功:" + b);
        // 3.删除
        boolean b3 = f1.delete();
        System.out.println("是否删除成功:"+b3);
        //4.一条龙创建
        File f2 = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa\\111\\222\\333\\444");
        boolean b4 = f2.mkdirs();
        System.out.println("是否创建成功:"+b4);
    }
}
```

##### 7.File类遍历目录的方法

```java
public String[] list();//返回该目录下所有文件和文件夹的名字
"public File[] listFiles();//返回该目录下所有文件和文件夹的File对象
注意:以上两个方法,只能遍历目录,不能遍历文件(如果遍历返回值是null)
  
public class FileForMethod {
    public static void main(String[] args) {
        //1.创建File对象
        File f = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa");
        //2.遍历名字
        String[] filenames = f.list();
        for (String filename : filenames) {
            System.out.println(filename);
        }
        //3.遍历File对象
        File[] files = f.listFiles();
        for (File file : files) {
            System.out.println(file.getAbsolutePath());
        }
    }
}  
```

### 二.递归

##### 1.什么是递归?

```java
递归: 并不是Java语言特有的技术,而是所有语言基本上都具有的
什么是递归:
	在方法中调用该方法本身,这种现象称为递归
public class TestRecursionDemo {
    public static void main(String[] args) {
        method();
    }
    public static void method(){
        System.out.println("method...");
        method(); //此时就是递归
    }
}
注意: 无限递归或者死递归,最终会导致栈内存溢出!!!(方法是在栈中执行的)
  
  
合理使用递归:
	a.递归必须不能是无限的(递归必须有出口)
	b.递归的次数也不能太多(不能超出系统最大递归次数)
 
public class TestRecursionDemo {
    public static void main(String[] args) {
        method(10);
    }
    public static void method(int n){
        if (n == 0) { //这就是递归的出口
            return;
        }
        System.out.println("method..."+n);
        method(n-1); //此时就是递归
    }
}      
      
```

##### 2.递归求和案例

```java
需求:求1+2+3...n的和(使用递归)
  
使用递归的步骤:
	a.定义方法,明确参数和返回值
	b.找"规律",调用自己
	c.让递归有一个出口
	
public class RecursionTestDemo {
    public static void main(String[] args) {
        //需求:求1+2+3...n的和(使用递归)
        int sum = getSum(10);
        System.out.println(sum);
    }

    /**
     * 使用递归
     * 使用递归的步骤:
     * a.定义方法,明确参数和返回值
     * b.找"规律",调用自己
     * c.让递归有一个出口
     */
    //a.定义方法,明确参数和返回值
    public static int getSum(int n) {
        //c.让递归有一个出口
        if (n == 1) {
            return 1;
        }
        //b.找"规律",调用自己
        //1+2+3..n = (1+2+3..n-1)+n
        //getSum(n) = getSum(n-1)+n
        return getSum(n - 1) + n;
    }

    /**
     * 定义方法:求1-n的和
     */
    public static int getSum(int n) {
        int sum = 0;
        for (int i = 1; i < n + 1; i++) {
            sum += i;
        }
        return sum;
    }
}	
```

![](递归求和图解.png)

##### 3.递归求积案例

```java
 需求:求1*2*3..n的积(n的阶乘,使用递归)
 
使用递归的步骤:
a.定义方法,明确参数和返回值
b.找规律,调用自己
c.让递归有个出口

public class RecursionTestDemo02 {
    public static void main(String[] args) {
       //需求:求1*2*3..n的积(n的阶乘,使用递归)
        int ji = getJi(5);
        System.out.println(ji);
    }

    /**
     * 使用递归
     */
    //a.定义方法
    public static int getJi(int n) {
        //c.有出口
        if (n == 1) {
            return 1;
        }
       //b.找规律
        //1*2*3...n = (1*2*3..n-1)*n
        //getJI(n) = getJi(n-1)*n
        return getJi(n-1)*n;
    }
}
```

##### ==4.文件搜索案例==

```java
需求:
	要在目下:C:\Users\Administrator\Desktop\temp\aaa
	查找文件:所有的.txt文件

public class FileSearchTestDemo {
    public static void main(String[] args) {
        //1.创建一个File对象
        File fileDir = new File("C:\\Users\\Administrator\\Desktop\\temp\\aaa");
        //2.调用方法
        findTxtFiles(fileDir);
    }

    /**
     * 在指定的目录下查找.txt文件
     */
    public static void findTxtFiles(File file) {
        //1.遍历目录
        File[] files = file.listFiles();
        //2.遍历数组
        for (File f : files) {
            //3.判断f是否为.txt文件
            if (f.isFile() && f.getName().endsWith(".txt")) {
                //4.如果是文件,并且是.txt文件,直接打印
                System.out.println("找到了一个txt文件:" + f.getAbsolutePath());
            } else if (f.isDirectory()) {
                //5.如果是一个文件夹
                findTxtFiles(f);
            }
        }
    }
}
```

### ==三.IO流的概述==

##### 1.什么是IO流

```java
IO流是什么???
I: Input 输入流,数据从外部设备到内存,也称为读流
O: Output 输出流,数据从内存到外部设备,也称为写流
流: 这是一种比喻
```

![](InputOutput.png)

##### 2.IO流的分类

```java
a.根据IO流的方向:
	输出流
	输入流
b.根据IO流中操作的数据类型:
	字节流
	字符流
```

##### ==3.Java中IO的四大流==

```java
字节输出流:顶层父类:OutputStream

字节输入流:顶层父类:InputStream

字符输出流:顶层父类:Writer

字符输入流:顶层父类:Reader

Java中IO流的所有子类命名是非常规范:
	命名: 功能+父类名
	比如: FileInputStream
		 FileWriter
```

### ==四.字节流==

##### 1.万物皆对象和IO流一切皆字节

```java
在Java中有一个概念:万物皆对象
	生活中任何一个事物,都可以用Java中的一个对象来描述
在IO流中有一个概念:一切皆字节
	计算机任何文件,最终都是0101组成的!!!
  	所有文件,比如.txt,.png,.avi等底层都是由字节组成的!!
  	问题1:为什么我们打开文件看到不是0101????
      	  因为我们使用软件打开的文件,比如.txt使用记事本或者Notepad++软件
      	  						比如.png使用图片工具,画图工具软件打开的
      	  						比如.avi使用视频播放器软件打开的
      	  软件会根据自身的代码对0101进行解码,从而显示不同的效果						
  	问题2:我们能否不解析,就像看文件的0101编码?
      	  可以,只要使用二进制软件打开即可(Binary Viewer)		
```

##### 2.字节输出流

```java
字节输出流:OutputStream(抽象类)

public void close(); 关闭此字节输出流(释放资源)
public void flush(); 刷新缓冲区(对于字节流来说是空方法)  
  
public void write(int b); 一次写一个字节
public void write(byte[] bs); 一次写一个字节数组
public void write(byte[] bs,int startIndex,int len); 一次写一个字节数组的一部分
```

##### 3.FileOutputStream类的使用

- FileOutputStream称为文件的字节输出流


- a.构造方法

  ```java
  public FileOutputStream(String pathname); 以文件的路径创建对象
  public FileOutputStream(File file); 以File对象来创建对象

  public class FileOutputStreamDemo01 {
      public static void main(String[] args) throws Exception {
          //1.创建一个FileOutputStream对象
          FileOutputStream fos = new FileOutputStream("1.txt");
  //        FileOutputStream fos = new FileOutputStream(new File("1.txt"));
          /**
           * 以上构造干了三件事!!!!
           * a.创建对象fos
           * b.判断文件是否存在
           *      如果存在,则会清空文件的内容
           *      如果不存在,则会自动创建空文件
           * c.让对象fos和文件绑定
           */
      }
  }
  ```

- b.写字节数据的三个方法

  ```java
  public void write(int b); 一次写一个字节
  public void write(byte[] bs); 一次写一个字节数组
  public void write(byte[] bs,int startIndex,int len); 一次写一个字节数组的一部分

  public class FileOutputStreamDemo02 {
      public static void main(String[] args) throws Exception {
          //1.创建一个FileOutputStream对象
          FileOutputStream fos = new FileOutputStream("1.txt");
          //2.先文件中写数据
          //a.一次写一个字节
          fos.write(97);//0110 0001
          //思考:现在我要求打开文件中看到97,怎么办??
  //        fos.write(57);
  //        fos.write(55);
          //b.一次写一个字节数组
  //        byte[] bs = {106,97,118,97};
  //        fos.write(bs);
          //思考:现在我要求打开文件看到HelloWorld,Java!
  //        byte[] bytes = "java".getBytes();
  //        System.out.println(Arrays.toString(bytes));
          fos.write("HelloWorld,Java!".getBytes());
          //c.一次写一个字节数组的一部分
          fos.write("HelloWorld,Java!".getBytes(),11,4);
      }
  }
  ```

- c.如何追加/续写

  ```java
  每次使用构造方法创建FileOutputStream对象时,文件存在都会清空内容,那么如何不清空,实现追加呢????
  非常简单,只要使用以下两个构造,第二个参数传入true即可
  public FileOutputStream(String pathname,boolean append); 以文件的路径创建对象
  public FileOutputStream(File file,boolean append); 以File对象来创建对象

  public class FileOutputStreamDemo03 {
      public static void main(String[] args) throws Exception {
          //1.创建一个FileOutputStream对象
          FileOutputStream fos = new FileOutputStream("1.txt",true);
          //2.写点数据
          fos.write("Java".getBytes());
      }
  }
  ```

- d.如何换行

  ```java
  非常简单:
  	只需要向文件中写一个代表换行的符号即可
  	windows: \r\n
  	Linux: \n
  	Mac: \r(从Mac OS X 以后也是\n) 
        
  public class FileOutputStreamDemo04 {
      public static void main(String[] args) throws Exception {
          //1.创建一个FileOutputStream对象
          FileOutputStream fos = new FileOutputStream("2.txt");
          //2.写点数据
          for (int i = 0; i < 10; i++) {
              fos.write("PHP".getBytes());
              //向文件中写一个\r\n即可
              fos.write("\r".getBytes());
          }
      }
  }
        
  ```

- 关闭流和刷新缓冲区

  ```java
  public void flush(); 刷新缓冲区(对于字节流来说是空方法)  
  public void close(); 关闭此字节输出流(释放资源)  
    	注意:流使用完毕,必须第一时间释放资源,保证该资源其他程序可以正常使用 
  ```

##### 4.字节输入流InputStream

```java
字节输入流:InputStream(抽象类)

public void close();//释放资源
public int read(); //一次读一个字节
public int read(byte[] bs); //一次读取一个字节数组, 返回值实际读取了多少个字节
```

##### 5.FileInputStream类的使用

- FileInputStream称为文件的字节输入流


- a.构造方法

  ```java
  public FileInputStream(String pathname); //以文件的路径创建对象
  public FileInputStream(File file);//以File对象创建对象

  public class FileInputStreamDemo01 {
      public static void main(String[] args) throws Exception {
          //1.创建FileInputStream对象
          FileInputStream fis = new FileInputStream("1.txt");
  //        FileInputStream fis = new FileInputStream(new File("1.txt"));
          /**
           * 以上构造干了三件事!!!
           * a.创建对象fis
           * b.判断文件是否存在
           *      如果存在,就存在(不清空)
           *      如果不存在,抛出异常FileNotFoundException
           * c.绑定fis和1.txt
           */
      }
  }
  ```

- ==b.读取一个字节==

  ```java
  public class FileInputStreamDemo02 {
      public static void main(String[] args) throws Exception {
          //1.创建FileInputStream对象
          FileInputStream fis = new FileInputStream("1.txt");
          //2.一次读取一个字节
  //        int b = fis.read();
  //        System.out.println((char) b);
          //=============一次读取一个字节的标准循环===========
          int b = 0;//用来接收读取到的字节
          /**
           * (b = fis.read()) != -1
           * 以上代码干了三件事!!!
           * a.先读  fis.read()
           * b.赋值  把读取到的字节赋值给b
           * c.判断  b != -1
           */
          while ((b = fis.read()) != -1) {
              System.out.print((char) b);
          }
          //3.释放资源
          fis.close();
      }
  }
  ```

- ==c.读取一个字节数组==

  ```java
  public class FileInputStreamDemo03 {
      public static void main(String[] args) throws Exception {
          //1.创建FileInputStream对象
          FileInputStream fis = new FileInputStream("1.txt");
          //2.一次读取一个字节数组
  //        byte[] bs = new byte[4];
  //        int len = fis.read(bs);
  //        System.out.println("实际读取:" + len + "个字节");
  //        System.out.println("读取的内容:"+ new String(bs,0,len));
          //==========一次读取一个字节数组的标准循环代码===========
          int len = 0;//接收实际读取的字节个数
          byte[] bs = new byte[4];//每次读取的字节数组
          /**
           * (len = fis.read(bs)) != -1
           * 以上代码干了 三件事!!!
           * a.先读 fis.read(bs)
           * b.赋值 把实际读取的字节个数赋值给len
           * c.判断 len != -1
           * 
           */
          while ((len = fis.read(bs)) != -1) {
              System.out.print(new String(bs, 0, len));
          }
          //3.释放资源
          fis.close();
      }
  }
  ```

##### ==6.字节流练习:复制图片==

- a.复制文件的过程(画图)

- b.代码实现(代码演示)

  ```java
  public class TestCopyFileDemo {
      public static void main(String[] args) throws Exception {
          //调用
          long start = System.currentTimeMillis();
          copy02();
          long end = System.currentTimeMillis();
          System.out.println("耗时:" + (end - start) + "毫秒");//耗时:2876毫秒 耗时:10毫秒
      }
      /**
       * 一个一个字节数组方式
       */
      public static void copy02() throws Exception {
          //1.创建2个流
          FileInputStream fis = new FileInputStream("G:\\upload\\222.png");
          FileOutputStream fos = new FileOutputStream("copy.png");
          //2.复制文件
          int len = 0;
          byte[] bs = new byte[1024 * 5];
          while ((len = fis.read(bs)) != -1) {
              fos.write(bs, 0, len);
          }
          //3.释放资源
          fos.close();
          fis.close();
      }

      /**
       * 一个一个字节方式
       */
      public static void copy01() throws Exception {
          //1.创建2个流
          FileInputStream fis = new FileInputStream("G:\\upload\\222.png");
          FileOutputStream fos = new FileOutputStream("copy.png");
          //2.复制文件
          int b = 0;
          while ((b = fis.read()) != -1) {
              fos.write(b);
          }
          //3.释放资源
          fos.close();
          fis.close();
      }
  }
  ```

### ==五.字符流==

##### 1.为什么要用字符流

```java
因为如果有中文,一个中文不止一个字节,一个中文是多个字节,如果使用字节流可能出现只读取中文的一半,打印出来是乱码!!!
```

##### 2.字符输入流

```java
字符输入流:Reader(抽象类)

public void close();释放资源
public int read(); 一次读一个字符
public int read(char[] chs); 一次读一个字符,返回值表示实际读取的字符个数  
```

##### 3.FileReader类的使用

- FileReader称为文件的字符输入流


- a.构造方法

  ```java
  public FileReader(String pathname);
  public FileReader(File file);

  public class FileReaderDemo01 {
      public static void main(String[] args) throws Exception {
          //1.创建FileReader對象
          FileReader fr = new FileReader("1.txt");
  //        FileReader fr = new FileReader(new File("1.txt"));
          /**
           * 以上構造干了三件事會
           * a.创建对象fr
           * b.判断文件是否存在
           *      如果存在,就是存在(不清空)
           *      如果不存在,直接抛出异常
           * c.绑定fr和1.txt文件
           */
      }
  }
  ```

- ==b.读取一个字符==

  ```java
  public class FileReaderDemo02 {
      public static void main(String[] args) throws Exception {
          //1.创建FileReader對象
          FileReader fr = new FileReader("1.txt");
          //2.读取数据,一个一个字符
  //        int ch = fr.read();
  //        System.out.println((char) ch);
          //==========一次读取一个字符的标准循环代码==========
          int ch = 0; //用来接收读取的字符
          /**
           * (ch = fr.read()) != -1
           * 以上代码干了三件事
           * a.先读 fr.read
           * b.赋值 把读取的字符赋值给ch
           * c.判断 ch != -1
           */
          while ((ch = fr.read()) != -1) {
              System.out.print((char) ch);
          }
          //3.释放资源
          fr.close();
      }
  }
  ```

- ==c.读取一个字符数组==

  ```java
  public class FileReaderDemo03 {
      public static void main(String[] args) throws Exception {
          //1.创建FileReader對象
          FileReader fr = new FileReader("1.txt");
          //2.读取数据,一个一个字符数组
  //        char[] chs = new char[3];
  //        int len = fr.read(chs);
  //        System.out.println("实际读取:" + len + "个字符");
  //        System.out.println("读取内容:" + new String(chs,0,len));
          //===========一次读取一个字符数组的标准循环=============
          int len = 0; //用来接收实际读取字符个数
          char[] chs = new char[3];//保存读取的字符数组内容
          /**
           * (len = fr.read(chs)) != -1
           * 以上代码干了三件事!!
           * a.先读 fr.read(chs);
           * b.赋值 把实际读取的字符个数赋值给len
           * c.判断 len != -1
           */
          while ((len = fr.read(chs)) != -1) {
              System.out.print(new String(chs, 0, len));
          }
          //3.释放资源
          fr.close();
      }
  }
  ```

##### 4.字符输出流

```java
字符输出流:Writer(抽象类)

public void close(); 释放资源
public void flush(); 刷新缓冲区(对于字符流有用!!!)

public void write(int ch); 一次写一个字符  
public void write(char[] chs); 一次写一个字符数组
public void write(char[] chs,int startIndex,int len);一次写一个字符数组的一部分

public void write(String str); 一次写一个字符串
public void write(String str,int startIndex,int len); 一次写一个字符串的一部分
```

##### 5.FileWriter类的使用

- FileWriter称为文件的字符输出流


- a.构造方法

  ```java
  public FileWriter(String pathname);
  public FileWriter(File file);

  public class FileWriterDemo01 {
      public static void main(String[] args) throws IOException {
          FileWriter fw = new FileWriter("1.txt");
  //        FileWriter fw = new FileWriter(new File("1.txt"));
          /**
           * 以上构造三个三件事!!
           * a.创建对象fw
           * b.判断文件是否存在
           *      如果存在,清空文件
           *      如果不存在,创建一个空文件
           * c.绑定fw对象和1.txt文件
           */
      }
  }
  ```

- b.写出字符数据的三组方法

  ```java
  public void write(int ch); 一次写一个字符  
  public void write(char[] chs); 一次写一个字符数组
  public void write(char[] chs,int startIndex,int len);一次写一个字符数组的一部分

  public void write(String str); 一次写一个字符串
  public void write(String str,int startIndex,int len); 一次写一个字符串的一部分

  public class FileWriterDemo02 {
      public static void main(String[] args) throws IOException {
          //1.创建FileWriter对象
          FileWriter fw = new FileWriter("1.txt");
          //2.写字符数据
          //写一个字符
          fw.write('中');
          //写一个字符数组
          char[] chs = {'中', '国', '万', '岁'};
          fw.write(chs);
          //写一个字符数组的一部分
          fw.write(chs, 2, 2);
          //写一个字符串
          fw.write("我爱Java我爱中国我爱黑马");
          //写一个字符串的一部分
          fw.write("我爱Java我爱中国我爱黑马", 3, 2);
          //3.释放资源
          fw.close();
      }
  }

  ```

- c.关闭和刷新的区别

  ```java
  close方法:
  	先刷新缓冲区,再关闭流,流不能继续使用,否则会抛出异常,提示流已经关闭了
  flush方法:
  	刷新缓冲区(把缓冲中已有的内容真正写到文件中),但是流并没有关闭,可以继续使用流
  	
  public class FileWriterDemo05 {
      public static void main(String[] args) throws IOException {
          //1.创建FileWriter对象
          FileWriter fw = new FileWriter("1.txt");
          //2.写数据
          fw.write("中国");
          //3.刷新缓冲区
          fw.flush();
          //4.继续写数据
          fw.write("小日本");
          //5.再次刷新
          fw.flush();
          //6.释放资源
          fw.close();
          //7.关闭之后不能继续使用流
          fw.write("米国");//抛出异常
      }
  }	
  ```

  ​

- d.续写和换行

  ```java
  如何续写:只要使用以下两个构造,第二个参数传入true即可
  public FileWriter(String pathname,boolean append);
  public FileWriter(File file,boolean append);

  public class FileWriterDemo03 {
      public static void main(String[] args) throws IOException {
          //1.创建FileWriter对象
          FileWriter fw = new FileWriter("1.txt",true);
          //2.写数据
          fw.write("mv");
          //3.释放资源
          fw.close();
      }
  }

  如何换行:只需要向文件中写一个代表换行的符号即可
  windows: \r\n
  Linux: \n
  Mac: \r(从MacOSX以后也是\n)
    
  public class FileWriterDemo04 {
      public static void main(String[] args) throws IOException {
          //1.创建FileWriter对象
          FileWriter fw = new FileWriter("1.txt");
          //2.写数据
          for (int i = 0; i < 10; i++) {
              fw.write("java");
              fw.write("\r\n");
          }
          //3.释放资源
          fw.close();
      }
  }   
  ```

  ​

### 总结:

```java
能够说出File对象的创建方式(3种)
能够使用File类常用方法(获取方法,判断方法,创建方法,删除方法)
"能够辨别相对路径和绝对路径
  	"绝对路径: 盘符开头的路径或者协议开头Http://www.baidu.com/aaa/11.png
	"相对路径: 相对于当前项目的根目录
能够遍历文件夹(list,listFiles)
能够解释递归的含义
能够使用递归的方式计算5的阶乘
能够说出使用递归会内存溢出隐患的原因


能够说出IO流的分类和功能
	输入流
	输出流
	字符流
	字节流
"能够使用字节输出流写出数据到文件
	OutputStream ---> FileOutputStream
	构造方法: 4个(2个不续写,2个续写)
     成员方法: 五个
     	public void close();
		public void flush();
		public void write(int b);
		public void write(byte[] bs);
		public void write(byte[] bs,int off,int len);
"能够使用字节输入流读取数据到程序
  	InputStream ---> FileInputStream
  	构造方法:2个
  	成员方法:3个
  		public void close();
		public int read();
		public int read(byte[] bs);
		
能够理解读取数据read(byte[])方法的原理
"能够使用字节流完成任意文件的复制

"能够使用FileWriter写数据的5个方法
	构造方法:4个
	成员方法:7个
		public void close();
		public void flush();
		public void write(int ch);
		public void write(char[] bs);
		public void write(char[] bs,int off,int len);
		public void write(String str);
		public void write(String str,int off,int len);
"能够说出FileWriter中关闭和刷新方法的区别
	关闭:先刷新后关闭
	刷新:仅刷新不关闭
"能够使用FileWriter写数据实现换行和追加写
	追加: 使用带有boolean类型的那2个构造即可
	换行: 写入和平台对应的换行符即可(windows \r\n)
"能够使用FileReader读数据一次一个字符
"能够使用FileReader读数据一次一个字符数组      
      构造方法:2个
      成员方法:3个
      	public void close();
		public int read();
		public int read(char[] chs);

```

