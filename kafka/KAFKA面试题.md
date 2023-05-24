# kafka总结

- [kafka总结](#kafka总结)
  - [kafka如何消息的不丢失](#kafka如何消息的不丢失)
  - [kafka如何保证顺序消费](#kafka如何保证顺序消费)
    - [方法一：一个Topic划分一个partition](#方法一一个topic划分一个partition)
    - [方法二：一个Topic划分多个partition](#方法二一个topic划分多个partition)
  - [kafka如何保证数据幂等性](#kafka如何保证数据幂等性)

## kafka如何消息的不丢失

## kafka如何保证顺序消费

Kafka的有序性包括全局有序和局部有序
全局有序

### 方法一：一个Topic划分一个partition

- 全局使用一个生产者
- 全局使用一个分区（可以使用不同分区和Topic实现隔离与扩展）
- 全局使用一个消费者（严格到一个线程去消费）
  
这种方法效率低下

### 方法二：一个Topic划分多个partition

- 采用key Hash的方法。以某个message数据特征（订单ID）作为key，再取Hash，那么订单相同的就会分到同一个分区
- 同一个分区只有一个消费者线程去消费，那么一定是有序的
- 同一个分区有多个线程消费者情况下，加入N个内存queue，相同key的数据存储在同一个queue中，每个消费者消费一个内存队列的数据

## kafka如何保证数据幂等性
