# netpoll 面试考点总结

## 1) Go 网络 I/O 模型是什么

- 基于 I/O 多路复用（平台相关：epoll/kqueue 等概念）
- 通过 netpoll 将 I/O 等待与 goroutine 调度结合，实现高并发

## 2) netpoll 与 GMP 的关系（高频）

- 网络 I/O 等待不会让整个调度停摆
- goroutine 等待 I/O 时进入等待态，事件就绪后被唤醒并重新参与调度

## 3) epoll/kqueue 需要掌握到什么程度

- 概念：fd、就绪事件、读写事件、事件循环
- 能解释“为什么比每连接一个线程更高效”（减少线程阻塞与上下文切换）

## 4) 排查网络瓶颈的证据链（必须会讲）

- pprof：CPU/alloc/ goroutine / mutex / block
- trace：Network/Syscall 时间线
- 指标：连接数、超时、重试、队列长度、P99
