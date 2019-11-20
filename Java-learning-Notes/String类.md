# day04 【String类、综合练习】

### 第一章 String类常用方法

##### 1.1 concat

```java
concat: 用于拼接当前字符串和参数字符串

public static void main(String[] args){
    String s1 = "Hello";
    String s2 = "World";
    String result = s1.concat(s2);
    System.out.println("result = " + result);
    System.out.println("s1 = " + s1);
    System.out.println("s2 = " + s2);
}

concat和+的区别:
a.concat只能拼接字符串,而+可以拼接字符串和任何内容
b.concat底层效率略高于+的,但是也是很低的(开发中大量的字符串拼接使用StringBuilder)
```

##### 1.2 contains

```java
contains: 判断当前字符串是否包含参数字符串(区分大小写)
  
public static void main(String[] args){
    String s = "我爱Java，我爱学习！";
    System.out.println("字符串中是否包含Java：" + s.contains("Java"));//true
    System.out.println("字符串中是否包含java：" + s.contains("java"))//false
}
```

##### 1.3 endsWith

```java
endsWith: 判断当前字符串是否以参数字符串结尾(区分大小写)
  
public static void main(String[] args){
  	String name = "Test.java";
	System.out.println("判断name是否以java结尾：" + name.endsWith("java"));//true
	System.out.println("判断name是否以Java结尾：" + name.endsWith("Java"));//false
}  
```

##### 1.4 startsWith

```java
endsWith: 判断当前字符串是否以参数字符串开头(区分大小写)
  
public static void main(String[] args){
  	String name = "张三丰";
  	System.out.println("判断name是否以张开头：" + name.startsWith("张"));//true
  	System.out.println("判断name是否以张开头：" + name.startsWith("张三"));//true
}
```

##### 1.5 indexOf

```java
indexOf: 在当前字符串中查找参数字符串(区分大小写),第一次出现的索引,如果没有找到返回-1
  
public static void main(String[] args){
  	String str = "我爱Java，我爱Java学习！";
    System.out.println(str.indexOf("Java"));//2
    System.out.println(str.indexOf("java"));//-1
}  
```

##### 1.6 lastIndexOf

```java
lastIndexOf: 在当前字符串中查找参数字符串(区分大小写),最后一次出现的索引,如果没有找到返回-1

public static void main(String[] args){
  	String str = "我爱Java，我爱Java学习！";
    System.out.println(str.lastIndexOf("Java"));//9
    System.out.println(str.lastIndexOf("java"));//-1
}    
```

##### 1.7 replace

```java
replace: 将当前字符串中的目标字符串替换为其他字符串

public static void main(String[] args){
  	String text = "你大爷的,赶紧给老子去死,你大爷的!!!";
  	String newText = text.replace("你大爷","???");
  	System.out.println(newText);//???的,赶紧给老子去死,???的!!!
}  
```

##### 1.8 substring

```java
substring: 表示截取字符串
	public String substring(int beginIndex); //从指定索引开始截取字符串,直到末尾
	public String substring(int beginIndex,int endIndex); //从指定索引开始到指定索引结束(不包含)截取字符串
	
public static void main(String[] args){
  	String str = "我爱Java";
  	System.out.println(str.substring(2)); //Java
  	System.out.println(str.substring(2,4)); //Ja
}  
```

##### 1.9 toCharArray

```java
toCharArray: 把当前字符串转成字符数组

public static void main(String[] args){
  	String str = "身无彩凤双飞翼";
	char[] chArray = str.toCharArray();
	System.out.println(chArray); //打印出的就是数组中内容:身无彩凤双飞翼
	//注意:(只有字符数组才可以,别的数组打印数组名都是打印出地址值)
}  
```

##### 1.10 toLowerCase

```java
toLowerCase: 把当前字符串转成全小写字符串

public static void main(String[] args){
  	String str = "我爱Java";
    System.out.println("转换为小写：" + str.toLowerCase());//我爱java
    System.out.println("原字符串：" + str);//我爱Java
}  
```

##### 1.11 toUpperCase

```java
toUpperCase: 把当前字符串转成全大写字符串

public static void main(String[] args){
  	String str = "我爱Java";
    System.out.println("转换为大写：" + str.toUpperCase());//我爱JAVA
    System.out.println("原字符串：" + str);//我爱Java
}  
```

##### 1.12 trim

```java
trim: 去除当前字符串"两端两端两端"的空格
public static void main(String[] args){
  	String str = " ad min ";
  	System.out.println(str.trim()); //打印结果:ad min
  	//注意:不会调换到字符串中间的空格,如果中间空格也想一起替换掉,使用replace(" ","");即可
}  

```

