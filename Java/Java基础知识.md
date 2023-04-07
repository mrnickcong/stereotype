# Java基础知识

## 一、 Java的四种引用，强弱软虚

- 强引用

强引用是平常中使用最多的引用，强引用在程序内存不足（OOM）的时候也不会被回收，使用
方式：

- 软引用
软引用在程序内存不足时，会被回收，使用方式：
可用场景： 创建缓存的时候，创建的对象放进缓存中，当内存不足时，JVM就会回收早先创建
的对象。

- 弱引用
弱引用就是只要JVM垃圾回收器发现了它，就会将之回收，使用方式：
可用场景： Java源码中的 java.util.WeakHashMap 中的 key 就是使用弱引用，我的理解就是，
一旦我不需要某个引用，JVM会自动帮我处理它，这样我就不需要做其它操作。

- 虚引用
虚引用的回收机制跟弱引用差不多，但是它被回收之前，会被放入 ReferenceQueue 中。注意
哦，其它引用是被JVM回收后才被传入 ReferenceQueue 中的。由于这个机制，所以虚引用大多
被用于引用销毁前的处理工作。还有就是，虚引用创建的时候，必须带有 ReferenceQueue ，
使用例子：
String str = new String("str");
System.out.println(str);
// 注意：wrf这个引用也是强引用，它是指向SoftReference这个对象的，
// 这里的软引用指的是指向new String("str")的引用，也就是SoftReference类中T
SoftReference<String> wrf = new SoftReference<String>(new String("str"));
WeakReference<String> wrf = new WeakReference<String>(str);
PhantomReference<String> prf = new PhantomReference<String>(new String("str"),
new ReferenceQueue<>());
可用场景： 对象销毁前的一些操作，比如说资源释放等。 Object.finalize() 虽然也可以做这
类动作，但是这个方式即不安全又低效
上诉所说的几类引用，都是指对象本身的引用，而不是指Reference的四个子类的引用
(SoftReference等)。