# 面试题索引

- [面试题索引](#面试题索引)
  - [计算机基础篇](#计算机基础篇)
  - [基础篇](#基础篇)
    - [Java语言基础](#java语言基础)
    - [Java集合](#java集合)
  - [多线程和高并发](#多线程和高并发)
  - [JVM篇](#jvm篇)
  - [Spring篇](#spring篇)
  - [SpringMVC篇](#springmvc篇)
  - [SpringBoot篇](#springboot篇)
  - [Redis篇](#redis篇)
  - [MySQL篇](#mysql篇)
    - [MySQL原理部分](#mysql原理部分)
    - [MySQL索引部分](#mysql索引部分)
    - [MySQL集群高可用](#mysql集群高可用)
    - [MySQL其他问题](#mysql其他问题)
  - [MyBatis](#mybatis)
  - [消息中间件](#消息中间件)
    - [kafka](#kafka)
  - [微服务组件](#微服务组件)
    - [注册中心](#注册中心)
    - [服务降级](#服务降级)
  - [分布式系统解决方案](#分布式系统解决方案)
    - [分布式锁](#分布式锁)
    - [分布式事务](#分布式事务)
    - [分布式架构](#分布式架构)
  - [敏捷开发](#敏捷开发)
  - [大数据](#大数据)

## 计算机基础篇

1. [零拷贝原理](./computer-base/%E9%9B%B6%E6%8B%B7%E8%B4%9D%E5%8E%9F%E7%90%86.md)
2. [多路复用模型](./computer-base/%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8%E6%A8%A1%E5%9E%8B.md)

## [基础篇](./java/0.md)

### Java语言基础

1. [String、StringBuffer 和 StringBuilder 的区别是什么](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
2. [Java的四种引用，强弱软虚](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
3. [深拷贝和浅拷贝的区别是什么](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
4. [介绍下Object中的常用方法](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
5. [OOM你遇到过哪些情况，SOF你遇到过哪些情况](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
6. [final、finally、finalize的区别](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
7. [获取一个类Class对象的方式有哪些](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
8. [说说什么是 fail-fast](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
9. [什么是字节码](./java/JavaSE-%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)

### Java集合

1. [简述Java的集合](./java/Java%E9%9B%86%E5%90%88.md)
2. [说说fail-fast与fail-safe机制](./java/Java%E9%9B%86%E5%90%88.md)
3. [HashMap 与 ConcurrentHashMap 的异同](./java/Java%E9%9B%86%E5%90%88.md)
4. [HashMap](./java/HashMap.md)
5. [ConcurrentHashMap](./java/ConcurrentHashMap.md)

## 多线程和高并发

1. [一、原子性高频问题](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [1.1  Java中如何实现线程安全?](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [1.2 CAS底层实现](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [1.3 CAS的常见问题](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   4. [1.4 四种引用类型 + ThreadLocal的问题？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
2. [二、可见性高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [2.1 Java的内存模型](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [2.2 保证可见性的方式](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [2.3 volatile修饰引用数据类型](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   4. [2.4 有了MESI协议，为啥还有volatile？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   5. [2.5 volatile的可见性底层实现](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
3. [三、有序性高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [3.1 什么是有序性问题](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [3.2 volatile的有序性底层实现](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
4. [四、synchronized高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [4.1 synchronized锁升级的过程?](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [4.2 synchronized锁粗化\&锁消除？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [4.3 synchronized实现互斥性的原理？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   4. [4.4 wait为什么是Object下的方法？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   5. [深入理解synchronized锁升级过程](./multithreading-high-concurrency/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3synchronized%E9%94%81%E5%8D%87%E7%BA%A7%E8%BF%87%E7%A8%8B.md)
   6. [synchronized解析及锁膨胀过程](https://developer.aliyun.com/article/898122)
5. [一、AQS高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [1.1 AQS是什么？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [1.2 唤醒线程时，AQS为什么从后往前遍历？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [1.3 AQS为什么用双向链表，（为啥不用单向链表）？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   4. [1.4 AQS为什么要有一个虚拟的head节点](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   5. [1.5 ReentrantLock的底层实现原理](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   6. [1.6 ReentrantLock的公平锁和非公平锁的区别](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   7. [1.7 ReentrantReadWriteLock如何实现的读写锁](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
6. [二、阻塞队列高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [2.1 说下你熟悉的阻塞队列？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [2.2 虚假唤醒是什么？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
7. [三、线程池高频问题：（最重要的点）](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [3.1 线程池的7个参数（不会就回家等通知）](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [3.2 线程池的状态有什么，如何记录的？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [3.3 线程池常见的拒绝策略（不会就回家等通知）](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   4. [3.4 线程池执行流程（不会就回家等通知）](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   5. [3.5 线程池为什么添加空任务的非核心线程](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   6. [3.6 在没任务时，线程池中的工作线程在干嘛？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   7. [3.7 工作线程出现异常会导致什么问题？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   8. [3.8 工作线程继承AQS的目的是什么？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   9. [3.9 核心参数怎么设置？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
8. [一、CountDownLatch，Semaphore的高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   1. [1.1 CountDownLatch是啥？有啥用？底层咋实现的？（可以融入到你的项目业务中。）](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   2. [1.2 Semaphore是啥？有啥用？底层咋实现的？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
   3. [1.3 main线程结束，程序会停止嘛？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
9. [二、CopyOnWriteArrayList的高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    1. [2.1 CopyOnWriteArrayList是如何保证线程安全的？有什么缺点吗？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
10. [三、ConcurrentHashMap（JDK1.8）的高频问题：](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    1. [3.1 HashMap为啥线程不安全？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    2. [3.2 ConcurrentHashMap如何保证线程安全的？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    3. [3.3 ConcurrentHashMap构建好，数组就创建出来了吗？如果不是，如何保证初始化数组的线程安全？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    4. [3.4 为什么负载因子是0.75，为什么链表长度到8转为红黑树？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    5. [put操作太频繁的场景，会造成扩容时期put的阻塞 ？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    6. [3.5 ConcurrentHashMap何时扩容，扩容的流程是什么？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    7. [3.6 ConcurrentHashMap得计数器如何实现的？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
    8. [3.7 ConcurrentHashMap的读操作会阻塞嘛？](./multithreading-high-concurrency/%E5%B9%B6%E5%8F%91%E7%BC%96%E7%A8%8B%E9%9D%A2%E8%AF%95%E9%A2%98.md)
11. sdf

## JVM篇

1. [简述JMM](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
2. [对象的内存布局图](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
3. [运行时数据区](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
4. [对象的创建以及分配过程](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
5. [什么是双亲委派](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
6. [MinorGC与FullGC分别在什么时候发生](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
7. [怎么判断对象是否可以被回收](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
8. [说一下JVM有哪些垃圾回收算法](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
9. [十大垃圾回收器](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
10. [G1垃圾回收器的优势](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
11. [简述happen-before](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
12. [常用调优策略](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
13. [项目JVM调优](./jvm/JVM%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)

## Spring篇

1. [说下Spring的优势](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
2. [简述Spring的核心](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
3. [谈谈Spring IOC 的理解 ，原理与实现（未整理完成）](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
4. [Spring框架中单例bean是线程安全的吗？](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
5. [Spring框架中使用了哪些设计模式及应用场景](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
6. [简述Spring bean的声明周期](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
7. [Spring 是如何解决循环依赖问题的](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
8. [BeanFactory 和 FactoryBean的区别](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
9. [SpringAOP和AspectJAOP有什么区别](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
10. [jdk代理和cglib代理区别](./spring/Spring%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86.md)
11. [Spring事务原理](./spring/Spring%E4%BA%8B%E5%8A%A1.md)
12. [Spring事务的隔离级别](./spring/Spring%E4%BA%8B%E5%8A%A1.md)
13. [Spring事务失效的场景有哪些](./spring/Spring%E4%BA%8B%E5%8A%A1.md)

## SpringMVC篇

1. [MVC设计模式的好处](./spring/SpringMVC%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
2. [SpringMVC的请求响应流程](./spring/SpringMVC%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)

## SpringBoot篇

1. [Springboot自动装配原理](./spring/SpringBoot%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
2. [如何实现一个 Starter](./spring/SpringBoot%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)

## Redis篇

1. [Redis 的数据类型及使用场景](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
2. [使用 Redis 有哪些好处](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
3. [为什么 使用 Redis 而不是用 Memcache 呢](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
4. [为什么 Redis 单线程模型效率也能那么高](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
5. [为什么Redis中要使用I/O多路复用这种技术](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
6. [说说 Redis 的线程模型](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
7. [Redis6.0新特性之多线程模型](./redis/Redis6%E6%96%B0%E7%89%B9%E6%80%A7%E4%B9%8B%E5%A4%9A%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B.md)
8. [Redis持久化方式以及区别](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
9. [Redis如何选择持久化方式](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
10. [说说你对Redis事务的理解](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
11. [简述Redis集群模式](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
12. [RedisCluster集群原理](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
13. [RedisCluster集群方案什么情况下会导致整个集群不可用](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
14. [缓存和数据库谁先更新呢](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
15. [怎么提高缓存命中率](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
16. [如果有大量的key需要设置同一时间过期，一般需要注意什么](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
17. [缓存雪崩、缓存穿透、缓存预热、缓存更新、缓存降级等问题](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
18. [Redis的数据淘汰策略](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
19. [简述Redis热点Key及其解决方案](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
20. [哪些操作会阻塞redis](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
21. [Redis分布式锁使用场景](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
22. [Redis分布式锁的实现方案](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
23. [Redis如何保证不释放别人的锁](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
24. [Redlock](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)

## MySQL篇

### MySQL原理部分

1. [MySQL的基础架构](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
2. [MySQL的存储引擎](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
3. [MySQL存储引擎架构了解吗](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
4. [MyISAM 和 InnoDB 有什么区别](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
5. [简述数据库事务](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
6. [ACID靠什么保证](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
7. [数据库事务的并发执行会导致什么问题](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
8. [数据库事务的隔离级别](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
9. [MySQL 的默认隔离级别是什么](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
10. [MySQL 的隔离级别是基于锁实现的吗](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)
11. [MySQL中有哪几种锁，列举一下？](./mysql/%E5%8E%9F%E7%90%86%E9%83%A8%E5%88%86.md)

### MySQL索引部分

1. [什么是索引](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
2. [MySQL索引有哪些类型](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
3. [索引有哪些优缺点](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
4. [InnoDB中的索引结构](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
5. [B树和B+树有什么区别](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
6. [MySQL为什么选择B+树作为索引的数据结构](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
7. [聚簇索引与非聚簇索引的区别](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
8. [索引失效的场景有哪些](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
9. [回表](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
10. [MySQL的索引覆盖](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
11. [MySQL的索引下推（index condition pushdown，ICP）](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
12. [索引使用遵循什么原则](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
13. [Hash索引和B+树索引的区别](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)

### MySQL集群高可用

1. [MySQL主从复制原理](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)
2. [读写分离](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)
3. [分库分表原理](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)

### MySQL其他问题

1. [MySQL服务器CPU飙升问题](./mysql/%E6%8F%90%E9%AB%98%E9%83%A8%E5%88%86.md)
2. [如何进行慢查询优化](./mysql/%E6%8F%90%E9%AB%98%E9%83%A8%E5%88%86.md)

## MyBatis

1. [#{}和${}的区别](./mybatis/MyBatis%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
2. [MyBatis重要组件](./mybatis/MyBatis%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
3. [MyBatis层次结构](./mybatis/MyBatis%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
4. [MyBatis的执行原理](./mybatis/MyBatis%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)

## 消息中间件

### kafka

1. [kafka如何消息的不丢失](./kafka/KAFKA%E9%9D%A2%E8%AF%95%E9%A2%98.md)
2. [kafka如何保证顺序消费](./kafka/KAFKA%E9%9D%A2%E8%AF%95%E9%A2%98.md)
3. [kafka如何保证数据幂等性](./kafka/KAFKA%E9%9D%A2%E8%AF%95%E9%A2%98.md)

## 微服务组件

### 注册中心

1. [Eureka注册发现](./java-micro-service/Eureka%E9%9D%A2%E8%AF%95%E9%A2%98%E6%95%B4%E7%90%86.md)
2. [Eureka自我保护](./java-micro-service/Eureka%E9%9D%A2%E8%AF%95%E9%A2%98%E6%95%B4%E7%90%86.md)
3. [Eureka缺陷](./java-micro-service/Eureka%E9%9D%A2%E8%AF%95%E9%A2%98%E6%95%B4%E7%90%86.md)
4. [Eureka 的多级缓存机制](./java-micro-service/Eureka%E9%9D%A2%E8%AF%95%E9%A2%98%E6%95%B4%E7%90%86.md)
5. [nacos和eureka的异同]

### 服务降级

## 分布式系统解决方案

### 分布式锁

1. [分布式锁使用场景](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
2. [分布式锁的特点](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
3. [分布式锁的实现方式](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
4. [Redis分布式锁使用场景](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
5. [Redis分布式锁的实现方案](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
6. [Redis分布式锁的八大坑](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)

### 分布式事务

### 分布式架构

1. [如何拆分微服务](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E8%AE%BE%E8%AE%A1.md)
2. [微服务拆分策略有哪些](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E8%AE%BE%E8%AE%A1.md)
3. [怎样设计出高内聚、低耦合的微服务](./system-design/%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%E8%AE%BE%E8%AE%A1.md)
4. [如何理解SaaS系统](./system-design/%E8%BD%AF%E4%BB%B6%E5%8D%B3%E6%9C%8D%E5%8A%A1saas.md)

## 敏捷开发

1. [你的项目中是怎么保证微服务敏捷开发的](./%E6%95%8F%E6%8D%B7%E5%BC%80%E5%8F%91/%E6%95%8F%E6%8D%B7%E5%BC%80%E5%8F%91.md)
2. [微服务的链路追踪、持续集成、AB发布要怎么做](./%E6%95%8F%E6%8D%B7%E5%BC%80%E5%8F%91/%E6%95%8F%E6%8D%B7%E5%BC%80%E5%8F%91.md)

## 大数据

1. [请谈一下Hive的特点](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
2. [Hive的HSQL转换为MapReduce的过程](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
3. [hive内部表和外部表的区别](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
4. [Hive有索引吗](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
5. [所有的Hive任务都会有MapReduce的执行吗？](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
6. [数据建模用的哪些模型](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
7. [为什么要对数据仓库分层](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
8. [sortby和orderby的区别](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
9. [数据倾斜](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
10. [说说对Hive桶表的理解](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
