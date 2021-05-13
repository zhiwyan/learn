## raft共识算法

### 概念

Raft是实现分布式共识的一种算法，主要用来管理日志复制的一致性。

- raft的三种状态（角色）
  - follower（群众）
  - candidate（候选人）
  - leader（领导）

- 复制转态机
- 任期（term）
- 心跳（heartbeats）和超时机制（timeout）



### raft的三个子问题

- 选举（leader election）
- 日志复制（log replication）
- 安全性（safety）
  - 通过日志条目的索引值和候选人最后日志条目的任期号来决定是否能当选



[Raft算法](https://segmentfault.com/a/1190000022280801)

