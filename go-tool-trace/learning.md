# go-tool-trace 学习路径（第二优先级）

## 为什么第二个学

- `trace` 是 `pprof` 的**互补工具**：pprof 看瓶颈（“哪里贵”），trace 看 runtime 全流程事件（“为什么贵/卡在哪个阶段”）
- 能把 pprof 无法解释的问题可视化，例如：**协程阻塞、调度延迟、GC STW、网络 I/O 等待、syscall 影响**

## 学习路径（闭环）

### 1) 基础使用

- 采集：
  - 在程序中 `trace.Start()` / `trace.Stop()`（`runtime/trace`）
  - 或 `go test -trace trace.out`
- 分析：
  - `go tool trace trace.out`（浏览器 UI）

### 2) 核心视图解读（你必须看懂）

- **Goroutine 时间线**：调度、运行、阻塞、唤醒的全流程
- **GC 视图**：STW 时长、GC 阶段耗时与频率
- **网络 I/O**：netpoll 相关等待与唤醒
- **系统调用**：syscall 对 M/P 的影响与调度变化

### 3) 实战结合（与 pprof 联动）

- 先用 pprof 找到“CPU 高/延迟高/吞吐低”的现象与热点
- 再用 trace 验证：
  - CPU 高耗时到底是业务代码还是 GC/调度导致
  - 请求慢是计算慢还是“等待多”（锁/chan/netpoll/syscall）

## 面试考点（高频）

- trace 与 pprof 的核心区别、各自适用场景
- 如何用 trace 分析 goroutine 阻塞与调度延迟（证据链怎么讲）
- GC STW 对业务（尤其延迟型业务）的影响与缓解思路

## TODO（建议你补充）

- 3 个最小 demo：chan 阻塞 / syscall 阻塞 / net I/O 等待
- 每个 demo 输出：trace 关键截图 + 解读（现象→证据→结论）
