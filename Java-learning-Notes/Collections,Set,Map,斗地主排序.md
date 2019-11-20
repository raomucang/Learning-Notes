# day06【Collections、Set、Map、斗地主排序】

### 第一章 Collections类

##### 1.0 Collections的介绍

```java
Collections是集合工具类,专门操作集合
```

##### 1.1 Collections常用功能

```java
public static void shuffle(List<?> list);//打乱集合顺序(洗牌)
public static <E> void sort(List<E> list); //对集合进行升序排序 
public class TestCollectionsDemo {
    public static void main(String[] args) {
        //1.创建一个集合
        ArrayList<Integer> arr = new ArrayList<Integer>();
        //2.添加数据
        arr.add(3);
        arr.add(1);
        arr.add(5);
        arr.add(4);
        arr.add(2);
        System.out.println(arr);
        //3.排序
        Collections.sort(arr);
        //4.打印
        System.out.println(arr);
    }
}
注意: sort方法对集合进行排序时,遵循以下规则
a.如果集合是数值类型(Byte,Short,Integer,Long,Float,Double),那么按照值的大小升序排序
b.如果集合是字符类型(Character),那么按照字符的ASCII码值的大小升序排序
c.如果集合是String类型,那么先按照首字母码值升序排序,如果首字母相同按照次字母,依次类推
```

##### 1.2 Comparator比较器排序

```java
带有比较器的排序方法:
	public static <T> void sort(List<T> list,Comparator<T> com); 
比较器排序的口诀: 升序 前-后
public class TestCollectionsDemo02 {
    public static void main(String[] args) {
        //1.创建集合,保存自定义类型
        ArrayList<Dog> dds = new ArrayList<Dog>();
        //2.添加数据
        dds.add(new Dog(4, "marry"));
        dds.add(new Dog(1, "jack"));
        dds.add(new Dog(5, "hanmeimei"));
        dds.add(new Dog(3, "tom"));
        dds.add(new Dog(2, "ady"));
        //3.排序
        Collections.sort(dds, new Comparator<Dog>() {
            //该方法就是比较器中比较元素的方法
            //返回值就是比较的结果
            //返回 负数  代表  前者 小于 后者
            //返回 正数  代表  前者 大于 后者
            //返回 0    代表  相等
            public int compare(Dog o1, Dog o2) {
                //结论: 升序 前-后
                //a.按照年龄升序
//                return o1.getAge() - o2.getAge();
                //b.按照狗的姓名的长度降序
//                return o2.getName().length() - o1.getName().length();
                //c.按照狗的姓名的第二个字母的ASCII码值降序
                return o2.getName().charAt(1) - o1.getName().charAt(1);
            }
        });
        //4.打印
        for (Dog dd : dds) {
            System.out.println(dd);
        }
    }
}
```

##### 1.3 可变参数

```java
可变参数: JDK1.5之后的新特性
可变参数: 参数个数可以随意(不是类型)
格式:
	public static 返回值类型 方法名(数据类型... 变量名){
      
    }
可变参数的本质:
	 可变参数就是一个数组
	 
public class TestVariableArgumentsDemo {
    public static void main(String[] args) {
        //调用方法
        System.out.println(getSum());
        System.out.println(getSum(1));
        System.out.println(getSum(1, 2));
        System.out.println(getSum(1, 2, 3, 4, 5, 6, 7, 8, 9));
    }
    //定义方法:求n个数之和
    public static int getSum(int... a) {
        //求数组元素之和
        int sum = 0;
        for (int i : a) {
            sum += i;
        }
        return sum;
    }
}
注意事项:
	a.一个方法最最最多只能有一个可变参数
	b.如果方法中既有可变参数也有正常参数,那么可变参必须写在最后面
```

##### 1.4 可变参数的应用场景

