# go tool trace 底层原理（runtime 事件与可视化）

## 你需要理解的“最小原理集”

> 目标：能把 trace 里看到的现象，映射到 runtime 的真实行为。

- **事件从哪来**：runtime 在关键路径上记录事件（调度、GC、syscall、netpoll 等）
- **时间线是什么**：事件按时间排序，展示 G/M/P 的状态变化
- **为什么能解释调度问题**：trace 直接暴露“runnable/运行/阻塞/唤醒”的因果
- **为什么能解释 GC 问题**：GC 相关阶段与 STW 以事件形式呈现，可与业务抖动对齐

## 与 GMP/GC 的关联（面试表达要点）

- Goroutine 阻塞时：状态如何变化、何时唤醒、如何重新参与调度
- syscall 进入/退出：M 是否被绑住、P 如何转移保证其他 G 继续跑
- GC 触发条件（粗略）：堆增长/分配压力导致 GC 周期变化（需与 pprof alloc/heap 联动验证）

## 与 pprof 的差异（一定要会讲）

- pprof：统计视角（哪里贵），更像“热点分析仪”
- trace：事件视角（发生了什么/按时间怎么发生），更像“全流程行车记录仪”
- 实战建议：pprof 先定方向，trace 再把根因钉死

## TODO

- 补 runtime 关键概念索引（G/M/P、netpoll、STW、runnable）
- 为每个概念配 1 个可复现 demo + trace 证据
