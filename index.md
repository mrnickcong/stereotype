# 面试题索引

- [面试题索引](#面试题索引)
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
  - [微服务组件](#微服务组件)
    - [注册中心](#注册中心)
    - [服务降级](#服务降级)
  - [分布式系统解决方案](#分布式系统解决方案)
    - [分布式锁](#分布式锁)
    - [分布式事务](#分布式事务)
    - [分布式架构](#分布式架构)
  - [敏捷开发](#敏捷开发)
  - [大数据](#大数据)

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

## JVM篇

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
5. [说说 Redis 的线程模型](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
6. [Redis持久化方式以及区别](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
7. [Redis如何选择持久化方式](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
8. [说说你对Redis事务的理解](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
9. [简述Redis集群模式](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
10. [RedisCluster集群原理](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
11. [RedisCluster集群方案什么情况下会导致整个集群不可用](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
12. [缓存和数据库谁先更新呢](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
13. [怎么提高缓存命中率](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
14. [如果有大量的key需要设置同一时间过期，一般需要注意什么](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
15. [缓存雪崩、缓存穿透、缓存预热、缓存更新、缓存降级等问题](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
16. [Redis的数据淘汰策略](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
17. [简述Redis热点Key及其解决方案](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
18. [哪些操作会阻塞redis](./redis/Redis%E9%97%AE%E9%A2%98%E6%95%B4%E7%90%86.md)
19. [Redis分布式锁使用场景](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
20. [Redis分布式锁的实现方案](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
21. [Redis如何保证不释放别人的锁](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)
22. [Redlock](./redis/Redis%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81.md)

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
5. [MySQL为什么选择B+树作为索引的数据结构](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
6. [聚簇索引与非聚簇索引的区别](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
7. [索引失效的场景有哪些](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
8. [回表](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
9. [MySQL的索引覆盖](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
10. [MySQL的索引下推（index condition pushdown，ICP）](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
11. [索引使用遵循什么原则](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)
12. [Hash索引和B+树索引的区别](./mysql/%E7%B4%A2%E5%BC%95%E9%83%A8%E5%88%86.md)

### MySQL集群高可用

1. [MySQL主从复制原理](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)
2. [读写分离](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)
3. [分库分表原理](./mysql/MySQL%E9%9B%86%E7%BE%A4%E9%AB%98%E5%8F%AF%E7%94%A8.md)

### MySQL其他问题

1. [MySQL服务器CPU飙升问题]

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

1. [分布式锁使用场景]
2. [分布式锁的特点]
3. [分布式锁的实现方式]
4. []
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

1. [hive内部表和外部表的区别](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
2. [Hive有索引吗](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
3. [所有的Hive任务都会有MapReduce的执行吗？](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
4. [数据建模用的哪些模型](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
5. [为什么要对数据仓库分层](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
6. [sortby和orderby的区别](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
7. [数据倾斜](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
8. [说说对Hive桶表的理解](./big-data/Hive%E7%9F%A5%E8%AF%86%E7%82%B9%E6%80%BB%E7%BB%93.md)
