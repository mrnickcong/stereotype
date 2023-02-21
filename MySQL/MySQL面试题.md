## 问题：1000万的数据，B+树的树高大致是多少？
```
```
## 问题：MVVC模式
## 问题：数据的一致性
## 问题：简述数据库事务
数据库事务（Transaction）是由若干个SQL语句构成的一个操作序列，有点类似于Java的synchronized同步。数据库系统保证在一个事务中的所有SQL要么全部执行成功，要么全部不执行，即数据库事务具有ACID特性：
* Atomicity：原子性
* Consistency：一致性
* Isolation：隔离性
* Durability：持久性
## 问题：数据库事务的隔离级别
  
  数据库事务是由数据库系统保证的
  
  数据库事务可以并发执行，而数据库系统从效率考虑，对事务定义了不同的隔离级别。
  
  SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：
| Isolation Level  | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| :--------------: | :----------------: | :-------------------------------: | :------------------: |
| Read Uncommitted |        Yes         |                Yes                |         Yes          |
|  Read Committed  |         -          |                Yes                |         Yes          |
| Repeatable Read  |         -          |                 -                 |         Yes          |
|   Serializable   |         -          |                 -                 |          -           |