```java
在Collections的addAll方法中使用到了可变参数
public static <E> void addAll(Collection<E> cc,E... elements);

public class TestVariableArgumentsDemo02 {
    public static void main(String[] args) {
        //可变参数的应用场景
        ArrayList<String> arr = new ArrayList<String>();
        //添加多个元素
//        arr.add("java1");
//        arr.add("java2");
//        arr.add("java3");
//        arr.add("java4");
//        arr.add("java5");
        //Collections
        Collections.addAll(arr,"java1","java2","java3","java4","java5");
        System.out.println(arr);
    }
}
```

### 第二章 Set接口

##### 1.Set接口的特点

```java
a.无索引
b.无序的,是指存入和取出时顺序不能保证一致(LinkedHashSet除外,它是有序的) 
c.元素唯一
```

##### 2.Set接口的常用方法以及常用子类

```java
Set接口的常用方法: 没有特有方法,只有Collection继承的方法(常用8个)
Set接口的常用实现类:
	HashSet,LinkedHashSet,TreeSet
```

##### 3.HashSet的数据结构以及使用

```java
HashSet的常用方法: 8个(就是从Collection中实现的)
HashSet的数据结构: 哈希表结构(保证元素的唯一性,并是无序的) 
  
public class TestSetDemo {
    public static void main(String[] args) {
        //1.创建HashSet
        HashSet<String> set1 = new HashSet<String>();
        //2.添加元素
        set1.add("java");
        set1.add("c++");
        set1.add("python");
        set1.add("php");
        set1.add("iso");
        System.out.println(set1);
    }
}  
```



##### 4.LinkedHashSet的数据结构以及使用

```java
LinkedHashSet的常用方法: 8个(就是从Collection中实现的)
LinkedHashSet的数据结构: 链式哈希表结构(保证元素的唯一性,并是有序的)
  
public class TestSetDemo {
    public static void main(String[] args) {
        //1.创建LinkedHashSet
        LinkedHashSet<String> set2 = new LinkedHashSet<String>();
        //2.添加元素
        set2.add("python");
        set2.add("java");
        set2.add("php");
        set2.add("c++");
        set2.add("iso");
        System.out.println(set2);
    }
}  
```

##### 5.TreeSet的数据结构以及使用

```java
TreeSet的常用方法: 8个(就是从Collection中实现的)
TreeSet的数据结构: 底层是搜索二叉树(排序二叉树)
TreeSet的特点:
		无索引
		无序的,但是是有自然顺序的
         唯一
public class TestTreeSetDemo {
    public static void main(String[] args) {
        //1.创建一个TreeSet集合
        TreeSet<Integer> set = new TreeSet<Integer>();
        //2.添加
        set.add(33);
        set.add(10);
        set.add(16);
        set.add(15);
        set.add(20);
        //3.打印
        System.out.println(set);
    }
}
输出结果: [10, 15, 16, 20, 33] Tree是无序中比较特殊的一种,有自然顺序的         
```

##### 6.哈希表结构的介绍[扩展]

