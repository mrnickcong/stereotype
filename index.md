# 面试题索引

- [面试题索引](#面试题索引)
  - [基础篇](#基础篇)
    - [Java语言基础](#java语言基础)
    - [Java集合](#java集合)
  - [多线程和高并发](#多线程和高并发)
  - [JVM篇](#jvm篇)
  - [Spring篇](#spring篇)
  - [redis篇](#redis篇)
  - [MySQL篇](#mysql篇)
    - [MySQL原理部分](#mysql原理部分)
    - [MySQL索引部分](#mysql索引部分)
    - [MySQL集群高可用](#mysql集群高可用)
  - [微服务组件](#微服务组件)
    - [注册中心](#注册中心)
    - [服务降级](#服务降级)
  - [分布式系统解决方案](#分布式系统解决方案)
    - [分布式锁](#分布式锁)
    - [分布式事务](#分布式事务)
    - [分布式架构](#分布式架构)

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
10. gf

### Java集合

1. [简述Java的集合](./java/Java%E9%9B%86%E5%90%88.md)
2. [说说fail-fast与fail-safe机制](./java/Java%E9%9B%86%E5%90%88.md)
3. [HashMap 与 ConcurrentHashMap 的异同](./java/Java%E9%9B%86%E5%90%88.md)
4. [HashMap](./java/HashMap.md)
5. [ConcurrentHashMap](./java/ConcurrentHashMap.md)

## 多线程和高并发

## JVM篇

## Spring篇

## redis篇

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
2. [读写分离]



## 微服务组件

### 注册中心

### 服务降级

## 分布式系统解决方案

### 分布式锁

### 分布式事务

### 分布式架构

1. [你是如何拆分微服务的]