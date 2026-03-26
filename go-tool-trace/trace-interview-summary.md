# go tool trace 面试考点总结

## 1) trace 能解决什么问题

- goroutine 阻塞与唤醒链路（卡在哪、为什么卡）
- 调度延迟（runnable 堆积、P 忙、syscall 影响）
- GC STW 对业务延迟的影响（阶段耗时、频率）
- 网络 I/O 等待（netpoll 相关问题）

## 2) trace 与 pprof 的核心区别（高频必问）

- **pprof**：热点统计（CPU/内存/分配/阻塞/竞争）
- **trace**：时间线事件（调度/GC/syscall/net I/O 的全过程）

回答模板：
- pprof 用来快速定位“哪个调用链最贵”
- trace 用来解释“为什么贵/卡在 runtime 的哪个阶段”
- 最终要形成证据链并做前后对比验证

## 3) 如何用 trace 排查 goroutine 阻塞（流程）

1. 明确现象与指标（吞吐、P99、goroutines、GC）
2. 采集短窗口 trace（尽量与问题窗口对齐）
3. 在 timeline 中定位：
   - 阻塞类型（chan/mutex/syscall/netpoll/GC）
   - 阻塞持续时间与是否集中在某路径
4. 回到代码与 pprof goroutine/mutex/block 交叉验证
5. 给出根因与最小改动，复测对比

## 4) 常见追问

- trace 采集对性能影响大不大？如何控制影响？
- GC STW 看到很长怎么办？第一步该怎么验证？
- syscall/网络 I/O 导致卡顿时，GMP 会发生什么？