- 对象的哈希值(对象的"数字指纹")

  ```java
  a.如何获取Java中对象的哈希值???
    		调用对象的hashCode方法即可
  b.我们平时说的地址值和哈希值其实是相等的,只是进制不同而已
  c.在Java中所有打印出来的地址值都是假的!!!而是哈希值的16进制而已
  	为什么说地址值是假的呢?? 因为Object中toString是这样的
  	public String toString() {
          return getClass().getName()+"@"+Integer.toHexString(hashCode());//把哈希值转成16进制
      }
  d.在Java中有木有真正的地址值,有!!!! 
    	类名 对象名 = new 类型(); 对象名中保存是真正的地址值
    	System.out.println(对象名) ==> System.out.println(对象名.toString()); 

  e.最后一个问题: 
  			不同对象的地址值会一样吗??  肯定不一样
    			不同对象的哈希值会一样吗?? 有可能一样,但是可能性极小
    
  ```

  结论:哈希表结构如何保证元素的唯一性?

  ```java
  哈希表结构 = 数组 + 链表 + 黑红树(JDK8新增的)
  哈希表特点: 无序的,元素唯一
  ****************************************************
  	结论:哈希表结构如何保证元素的唯一性?
        	 先比较元素的哈希值,再比较equals方法,只有两个结果都是true,才判断相同
        	 如果哈希表保存自定义类型,那么为了保证元素的唯一性,
  		我们必须重写自定义类型的hashCode和equals方法
  ****************************************************
  ```


  ```
7.哈希表结构保存自定义类型练习
public class Dog {
    int age;
    String name;
    public Dog() {
    }
    public Dog(int age, String name) {
        this.age = age;
        this.name = name;
    }
    //为了保证元素的唯一性,必须重写hashCode和equals方法,让这两个方法都和内容有关
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Dog dog = (Dog) o;
        return age == dog.age &&
                Objects.equals(name, dog.name);
    }
    @Override
    public int hashCode() {
        return Objects.hash(age, name);
    }

    @Override
    public String toString() {
        return "Dog{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

public class TestHashCodeDemo04 {
    public static void main(String[] args) {
        //1.创建一个哈希表结构的集合,保存自定义类型
        HashSet<Dog> set = new HashSet<Dog>();
        //2.保存
        set.add(new Dog(1, "旺小财"));
        set.add(new Dog(2, "旺中财"));
        set.add(new Dog(3, "旺大财"));
        set.add(new Dog(4, "旺老财"));
        set.add(new Dog(4, "旺老财"));
        set.add(new Dog(5, "旺死财"));
        //3.打印集合
        System.out.println(set);
    }
}
注意: 如果哈希表保存自定义类型,那么为了保证元素的唯一性,
		我们必须重写自定义类型的hashCode和equals方法
  ```

### 第三章 Map集合

##### 3.1 Map的概述

```java
Map集合特点:
a.Collection集合称为单列集合,Map集合称为双列集合
b.Map集合中键必须是唯一的,值是可以重复的
```

##### 3.2 Map的常用子类

```java
双列集合的根接口:Map<K,V>
常见的Map接口的实现类:
	HashMap
	LinkedHashMap
	TreeMap
```

##### 3.3 Map的通用方法**

```java
增: public V put(K 键,V 值); //添加一个不存的键时,返回值是null
删: public V remove(Object 键); //根据键,删除键值对,同时返回被删除的键值对中的值 
改: public V put(K 键,V 值); //添加一个已经存在的键时,此时作为修改,返回值是被修改前的值
查: public V get(K 键); //根据键查找值
其他方法:
	public boolean containsKey(K 键); //判断集合中是否有该键的存在

public class TestMapDemo {
    public static void main(String[] args) {
        //1.创建一个Map
        HashMap<String, Integer> map = new HashMap<String, Integer>();
        //2.添加
        Integer v1 = map.put("张三", 10);
        Integer v2 = map.put("李四", 6);
        Integer v3 = map.put("王五", 19);
        Integer v4 = map.put("赵六", 7);
        Integer v5 = map.put("前妻", 3);
        Integer v6 = map.put("王五", 888);
        System.out.println(v1);
        System.out.println(v2);
        System.out.println(v3);
        System.out.println(v4);
        System.out.println(v5);
        System.out.println(v6);
        System.out.println(map);
        System.out.println("===============");
        //3.删除
        Integer v7 = map.remove("张三");
        System.out.println(v7);
        System.out.println(map);
        //4.获取
        Integer v8 = map.get("张三");
        System.out.println(v8);
        System.out.println(map);
        //5.判断是否包含某个键
        boolean b = map.containsKey("六六");
        System.out.println(b);
    }
}
```

##### 3.4 Map的遍历

- ##### 遍历方式一:以键找值

  ```java
  public class MapDemo01 {
      public static void main(String[] args) {
          //Map的第一种遍历方式:以键找值
          //1.创建一个Map
          HashMap<String, Integer> map = new HashMap<String, Integer>();
          //2.添加
          map.put("张三", 10);
          map.put("李四", 6);
          map.put("王五", 19);
          map.put("赵六", 7);
          //3.遍历Map
          //a.获取所有键的集合
          Set<String> keys = map.keySet();
          //b.遍历该集合 迭代器,foreach
          for (String key : keys) {
              //c.以键找值
              Integer value = map.get(key);
              //打印
              System.out.println(key+"="+value);
          }
      }
  }
  ```

