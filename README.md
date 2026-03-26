# Go performance & runtime notes

个人笔记：Go 性能与运行时相关工具、原理与面试要点。

## 工具 / 技术对照

| 工具 / 技术 | 核心作用 | 面试考点 |
| --- | --- | --- |
| [**go tool trace**](./go-tool-trace/) | 追踪 Go runtime 全流程事件（调度、GC、系统调用、网络 I/O） | 1. 如何用 trace 分析 Goroutine 阻塞、调度延迟；2. GC STW 对业务的影响；3. 网络 I/O 阻塞排查 |
| [**delve（dlv）**](./dlv/) | Go 官方调试器 | 1. 结合 pprof 排查复杂内存泄漏、死锁；2. 断点调试 GMP 调度、chan 阻塞流程；3. 线上问题复现与定位 |
| [**netpoll 网络轮询器**](./netpoll/) | Go 网络 I/O 非阻塞调度核心 | 1. 网络 I/O 阻塞时 GMP 的行为；2. epoll/kqueue 底层实现；3. 网络性能优化 |
| [**逃逸分析**（`go build -gcflags="-m"`）](./escape-analysis/) | 分析变量分配在栈 / 堆，优化内存分配 | 1. 逃逸分析规则；2. 如何减少堆分配、降低 GC 压力；3. 面试常问：什么情况变量会逃逸 |
| [**sync.Pool** 复用机制](./sync-pool/) | 减少内存分配、降低 GC 压力，复用临时对象 | 1. Pool 底层实现；2. 与 GMP 调度的结合；3. 实战优化场景（学习重点） |

## 目录结构

- `go-tool-trace/`: trace 相关笔记与案例
- `dlv/`: dlv 调试相关笔记与复现脚本
- `netpoll/`: netpoll / epoll / kqueue 相关笔记
- `escape-analysis/`: 逃逸分析规则与优化记录
- `sync-pool/`: `sync.Pool` 原理与实战优化场景
