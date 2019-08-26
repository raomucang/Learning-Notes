# equals方法 和 hashCode方法

[这里有一篇文章讲解的很好，这里有很多部分都参考了这篇文章，参考文章作者CSDN：flyingsen](https://blog.csdn.net/zj15527620802/article/details/88547914)

____________________________________

`equals`方法和`hashCode`方法是属于`Object`类的方法

## equals

### equals方法在Java api中的定义是

```Java
public boolean equals​(Object obj)指示一些其他对象是否等于此。
equals方法在非空对象引用上实现等价关系：

自反性 ：对于任何非空的参考值x ， x.equals(x)应该返回true 。
它是对称的 ：对于任何非空引用值x和y ， x.equals(y)应该返回true当且仅当y.equals(x)回报true 。
传递性 ：对于任何非空引用值x ， y ，并z ，如果x.equals(y)回报true和y.equals(z)回报true ，然后x.equals(z)应该返回
true 。
它是一致的 ：对于任何非空参考值x和y ，多重调用x.equals(y)一致返回true或一致返回false ，前提是在equals对对象进行比较
的信息被修改。
对于任何非空的参考值x ， x.equals(null)应该返回false 。
该equals类方法Object实现对象上差别可能性最大的相等关系; 也就是说，对于任何非空参考值x和y ，当且仅当x和y引用相同对象
（ x == y具有值true ）时，该方法返回true 。

请注意，无论何时覆盖此方法，通常需要覆盖hashCode方法，以便维护hashCode方法的一般合同，该方法规定相等的对象必须具有相等
的哈希码。

参数
obj - 与之比较的参考对象。

结果
true如果此对象与obj参数相同; 否则为false 。

另请参见：
hashCode() ， HashMap
```

### 重写equals方法

```Java
public boolean equals(Object o) {
        //如果当前对象和要比较的对象是同一个对象，那么返回true
        if (this == o) return true;
        //如果要比较的对象不是当前对象的类的实例化对象，则返回false
        if (!(o instanceof Demo)) return false;
        Demo demo = (Demo) o;
        //如果当前对象与所比较的对象的所有属性都相等，那么返回true，否则返回true
        return age == demo.age &&
                name.equals(demo.name);
    }
```

## hashCode

### hashCode方法在Java api中的定义是

```Java
public int hashCode​()返回对象的哈希码值。支持这种方法有利于哈希表，如HashMap提供的那样。
hashCode的总合同是：

无论何时在执行Java应用程序时多次调用同一对象， hashCode方法必须始终返回相同的整数，前提是修改了对象中equals比较中信息。
该整数不需要从一个应用程序的执行到相同应用程序的另一个执行保持一致。
如果根据equals(Object)方法两个对象相等，则在两个对象中的每个对象上调用hashCode方法必须产生相同的整数结果。
不要求如果两个对象根据equals(java.lang.Object)方法不相等，则在两个对象中的每一个上调用hashCode方法必须产生不同的整数结果。
但是，程序员应该意识到，为不等对象生成不同的整数结果可能会提高哈希表的性能。
尽可能合理实用，由类Object定义的hashCode方法确实为不同对象返回不同的整数。（在某个时间点，hashCode可能或可能不被
实现为对象的存储器地址的某些功能。）

结果
该对象的哈希码值。

另请参见：
equals(java.lang.Object) ， System.identityHashCode(java.lang.Object) 

```

### 重写hashCode方法

Java ide自动生成的代码是

```Java
public int hashCode() {
        return Objects.hash(name, age);
    }
```

这表明重写的方法是调用了`Objects`类的`hash`方法，继续找下去，发现Objects类使用可变参数把对象的属性都传入了`Arrays`类的`hashCode`方法

```Java
public static int hash(Object... values) {
        return Arrays.hashCode(values);
    }
```

`Arrays`类的`hashCode`方法，代码如下

```Java
public static int hashCode(Object a[]) {
    //如果传递过来的对象为null，直接返回0
        if (a == null)
            return 0;
    //设置一个变量result，用来存储后面计算哈希值是算出的中间值和最终的哈希值
        int result = 1;
    //遍历a数组，a数组中存储的就是对象的属性
        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());

        return result;
    }
```

`Arrays`类计算属性的哈希值调用的是相应类型的`hashCode`方法，其中`String`类型的`hashCode`方法如下

```Java
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
```

从上面的代码中我们可以发现在计算哈希值时使用的是数字`31`,这是因为
1.`31`是一个不大不小的质数，是作为`hashCode`乘子的优选质数之一
2.`31`可以被`JVM`优化，`31 × N`可以被编译器优化为左移`N`位后减`1`，即`31 × N = (N << 5) - 1`

### 注意

**需要注意的是当equals()方法被override时，hashCode()也要被override。按照一般hashCode()方法的实现来说，相等的对象，它们的hash code一定相等。**

想要弄明白hashCode的作用，必须要先知道Java中的集合。
　　
总的来说，Java中的集合（Collection）有两类，一类是List，再有一类是Set。前者集合内的元素是有序的，元素可以重复；后者元素无序，但元素不可重复。这里就引出一个问题：要想保证元素不重复，可两个元素是否重复应该依据什么来判断呢？

这就是Object.equals方法了。但是，如果每增加一个元素就检查一次，那么当元素很多时，后添加到集合中的元素比较的次数就非常多了。也就是说，如果集合中现在已经有1000个元素，那么第1001个元素加入集合时，它就要调用1000次equals方法。这显然会大大降低效率。

于是，Java采用了哈希表的原理。哈希（Hash）实际上是个人名，由于他提出一哈希算法的概念，所以就以他的名字命名了。哈希算法也称为散列算法，是将数据依特定算法直接指定到一个地址上，初学者可以简单理解，hashCode方法实际上返回的就是对象存储的物理地址（实际可能并不是）。

这样一来，当集合要添加新的元素时，先调用这个元素的hashCode方法，就一下子能定位到它应该放置的物理位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。所以这里存在一个冲突解决的问题。这样一来实际调用equals方法的次数就大大降低了，几乎只需要一两次。  

**简而言之，在集合查找时，hashCode能大大降低对象比较次数，提高查找效率！**

Java对象的equals方法和hashCode方法是这样规定的：

**1、相等（相同）的对象必须具有相等的哈希码（或者散列码）。**

**2、如果两个对象的hashCode相同，它们并不一定相同。**
