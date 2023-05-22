# Java基础知识

- [Java基础知识](#java基础知识)
  - [一、Java中基本数据类型](#一java中基本数据类型)
  - [二、 重载和重写的区别](#二-重载和重写的区别)
  - [三、 equals与==的区别](#三-equals与的区别)
  - [四、介绍下Java中的内部类](#四介绍下java中的内部类)
  - [五、 Java的四种引用，强弱软虚](#五-java的四种引用强弱软虚)
  - [六、HashCode的作用](#六hashcode的作用)
  - [七、有没有可能两个不相等的对象有相同的hashcode](#七有没有可能两个不相等的对象有相同的hashcode)
  - [八、深拷贝和浅拷贝的区别是什么?](#八深拷贝和浅拷贝的区别是什么)
  - [九、static都有哪些用法?](#九static都有哪些用法)
  - [十、 介绍下Object中的常用方法](#十-介绍下object中的常用方法)
  - [十一、Java创建对象有几种方式？](#十一java创建对象有几种方式)
  - [十二、获取一个类Class对象的方式有哪些？](#十二获取一个类class对象的方式有哪些)
  - [十三、介绍下你对Java集合的理解](#十三介绍下你对java集合的理解)
  - [十四、有了数组为什么还要再搞一个ArrayList呢？](#十四有了数组为什么还要再搞一个arraylist呢)
  - [十五、说说什么是 fail-fast？](#十五说说什么是-fail-fast)
  - [十六、HashMap 与 ConcurrentHashMap 的异同](#十六hashmap-与-concurrenthashmap-的异同)
  - [十七、介绍下你对红黑树的理解](#十七介绍下你对红黑树的理解)
  - [十八、OOM你遇到过哪些情况，SOF你遇到过哪些情况](#十八oom你遇到过哪些情况sof你遇到过哪些情况)
    - [SOF（堆栈溢出StackOverflow）](#sof堆栈溢出stackoverflow)
    - [OOM](#oom)
  - [十九、异常处理影响性能吗](#十九异常处理影响性能吗)
  - [二十、JVM、JRE和JDK的关系是什么](#二十jvmjre和jdk的关系是什么)
  - [二十一、什么是字节码](#二十一什么是字节码)
  - [二十二、final、finally、finalize的区别](#二十二finalfinallyfinalize的区别)
    - [final 用于修饰变量、方法和类](#final-用于修饰变量方法和类)
    - [finally](#finally)
    - [finalize](#finalize)
  - [String、StringBuffer和StringBuilder的区别是什么](#stringstringbuffer和stringbuilder的区别是什么)

## 一、Java中基本数据类型

| 基本类型 | 字节大小 |    默认值    |  封装类   |
| :------: | :------: | :----------: | :-------: |
|   byte   |    1     |   (byte)0    |   Byte    |
|  short   |    2     |   (short)0   |   Short   |
|   int    |    4     |      0       |  Integer  |
|   long   |    8     |      0L      |   Long    |
|  float   |    4     |     0.0f     |   Float   |
|  double  |    8     |     0.0d     |  Double   |
| boolean  |          |    false     |  Boolean  |
|   char   |    2     | \u0000(null) | Character |

boolean: int 4个字节

需要注意：

1. int是基本数据类型，Integer是int的封装类，是引用类型。int默认值是0，而Integer默认值是null，所以Integer能区分出0和null的情况。一旦java看到null，就知道这个引用还没有指向某个对象，再任何引用使用前，必须为其指定一个对象，否则会报错。
2. 基本数据类型在声明时系统会自动给它分配空间，而引用类型声明时只是分配了引用空间，必须通过实例化开辟数据空间之后才可以赋值。数组对象也是一个引用对象，将一个数组赋值给另一个数组时只是复制了一个引用，所以通过某一个数组所做的修改在另一个数组中也看的见。虽然定义了boolean这种数据类型，但是只对它提供了非常有限的支持。在Java虚拟机中没有任何供boolean值专用的字节码指令，Java语言表达式所操作的boolean值，在编译之后都使用Java虚拟机中的int数据类型来代替，而boolean数组将会被编码成Java虚拟机的byte数组，每个元素boolean元素占8位。这样我们可以得出boolean类型占了单独使用是4个字节，在数组中又是1个字节。使用int的原因是，对于当下32位的处理器（CPU）来说，一次处理数是32位（这里不是指的是32/64位系统，而是指CPU硬件层面），具有高效存取的特点。

## 二、 重载和重写的区别

**重写(Override)**：
从字面上看，重写就是 重新写一遍的意思。其实就是在子类中把父类本身有的方法重新写一遍。子
类继承了父类原有的方法，但有时子类并不想原封不动的继承父类中的某个方法，所以在方法名，
参数列表，返回类型(除过子类中方法的返回值是父类中方法返回值的子类时)都相同的情况下， 对
方法体进行修改或重写，这就是重写。但要注意子类函数的访问修饰权限不能少于父类的。

```text
public class Main {
 public static void main(String[] args) {
 
 Double i1 = 100.0;
 Double i2 = 100.0;
 Double i3 = 200.0;
 Double i4 = 200.0;
 
 System.out.println(i1==i2);
 System.out.println(i3==i4);
 }
}
false
false
public class Father {
 public static void main(String[] args) {
 // TODO Auto-generated method stub
 Son s = new Son();
 s.sayHello();
 }
 public void sayHello() {
 System.out.println("Hello");
 }
}
class Son extends Father{
    @Override
    public void sayHello() {
    // TODO Auto-generated method stub
    System.out.println("hello by ");
    }
}
```

**重写 总结：** 1.发生在父类与子类之间 2.方法名，参数列表，返回类型（除过子类中方法的返回类型
是父类中返回类型的子类）必须相同 3.访问修饰符的限制一定要大于被重写方法的访问修饰符
（public>protected>default>private) 4.重写方法一定不能抛出新的检查异常或者比被重写方法申
明更加宽泛的检查型异常

**重载（Overload）**：同一个Java类同名、同参、不同返回类型
在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同甚至是参数顺序不
同）则视为重载。同时，重载对返回类型没有要求，可以相同也可以不同，但不能通过返回类型是
否相同来判断重载。

```text
public class Father {
 public static void main(String[] args) {
 // TODO Auto-generated method stub
 Father s = new Father();
 s.sayHello();
 s.sayHello("wintershii");
 }
 public void sayHello() {
 System.out.println("Hello");
 }
 public void sayHello(String name) {
 System.out.println("Hello" + " " + name);
 }
}
```

**重载 总结：** 1.重载Overload是一个类中多态性的一种表现 2.重载要求同名方法的参数列表不同(参
数类型，参数个数甚至是参数顺序) 3.重载的时候，返回值类型可以相同也可以不相同。无法以返回
型别作为重载函数的区分标准

## 三、 equals与==的区别

**== ：**
== 比较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是
否是指相同一个对象。比较的是真正意义上的指针操作。
1、比较的是操作符两端的操作数是否是同一个对象。 2、两边的操作数必须是同一类型的（可以是
父子类之间）才能编译通过。 3、比较的是地址，如果是具体的阿拉伯数字的比较，值相等则为
true，如： int a=10 与 long b=10L 与 double c=10.0都是相同的（为true），因为他们都指向地
址为10的堆。

**equals：**
equals用来比较的是两个对象的内容是否相等，由于所有的类都是继承自java.lang.Object类的，所
以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是Object类中的方法，而Object
中的equals方法返回的却是==的判断。
总结：
所有比较是否相等时，都是用equals 并且在对常量相比较时，把常量写在前面，因为使用object的
equals object可能为null 则空指针
在阿里的代码规范中只使用equals ，阿里插件默认会识别，并可以快速修改，推荐安装阿里插件来
排查老代码使用“==”，替换成equals

## 四、介绍下Java中的内部类

**目的：**提高安全性，使用灵活

在Java中，可以将一个类定义在另一个类里面或者一个方法里面，这样的类称为内部类。广泛意义上的内部类一般来说包括这三种：成员内部类、局部内部类、匿名内部类

![Java内部类](../images/Java内部类.png)

内部类访问外部变量需要该变量用final修饰。

原因：跟方法的生命周期有关系。方法声明周期跟变量周期不一致，GC回回收造成错误。

## 五、 Java的四种引用，强弱软虚

- 强引用

强引用是平常中使用最多的引用，强引用在程序内存不足（OOM）的时候也不会被回收，

**使用方式：**

```text
String str = new String("str");
System.out.println(str);
```

- 软引用
软引用在程序内存不足时，会被回收，使用方式：

```text
// 注意：wrf这个引用也是强引用，它是指向SoftReference这个对象的，
// 这里的软引用指的是指向new String("str")的引用，也就是SoftReference类中T
SoftReference<String> wrf = new SoftReference<String>(new String("str"));
```

​	**可用场景：** 创建缓存的时候，创建的对象放进缓存中，当内存不足时，JVM就会回收早先创建的对象。

- 弱引用
弱引用就是只要JVM垃圾回收器发现了它，就会将之回收，使用方式：

```text
WeakReference<String> wrf = new WeakReference<String>(str);
```

**可用场景：** Java源码中的 java.util.WeakHashMap 中的 key 就是使用弱引用，我的理解就是，一旦我不需要某个引用，JVM会自动帮我处理它，这样我就不需要做其它操作。

- 虚引用
虚引用的回收机制跟弱引用差不多，但是它被回收之前，会被放入 ReferenceQueue 中。注意
哦，其它引用是被JVM回收后才被传入 ReferenceQueue 中的。由于这个机制，所以虚引用大多
被用于引用销毁前的处理工作。还有就是，虚引用创建的时候，必须带有 ReferenceQueue ，
**使用例子：**

```text
PhantomReference<String> prf = new PhantomReference<String>(new String("str"),
new ReferenceQueue<>());
```

**可用场景：** 对象销毁前的一些操作，比如说资源释放等。 Object.finalize() 虽然也可以做这类动作，但是这个方式即不安全又低效
上诉所说的几类引用，都是指对象本身的引用，而不是指Reference的四个子类的引用(SoftReference等)

## 六、HashCode的作用

java的集合有两类，一类是List，还有一类是Set。前者有序可重复，后者无序不重复。当我们在set中插入的时候怎么判断是否已经存在该元素呢，可以通过equals方法。但是如果元素太多，用这样的方法就会比较满。于是有人发明了哈希算法来提高集合中查找元素的效率。 

这种方式将集合分成若干个存储区域，每个对象可以计算出一个哈希码，可以将哈希码分组，每组分别对应某个存储区域，根据一个对象的哈希码就可以确定该对象应该存储的那个区域。

hashCode方法可以这样理解：它返回的就是根据对象的内存地址换算出的一个值。这样一来，当集合要添加新的元素时，先调用这个元素的hashCode方法，就一下子能定位到它应该放置的物理位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。

这样一来实际调用equals方法的次数就大大降低了，几乎只需要一两次。

## 七、有没有可能两个不相等的对象有相同的hashcode

能.在产生hash冲突时,两个不相等的对象就会有相同的 hashcode 值.当hash冲突产生时,一般
有以下几种方式来处理:

- 拉链法:每个哈希表节点都有一个next指针,多个哈希表节点可以用next指针构成一个单向链
  表，被分配到同一个索引上的多个节点可以用这个单向链表进行存储.
- 开放定址法:一旦发生了冲突,就去寻找下一个空的散列地址,只要散列表足够大,空的散列地址总
  能找到,并将记录存入
- 再哈希:又叫双哈希法,有多个不同的Hash函数.当发生冲突时,使用第二个,第三个….等哈希函数
  计算地址,直到无冲突

## 八、深拷贝和浅拷贝的区别是什么?

原型模式：设计模式 --> Spring bean的Scope

**浅拷贝**:被复制对象的所有变量都含有与原来的对象相同的值,而所有的对其他对象的引用仍然指
向原来的对象.换言之,浅拷贝仅仅复制所考虑的对象,而不复制它所引用的对象.

![image.png](../images/浅克隆.png)

**深拷贝**:被复制对象的所有变量都含有与原来的对象相同的值.而那些引用其他对象的变量将指向
被复制过的新对象.而不再是原有的那些被引用的对象.换言之.深拷贝把要复制的对象所引用的
对象都复制了一遍

![image.png](../images/深克隆.png)

## 九、static都有哪些用法?

所有的人都知道static关键字这两个基本的用法:静态变量和静态方法.也就是被static所修饰的变量

方法都属于类的静态资源,类实例所共享.

除了静态变量和静态方法之外,static也用于静态块,多用于初始化操作:

```java
public calss PreCache{
 static{
 //执行相关操作
 }
}
```

此外static也多用于修饰内部类,此时称之为静态内部类.

最后一种用法就是静态导包,即 import static .import static是在JDK 1.5之后引入的新特性,可以用
来指定导入某个类中的静态资源,并且不需要使用类名,可以直接使用资源名,比如:

```
import static java.lang.Math.*;
public class Test{
 public static void main(String[] args){
 //System.out.println(Math.sin(20));传统做法
 System.out.println(sin(20));
 }
}
```

## 十、 介绍下Object中的常用方法

![Object中的成员方法](../images/Object的成员方法.png)

**clone 方法**
保护方法，实现对象的浅复制，只有实现了 Cloneable 接口才可以调用该方法，否则抛出
CloneNotSupportedException 异常，深拷贝也需要实现 Cloneable，同时其成员变量为引用类型
的也需要实现 Cloneable，然后重写 clone 方法。

**finalize 方法**
该方法和垃圾收集器有关系，判断一个对象是否可以被回收的最后一步就是判断是否重写了此方
法。

**equals 方法**
该方法使用频率非常高。一般 equals 和 == 是不一样的，但是在 Object 中两者是一样的。子类一
般都要重写这个方法。

**hashCode 方法**
该方法用于哈希查找，重写了 equals 方法一般都要重写 hashCode 方法，这个方法在一些具有哈
希功能的 Collection 中用到。

一般必须满足 obj1.equals(obj2)==true 。可以推出 obj1.hashCode()==obj2.hashCode() ，但是
hashCode 相等不一定就满足 equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

* JDK 1.6、1.7 默认是返回随机数；
* JDK 1.8 默认是通过和当前线程有关的一个随机数 + 三个确定值，运用 Marsaglia’s xorshift
  scheme 随机数算法得到的一个随机数。

**wait 方法**
&emsp;&emsp;配合 synchronized 使用，wait 方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait() 方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。
调用该方法后当前线程进入睡眠状态，直到以下事件发生。

1. 其他线程调用了该对象的 notify 方法；
2. 其他线程调用了该对象的 notifyAll 方法；
3. 其他线程调用了 interrupt 中断该线程；
4. 时间间隔到了。
   此时该线程就可以被调度了，如果是被中断的话就抛出一个 InterruptedException 异常。

**notify 方法**
配合 synchronized 使用，该方法唤醒在该对象上等待队列中的某个线程（同步队列中的线程是给抢占 CPU 的线程，等待队列中的线程指的是等待唤醒的线程）。
**notifyAll 方法**
配合 synchronized 使用，该方法唤醒在该对象上等待队列中的所有线程。
**总结**
只要把上面几个方法熟悉就可以了，toString 和 getClass 方法可以不用去讨论它们。该题目考察的是对 Object 的熟悉程度，平时用的很多方法并没看其定义但是也在用，比如说：wait() 方法，equals() 方法等。

```txt
Class Object is the root of the class hierarchy.Every class has Object as a
superclass. All objects, including arrays, implement the methods of this class.
```

大致意思：Object 是所有类的根，是所有类的父类，所有对象包括数组都实现了 Object 的方法。

## 十一、Java创建对象有几种方式？

java中提供了以下四种创建对象的方式:

**new 关键字**
平时使用的最多的创建对象方式

```java
User user=new User();
```

**反射方式**
使用 newInstance()，但是得处理两个异常 InstantiationException、IllegalAccessException：

```java
User user=User.class.newInstance();
Object object=(Object)Class.forName("java.lang.Object").newInstance()
```

**clone方法**
Object对象中的clone方法来完成这个操作

**反序列化操作**
调用 ObjectInputStream 类的 readObject() 方法。我们反序列化一个对象，JVM 会给我们创建一个单独的对象。JVM 创建对象并不会调用任何构造函数。一个对象实现了 Serializable 接口，就可以把对象写入到文中，并通过读取文件来创建对象。

**总结**
创建对象的方式关键字：new、反射、clone 拷贝、反序列化。

## 十二、获取一个类Class对象的方式有哪些？

搞清楚类对象和实例对象，但都是对象。

**第一种：**通过类对象的 getClass() 方法获取，细心点的都知道，这个 getClass 是 Object 类里面的
方法。

```text
User user=new User();
//clazz就是一个User的类对象
Class<?> clazz=user.getClass();
```

**第二种：**通过类的静态成员表示，每个类都有隐含的静态成员 class。

```text
//clazz就是一个User的类对象
Class<?> clazz=User.class;
```

**第三种：**通过 Class 类的静态方法 forName() 方法获取。

```text
Class<?> clazz = Class.forName("com.tian.User");
```

## 十三、介绍下你对Java集合的理解

![Java中的集合](../images/Java中的集合.png)

TreeSet的本质是TreeMap

HashSet的本质是HashMap

## 十四、有了数组为什么还要再搞一个ArrayList呢？

&emsp;&emsp;通常我们在使用的时候，如果在不明确要插入多少数据的情况下，普通数组就很尴尬了，因为你不知道需要初始化数组大小为多少，而 ArrayList 可以使用默认的大小，当元素个数到达一定程度后，会自动扩容。
&emsp;&emsp;可以这么来理解：我们常说的数组是定死的数组，ArrayList 却是动态数组。

## 十五、说说什么是 fail-fast？

**&emsp;&emsp;fail-fast** 机制是 Java 集合（Collection）中的一种错误机制。当多个线程对同一个集合的内容进行操作时，就可能会产生 fail-fast 事件。
&emsp;&emsp;例如：当某一个线程 A 通过 iterator 去遍历某集合的过程中，若该集合的内容被其他线程所改变了，那么线程 A 访问集合时，就会抛出 ConcurrentModificationException 异常，产生 fail-fast 事件。这里的操作主要是指 add、remove 和 clear，对集合元素个数进行修改。
**&emsp;&emsp;解决办法**：建议使用“java.util.concurrent 包下的类”去取代“java.util 包下的类”。可以这么理解：在遍历之前，把 modCount 记下来 expectModCount，后面 expectModCount 去和 modCount 进行比较，如果不相等了，证明已并发了，被修改了，于是抛出ConcurrentModificationException 异常。

## 十六、HashMap 与 ConcurrentHashMap 的异同

1. 都是 key-value 形式的存储数据；
2. HashMap 是线程不安全的，ConcurrentHashMap 是 JUC 下的线程安全的；
3. HashMap 底层数据结构是数组 + 链表（JDK 1.8 之前）。JDK 1.8 之后是数组 + 链表 + 红黑
   树。当链表中元素个数达到 8 的时候，链表的查询速度不如红黑树快，链表会转为红黑树，红黑树查询速度快；
4. HashMap 初始数组大小为 16（默认），当出现扩容的时候，以 0.75 * 数组大小的方式进行扩容；
5. ConcurrentHashMap 在 JDK 1.8 之前是采用分段锁来现实的 Segment + HashEntry，
   Segment 数组大小默认是 16，2 的 n 次方；JDK 1.8 之后，采用 Node + CAS + Synchronized来保证并发安全进行实现

## 十七、介绍下你对红黑树的理解

红黑树的特点：

![image.png](../images/红黑树的特点.png)

红黑色的本质：2-3-4树

红黑树保证黑节点平衡的方式:左旋/右旋+变色 来保证

## 十八、OOM你遇到过哪些情况，SOF你遇到过哪些情况

### SOF（堆栈溢出StackOverflow）

**StackOverflowError 的定义：**当应用程序递归太深而发生堆栈溢出时，抛出该错误。

因为栈一般默认为1-2m，一旦出现死循环或者是大量的递归调用，在不断的压栈过程中，造成栈容
量超过1m而导致溢出。

**栈溢出的原因：**递归调用，大量循环或死循环，全局变量是否过多，数组、List、map数据过大。

### OOM

1. OutOfMemoryError异常
除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生OutOfMemoryError(OOM)异常的
可能。
Java Heap 溢出：
一般的异常信息：java.lang.OutOfMemoryError:Java heap spacess。
java堆用于存储对象实例，我们只要不断的创建对象，并且保证GC Roots到对象之间有可达路径来
避免垃圾回收机制清除这些对象，就会在对象数量达到最大堆容量限制后产生内存溢出异常。

出现这种异常，一般手段是先通过内存映像分析工具(如Eclipse Memory Analyzer)对dump出来的
堆转存快照进行分析，重点是确认内存中的对象是否是必要的，先分清是因为内存泄漏(Memory
Leak)还是内存溢出(Memory Overflow)。
如果是内存泄漏，可进一步通过工具查看泄漏对象到GCRoots的引用链。于是就能找到泄漏对象是
通过怎样的路径与GC Roots相关联并导致垃圾收集器无法自动回收。
如果不存在泄漏，那就应该检查虚拟机的参数(-Xmx与-Xms)的设置是否适当。
2. 虚拟机栈和本地方法栈溢出
如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常。
如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常
这里需要注意当栈的大小越大可分配的线程数就越少。
3. 运行时常量池溢出
异常信息：java.lang.OutOfMemoryError:PermGenspace
如果要向运行时常量池中添加内容，最简单的做法就是使用String.intern()这个Native方法。该方法
的作用是：如果池中已经包含一个等于此String的字符串，则返回代表池中这个字符串的String对
象；否则，将此String对象包含的字符串添加到常量池中，并且返回此String对象的引用。由于常量
池分配在方法区内，我们可以通过-XX:PermSize和-XX:MaxPermSize限制方法区的大小，从而间接
限制其中常量池的容量
4. 方法区溢出

方法区用于存放Class的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。也有可
能是方法区中保存的class对象没有被及时回收掉或者class信息占用的内存超过了我们配置。
异常信息：java.lang.OutOfMemoryError:PermGenspace
方法区溢出也是一种常见的内存溢出异常，一个类如果要被垃圾收集器回收，判定条件是很苛刻
的。在经常动态生成大量Class的应用中，要特别注意这点。

## 十九、异常处理影响性能吗

异常处理的性能成本非常高，每个 Java 程序员在开发时都应牢记这句话。创建一个异常非常慢，抛出一个异常又会消耗1~5ms，当一个异常在应用的多个层级之间传递时，会拖累整个应用的性能。

仅在异常情况下使用异常；在可恢复的异常情况下使用异常；尽管使用异常有利于 Java 开发，但是在应用中最好不要捕获太多的调用栈，因为在很多情况下都不需要打印调用栈就知道哪里出错了。因此，异常消息应该提供恰到好处的信息。


## 二十、JVM、JRE和JDK的关系是什么

JDK是（Java Development Kit）的缩写，它是功能齐全的 Java SDK。它拥有 JRE 所拥有的一切，还有编译器（javac）和工具（如 javadoc 和 jdb）。它能够创建和编译程序。

JRE是Java Runtime Environment缩写，它是运行已编译 Java 程序所需的所有内容的集合，包括 Java 虚拟机（JVM），Java 类库，java 命令和其他的一些基础构件。但是，它不能用于创建新程序。

JDK包含JRE，JRE包含JVM。

## 二十一、什么是字节码

之所以被称之为字节码，是**因为字节码文件由十六进制值组成，而JVM以两个十六进制值为一组，即以字节为单位进行读取**。

在Java中一般是用javac命令编译源代码为字节码文件，编译生成固定格式的字节码（.class文件）供JVM使用

## 二十二、final、finally、finalize的区别

### final 用于修饰变量、方法和类

- final 变量：被修饰的变量不可变，不可变分为引用不可变和对象不可变，final 指的是引用不可变，final 修饰的变量必须初始化，通常称被修饰的变量为常量。
- final 方法：被修饰的方法不允许任何子类重写，子类可以使用该方法。
- final 类：被修饰的类不能被继承，所有方法不能被重写。

### finally

finally 作为异常处理的一部分，它只能在 try/catch 语句中，并且附带一个语句块表示这段语句最终一定被执行（无论是否抛出异常），经常被用在需要释放资源的情况下，System.exit (0) 可以阻断 finally 执行。

### finalize

- finalize 是在 java.lang.Object 里定义的方法，也就是说每一个对象都有这么个方法，这个方法在 gc 启动，该对象被回收的时候被调用。
- 一个对象的 finalize 方法只会被调用一次，finalize 被调用不一定会立即回收该对象，所以有可能调用 finalize 后，该对象又不需要被回收了，然后到了真正要被回收的时候，因为前面调用过一次，所以不会再次调用 finalize 了，进而产生问题，因此不推荐使用 finalize 方法。

## String、StringBuffer和StringBuilder的区别是什么

String是只读字符串，它并不是基本数据类型，而是一个对象。从底层源码来看是一个final类型的
字符数组，所引用的字符串不能被改变，一经定义，无法再增删改。每次对String的操作都会生成
新的String对象。
每次+操作 ： 隐式在堆上new了一个跟原字符串相同的StringBuilder对象，再调用append方法 拼
接+后面的字符。
StringBuffer和StringBuilder他们两都继承了AbstractStringBuilder抽象类，从
AbstractStringBuilder抽象类中我们可以看到
他们的底层都是可变的字符数组，所以在进行频繁的字符串操作时，建议使用StringBuffer和
StringBuilder来进行操作。 另外StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所
以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。