- ##### 遍历方式二:键值对对象

  ```java
  public class MapDemo02 {
      public static void main(String[] args) {
          //Map的第一种遍历方式:以键找值
          //1.创建一个Map
          HashMap<String, Integer> map = new HashMap<String, Integer>();
          //2.添加
          map.put("张三", 10);
          map.put("李四", 6);
          map.put("王五", 19);
          map.put("赵六", 7);
          //3.Map集合的遍历方式二:键值对对象方式
          //a.获取Map集合中所有的键值对对象的集合
          Set<Map.Entry<String, Integer>> entries = map.entrySet();
          //b.遍历该键值对对象的集合 迭代器,foreach
          for(Map.Entry<String, Integer> entry : entries){
              //c.调用键值对对象Entry的getKey和getValue
              String key = entry.getKey();
              Integer value = entry.getValue();
              System.out.println(key + "=" + value);
          }
      }
  }
  ```

##### 3.5 HashMap介绍

```java
HashMap的特有方法:无
HashMap的数据结构: 哈希表结构(无序)
HashMap的特点: 无索引,无序的,键是唯一的
注意: 如果键是自定义类型,重写hashCode和equals方法
```

##### 3.6 LinkedHashMap介绍

```java
LinkedHashMap的特有方法:无
LinkedHashMap的数据结构:链式哈希表结构(有序)
LinkedHashMap的特点: 无索引,有序的,键是唯一的 
注意: 如果键是自定义类型,重写hashCode和equals方法
```

- 练习:Map保存自定义类型的键

  ```java
  public class MapTestDemo01 {
      public static void main(String[] args) {
          //使用Map集合保存自定义类型的键
          //需求:学生作为键, 家庭住址作为值
          HashMap<Student,String> map = new HashMap<Student, String>();
          //添加数据
          map.put(new Student(18, "柳岩"), "北京中关村");
          map.put(new Student(22, "杨幂"), "你家隔壁");
          map.put(new Student(28, "班长"), "下水道");
          map.put(new Student(38, "鹏鹏"), "黑马程序员");
          //班长搬家,搬到柳岩隔壁
          map.put(new Student(28, "班长"), "柳岩隔壁");
          //打印
          System.out.println(map);
          //{Student{age=18,name='柳岩'}='北京中关村',键=值,..}
      }
  }
  注意:如果键是自定义类型,重写hashCode和equals方法
  ```

##### 3.7 TreeMap集合

```java
TreeMap的数据结构:二叉树
TreeMap的特点: 无索引,无序(键是自然顺序的),键是唯一

public class TestTreeMapDemo {
    public static void main(String[] args) {
        //1.创建TreeMap对象
        TreeMap<Integer, String> map = new TreeMap<Integer, String>();
        //2.添加
        map.put(2, "杨幂");
        map.put(1, "柳岩");
        map.put(4, "副班长");
        map.put(3, "班长");
        //3.输出
        System.out.println(map);
        
        //如果键是数值类型,按照数值的大小升序
        //如果键是字符类型,按照ASCII值大小升序
        //如果键是字符串类型,按照首字母的ASCII,次字母,..依次类推
        //如果键是自定义类型,需要在创建TreeMap时通过构造传入比较器对象
        TreeMap<Student,String> map = new TreeMap<Student, String>(new Comparator<Student>(){
            @Override
            public int compare(Student o1, Student o2) {
                //按照学生的年龄降序
//                return o2.getAge() - o1.getAge();
                //按照学生的姓名长度升序
                return o1.getName().length() - o2.getName().length();
            }
        });
        //添加数据
        map.put(new Student(18, "ady"), "北京中关村");
        map.put(new Student(22, "marry"), "你家隔壁");
        map.put(new Student(28, "rose"), "下水道");
        map.put(new Student(38, "hanmeimei"), "黑马程序员");
        //打印
        System.out.println(map);
    }
}
```

