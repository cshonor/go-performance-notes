# go tool trace

## 核心作用

追踪 Go runtime 全流程事件：调度（G/M/P）、GC、系统调用、网络 I/O、用户任务等，用于定位“慢在 runtime 里哪里”。

## 常用入口

- 采集 trace：
  - `go test -run TestX -trace trace.out ./...`
  - `go test -c` 后运行二进制，或在程序里 `runtime/trace` 手动 start/stop
- 分析：
  - `go tool trace trace.out`（浏览器 UI）

## 看什么（排查思路）

- **Goroutine 阻塞**：是否大量阻塞在 `chan`/`select`/`sync`/`syscall`/`netpoll`
- **调度延迟**：Runnable 多、但运行时间片不足（P 忙/GC/系统调用导致）
- **GC 影响**：STW 片段、GC 周期频率（是否因堆分配过多触发频繁 GC）
- **网络 I/O**：是否卡在 netpoll / 读写超时 / 连接抖动导致 goroutine 堆积

## 面试考点

- 如何用 trace 定位 goroutine 卡住/调度慢的根因
- GC STW 对延迟型业务的影响如何评估与缓解
- 网络 I/O 阻塞的排查路径（trace + pprof + 指标）

## 待补充

- 最小可复现 demo（chan 阻塞 / syscall 阻塞 / netpoll 阻塞）
- 典型截图与解读要点（问题 → 证据 → 结论）
