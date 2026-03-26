# netpoll 底层原理（epoll/kqueue 与 GMP 调度）

## 你需要掌握的“最小原理集”

- I/O 多路复用：一个线程等待多个 fd 的就绪事件（概念层）
- netpoll 的职责：监听就绪事件，并把“等待 I/O 的 goroutine”变回 runnable
- 关键结论：网络 I/O 等待不应该把整个调度停住（要理解它如何与 GMP 协作）

## 网络 I/O 阻塞时 GMP 的行为（面试高频）

> 目标：能讲清楚“谁阻塞、谁继续跑”。

- 当 goroutine 进入网络 I/O 等待：
  - goroutine 进入等待态
  - runtime 通过 netpoll 机制在事件就绪时唤醒它
- 事件就绪后：
  - goroutine 重新变为 runnable
  - 等待被调度继续执行

