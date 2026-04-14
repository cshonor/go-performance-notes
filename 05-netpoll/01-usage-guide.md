# netpoll 使用指南（全维度）

## 定位：它解决什么问题

netpoll 是 Go 网络 I/O 非阻塞模型的核心之一。它不是一个“你每天直接调用的工具”，但你需要能：

- 理解 Go 网络服务为何能高并发（I/O 等待不阻塞整体调度）
- 排查网络卡顿/连接堆积时，goroutine/调度/IO 等待发生在哪里
- 结合 pprof + trace 建立网络瓶颈的证据链

## 你需要掌握的基础概念

- I/O 多路复用：epoll/kqueue（概念层即可）
- fd 就绪事件：可读/可写/错误
- 就绪事件到 goroutine 唤醒：从“等待”到“可运行”的转换

## 实战中怎么用（不是直接用 netpoll，而是用工具链观测它）

- **pprof**：
  - CPU：热点是否在编解码/日志/业务计算
  - goroutine：是否大量卡在读写/等待路径
  - mutex/block：是否被锁/chan 阻塞拖慢
- **trace**：
  - Network/Syscall：I/O 等待与唤醒、syscall 影响

## TODO

- 补 2 个典型案例模板：慢客户端写阻塞、超时设置不当导致堆积