##### 3.8 Map集合练习

```java
需求:输入一个字符串中每个字符出现次数。
比如: 输入 HelloWorld
	==> h 1	e 1 l 3 o 2 W 1 r 1 d 1
分析步骤:
	1.输入字符串
	2.准备一个集合,保存程序的结果(应该采用map集合)
    3.遍历字符串取出每个字符,统计次数
     4.输出结果
     
public class MapTestDemo02 {
    public static void main(String[] args) {
//        1.输入字符串
        System.out.println("请输入一个字符串:");
        Scanner sc = new Scanner(System.in);
        String line = sc.nextLine();

        //2.准备一个集合,保存程序的结果(应该采用map集合)
        LinkedHashMap<Character, Integer> map = new LinkedHashMap<Character, Integer>();

        //3.遍历字符串取出每个字符,统计次数
        for (int i = 0; i < line.length(); i++) {
            //取出字符
            Character ch = line.charAt(i);
            //判断map中是否存在ch这个键
            if (map.containsKey(ch)) {
                //如果该字符ch不是第一次出现
                Integer count = map.get(ch);
                map.put(ch, count + 1);
            }else {
                //如果该字符ch是第一次出现
                map.put(ch, 1);
            } 
        }
        //4.打印输出
        System.out.println(map);
    }
}	
```

### 第四章 模拟斗地主洗牌发牌(升级版)

```java
斗地主自定义排序的步骤分析
1.创建一个编号和牌对应的map集合
2.准备一副牌(54个编号)
3.洗牌
4.发牌(发的编号)
5.排序(编号的排序,也是对牌的排序)
6.以键找值,然后才能看牌

public class SortDouDiZhu {
    public static void main(String[] args) {
//        斗地主自定义排序的步骤分析
//        1.创建一个编号和牌对应的map集合
        LinkedHashMap<Integer, String> map = new LinkedHashMap<Integer, String>();
        //a.定义一个编号
        int index = 1;
        //b.数值
        String[] nums = {"3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A", "2"};
        //c.花色
        String[] colors = {"♥", "♠", "♦", "♣"};
        //拼接
        for (String num : nums) {
            for (String color : colors) {
                //一张牌
                String card = color + num;
                //添加到map中
                map.put(index++,card);
            }
        }
        //d.大小王
        map.put(53, "小S");
        map.put(54, "大S");

//        2.准备一副牌(54个编号)
        ArrayList<Integer> cardBox = new ArrayList<Integer>();
        for (int i = 1; i < 55; i++) {
            cardBox.add(i);
        }
//        3.洗牌
        Collections.shuffle(cardBox);
//        4.发牌(发的编号)
        ArrayList<Integer> p1 = new ArrayList<Integer>();
        ArrayList<Integer> p2 = new ArrayList<Integer>();
        ArrayList<Integer> p3 = new ArrayList<Integer>();
        ArrayList<Integer> dp = new ArrayList<Integer>();
        //怎么发牌?
        for (int i = 0; i < cardBox.size() - 3; i++) {
            //取出牌
            Integer card = cardBox.get(i);
            //发给谁??
            if (i % 3 == 0) {
                p1.add(card);
            } else if (i % 3 == 1) {
                p2.add(card);
            } else{
                p3.add(card);
            }
        }
        //剩下三张给底牌
        dp.add(cardBox.get(51));
        dp.add(cardBox.get(52));
        dp.add(cardBox.get(53));

//        5.排序(编号的排序,也是对牌的排序)
        Collections.sort(p1);
        Collections.sort(p2);
        Collections.sort(p3);
        Collections.sort(dp);

//        6.以键找值,然后才能看牌
        lookCards(p1,map);
        lookCards(p2,map);
        lookCards(p3,map);
        lookCards(dp,map);

    }

    public static void lookCards(ArrayList<Integer> pp, LinkedHashMap<Integer, String> map) {
        for (Integer id : pp) {
            String card = map.get(id);
            System.out.print(card+"\t");
        }
        System.out.println();
    }
}
```