### 第二章 综合案例-案例演示(理解)

### 第三章 综合案例-类设计

##### 3.1 父类Person

```java
父类Person:
成员变量:
	String ID;
	String name;
	String gender;
	String birthday;
	int age;
	String hobby;
成员方法:
	重写toString,返回所有成员变量拼接成的字符串
	
构造方法:
	public Person(){}
	public Person(全参数构造){this.xxx = xxx;...}

```

##### 3.2 子类Student

```java
子类Student:
成员变量:
	完全继承父类即可(也可以添加一些特有的,比如考试分数等之类的)
      
成员方法:
	直接继承父类的toString即可(如果子类添加了特有的成员变量,那么要重新重写toString)
	
构造方法:
	public Student(){}
	public Student(全参数构造){super(全参数构造);如果有自己特有的,子类构造中赋值即可};
```

##### 3.3 子类Teacher

```java
子类Teacher:
成员变量:
	完全继承父类即可(也可以添加一些特有的,比如绩效,薪资,等级等之类的)
      
成员方法:
	直接继承父类的toString即可(如果子类添加了特有的成员变量,那么要重新重写toString)
	
构造方法:
	public Teacher(){}
	public Teacher(全参数构造){super(全参数构造);如果有自己特有的,子类构造中赋值即可};
```

##### 3.4 工具类Utils类

```java
工具类:Utils
public class Utils{
	//私有化构造方法,不允许创建该类的对象
	private Utils(){}
	//自动增长的ID
	public static long stuID;
	public static long teaID;
	//使用静态代码块赋值
	static{
		stuID = 0;
		teaID = 0;
	}
	
	//添加一个根据出生年月日计算年龄的方法
	public static int birthdayToAge(String birthday){
		//...
	}
}
```

##### 3.5 启动类

```java
启动类:其实就是我们的测试类
在主类中需要编写main方法,以及main方法中的具体业务逻辑
```

### 第四章 综合案例-类制作(见案例代码)

### 第五章 综合案例-启动类实现

##### 5.1 架构搭建

```java
/**
 * 学生管理和教师管理双系统启动类
 */
public class MainApp {
    public static void main(String[] args){
        //1.可以先把一些肯定能用到对象定义出来
        Scanner sc = new Scanner(System.in);
        ArrayList<Student> students = new ArrayList<Student>();
        ArrayList<Teacher> teachers = new ArrayList<Teacher>();
        //2.搭建程序架构
        //2.1 主界面
        System.out.println("欢迎来到传智大学管理系统");
        while (true) {
            System.out.println("1.学生管理系统    2.教师管理系统    3.退出");
            //2.2 用户选择
            String user = sc.nextLine();
            //2.3 使用switch判断
            switch (user) {
                case "1":
                    //进入学生管理系统
                    enterStudentManagerSystem();
                    break;
                case "2":
                    //进入教师管理系统
                    enterTeacherManagerSystem();
                    break;
                case "3":
                    //退出
                    System.out.println("谢谢,拜拜!!!");
                    System.exit(0);//终止JVM
                    break;
                default:
                    //提示输入有误
                    System.out.println("您输入的系统编号不存在,请确认后再重新输入!");
                    break;
            }
        }
    }

    //进入学生管理系统
    public static void enterStudentManagerSystem(){
        Scanner sc = new Scanner(System.in);
        System.out.println("欢迎来到【学生管理系统】");
        while (true) {
            //二级界面
            System.out.println("1.添加学员   2.修改学员   3.删除学员   4.查询学员   5.返回");
            //用户选择
            String user = sc.nextLine();
            //判断
            switch (user) {
                case "1":
                    //添加学生
                    addStudent();
                    break;
                case "2":
                    //修改学生
                    updateStudent();
                    break;
                case "3":
                    //删除学生
                    deleteStudent();
                    break;
                case "4":
                    //查询学生
                    findAllStudents();
                    break;
                case "5":
                    //返回
                    System.out.println("即将返回到主界面...");
                    return;
                default:
                    System.out.println("您输入的功能编号有误,请确认后重新输入");
                    break;
            }
        }
    }
    //添加学生
    public static void addStudent() {
        System.out.println("【添加学生成功】");
    }
    //修改学生
    public static void updateStudent() {
        System.out.println("【修改学生成功】");
    }
    //删除学生
    public static void deleteStudent() {
        System.out.println("【删除学生成功】");
    }
    //查询学生
    public static void findAllStudents() {
        System.out.println("【查询所有学生信息如下:】");
    }


    //进入教师管理系统
    public static void enterTeacherManagerSystem() {
        System.out.println("欢迎来到【教师管理系统】");
    }
}
```

