# netpoll（网络轮询器）

## 学习顺序（推荐）

1. [netpoll 使用指南](./01-usage-guide.md)（Go 网络 I/O 模型、观测方法）
2. [netpoll 实战案例](./02-practice-demo.md)（pprof/trace 分析网络瓶颈）
3. [netpoll 底层原理](./03-principle.md)（epoll/kqueue、多路复用、与 GMP 的关系）
4. [netpoll 面试总结](./04-interview-summary.md)（高频问答与表达模板）

## 核心作用

Go 网络 I/O 非阻塞调度核心：把 fd 上的可读/可写事件（epoll/kqueue/wepoll）转成 goroutine 的就绪与唤醒，减少线程阻塞，提高并发网络性能。

## 关注的问题

- 网络 I/O 卡顿时：goroutine 是 **阻塞在读写**、**等待 netpoll 唤醒**，还是 **在 syscall**？
- 高并发下：netpoll 事件分发是否成为瓶颈（连接数、活跃连接、事件风暴）

## 面试考点

- 网络 I/O 阻塞时 GMP 的行为（M 是否被绑死在 syscall，P 如何转移/新建 M）
- epoll/kqueue（Windows 侧 wepoll/IOCP 相关）的基本机制与差异
- 网络性能优化抓手：减少阻塞、减少分配、合理超时、连接池/复用、批量读写

## 待补充

- 与 `go tool trace`/`pprof` 结合：如何证明瓶颈在 netpoll（证据链）
- 典型故障：连接泄漏、超时设置不当、慢客户端导致写阻塞
