# MySQL主从复制原理

[文章来源](https://ost.51cto.com/posts/11721)

MySQL 主从复制涉及到三个线程：

一个在主节点的线程：log dump thread

从库会生成两个线程：一个 I/O 线程，一个 SQL 线程


主库会生成一个 log dump 线程,用来给从库 I/O 线程传 Binlog 数据。

从库的 I/O 线程会去请求主库的 Binlog，并将得到的 Binlog 写到本地的 relay log (中继日志)文件中。

SQL 线程,会读取 relay log 文件中的日志，并解析成 SQL 语句逐一执行。