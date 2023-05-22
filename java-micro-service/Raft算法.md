# Raft算法

- [Raft算法](#raft算法)
  - [什么是raft算法](#什么是raft算法)

## 什么是raft算法

终于到了 raft 协议，raft 的论文开篇就是这么一段话：

```text
Raft is a consensus algorithm for managing a replicated log. 
It produces a result equivalent to (multi-)Paxos, and it is as efficient as Paxos, but its structure is different from Paxos;
```

raft 证明和 paxos 等价，raft 是一种日志复制的一致性算法。

看懂了吗？raft 的着眼点就是 日志+状态机 的方向。