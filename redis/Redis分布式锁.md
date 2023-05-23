# Redis分布式锁

- [Redis分布式锁](#redis分布式锁)
  - [Redis分布式锁使用场景](#redis分布式锁使用场景)
  - [Redis分布式锁的实现方案](#redis分布式锁的实现方案)
  - [Redis如何保证不释放别人的锁](#redis如何保证不释放别人的锁)
  - [Redlock](#redlock)

## Redis分布式锁使用场景

## Redis分布式锁的实现方案

redis的分布式锁采用setnx命令就可以，非常简单。因此很多大型项目选择redis实现分布式锁。基于这个特性，我们就可以用setnx实现加锁的目的：通过setnx加锁，加锁之后其他服务无法加锁，用完之后，再通过delete解锁，深藏功与名。

```java
setnx key value
//并且可以设置过期时间：
set key value nx ex seconds
```

## Redis如何保证不释放别人的锁

## Redlock
