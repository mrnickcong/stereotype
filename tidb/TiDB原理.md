# TiDB原理

- [TiDB原理](#tidb原理)
  - [TIDB](#tidb)



## TIDB

TiDB是一款来自中国的开源解决方案，它给出了一种兼容 MySQL 的 HTAP 数据库，支持强一致性，并且分布式可扩展。TiDB 实现为分层架构，其中 TiDB 服务器作为无状态计算层出于顶层。底层存储层实现为支持事务的键值数据库，称为 TiKV。TiKV 的设计受到了 Google Spanner 的启发