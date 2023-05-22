# MySQL日志

- [MySQL日志](#mysql日志)
  - [MySQL日志分离](#mysql日志分离)

## MySQL日志分离

MySQL中有以下日志文件，分别是：

1：重做日志（redo log）
2：回滚日志（undo log）
3：二进制日志（binlog）
4：错误日志（errorlog）
5：慢查询日志（slow query log）
6：一般查询日志（general log）
7：中继日志（relay log）

undolog redolog binlog

- undo log（回滚日志）：是 Innodb 存储引擎层生成的日志，实现了事务中的原子性，主要用于事务回滚和 MVCC。
- redo log（重做日志）：是 Innodb 存储引擎层生成的日志，实现了事务中的持久性，主要用于掉电等故障恢复；
- binlog （归档日志）：是 Server 层生成的日志，主要用于数据备份和主从复制