##### 5.2 添加学生功能

```java
//添加学生
public static void addStudent(ArrayList<Student> students) {
    Scanner sc = new Scanner(System.in);
    //1.提示
    //2.输入
    System.out.println("请输入新学生的姓名:");
    String name = sc.nextLine();

    System.out.println("请输入新学生的性别:");
    String gender = sc.nextLine();

    System.out.println("请输入新学生的生日(格式:yyyy-mm-dd):");
    String birthday = sc.nextLine();

    System.out.println("请输入新学生的特长:");
    String hobby = sc.nextLine();

    //3.创建一个学生对象
    Student s = new Student(++Utils.stuID+"", name, gender, birthday, hobby);

    //4.添加到集合中
    students.add(s);

    //5.提示
    System.out.println("【添加学生成功】");
}
```

##### 5.3 查看所有学生信息

```java
public static void findAllStudents(ArrayList<Student> students) {
    //1.判断有木有学生
    if (students.size() == 0) {
      System.out.println("【系统中暂无学生信息,请添加!】");
      return;
    }
    //2.展示学生信息
    System.out.println("【查询所有学生信息如下:】");
    //3.表头
    System.out.println("编号\t\t姓名\t\t性别\t\t出生日期\t\t\t年龄\t\t特长");
    //4.遍历集合
    for (int i = 0; i < students.size(); i++) {
      Student s = students.get(i);
      System.out.println(s);
    }
    //5.添加一个分割线
    System.out.println("------------------------------------");
}
```

##### 5.4 删除学生功能

```java
//删除学生
public static void deleteStudent(ArrayList<Student> students) {
    //1.提示用户输入要删除学生的ID
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入您要删除的学生学号:");
    String ID = sc.nextLine();

    //2.查找该学号的学生
    Student s = findStudentByID(ID, students);
    //3.判断
    if (s == null) {
      System.out.println("【查无此人!】");
      return;
    }
    //4.展示该学生的信息
    System.out.println("【ID为" + ID + "的学生信息如下:】");
    System.out.println("编号\t\t姓名\t\t性别\t\t出生日期\t\t\t年龄\t\t特长");
    System.out.println(s);
    System.out.println("----------------------------------");

    //5.再次提示是否真的删除
    System.out.println("您真的真的真的确定要删除该学生吗?y/n");
    String isDelete = sc.nextLine();

    //6.判断是否真的删除
    switch (isDelete) {
      case "y":
        //安心删除
        students.remove(s);
        System.out.println("【删除学生成功】");
        break;
      case "n":
        System.out.println("【删除操作已取消】");
        break;
      default:
        System.out.println("【您输入的有误,删除操作自动取消!!!】");
        break;
    }
}
```

##### 5.5 修改学生功能

```java
//修改学生
public static void updateStudent(ArrayList<Student> students) {
    Scanner sc = new Scanner(System.in);
    //1.提示输入要修改的学生的ID
    System.out.println("请输入您要修改的学生学号:");
    String ID = sc.nextLine();

    //2.调用方法,从集合中查找该ID的学生
    Student s = findStudentByID(ID, students);

    //3.判断
    if (s == null) {
      System.out.println("【查无此人】");
      return;
    }

    //4.展示该学生的信息
    System.out.println("【ID为" + ID + "的学生信息如下:】");
    System.out.println("编号\t\t姓名\t\t性别\t\t出生日期\t\t\t年龄\t\t特长");
    System.out.println(s);
    System.out.println("----------------------------------");

    //5.提示和输入
    System.out.println("请输入该学生新的姓名(如果不修改,输入0)");
    String newName = sc.nextLine();
    System.out.println("请输入该学生新的性别(如果不修改,输入0)");
    String newGender = sc.nextLine();
    System.out.println("请输入该学生新的生日(如果不修改,输入0)");
    String newBirthday = sc.nextLine();
    System.out.println("请输入该学生新的特长(如果不修改,输入0)");
    String newHobby = sc.nextLine();

    //6.判断并修改
    if (!newName.equals("0")) {
      s.setName(newName);
    }

    if (!newGender.equals("0")) {
      s.setGender(newGender);
    }

    if (!newBirthday.equals("0")) {
      s.setBirthday(newBirthday);
    }

    if (!newHobby.equals("0")) {
      s.setHobby(newHobby);
    }
    System.out.println("【修改学生成功】");

    //7.打印一下修改之后的学生信息
    System.out.println("【您修改后的学生信息如下:】");
    System.out.println("编号\t\t姓名\t\t性别\t\t出生日期\t\t\t年龄\t\t特长");
    System.out.println(s);
}
```

### 第六章 课堂练习(教师管理系统)