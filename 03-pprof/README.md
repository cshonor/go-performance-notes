# pprof

## 为什么先学 pprof

`pprof` 是 Go 性能排查的 **默认入口工具**：
- 定位 CPU 热点
- 理解内存增长与分配压力（从而推断 GC 压力）
- 查看 goroutine 状态（泄漏 / 死锁 / I/O 卡住）
- 度量锁竞争（mutex）与阻塞（block）
- 为优化提供可量化的前后对比证据

## 需要掌握的 profiles

- **CPU**：时间花在哪里
- **Heap**：当前存活对象（常用于判断“内存为什么一直涨”）
- **Allocations**：分配发生在哪里（用于减少堆分配、降低 GC 压力）
- **Goroutine**：goroutine 堆栈与状态
- **Mutex**：锁竞争
- **Block**：阻塞事件（channel、sync、select 等）

## 最小工作流（建议养成习惯）

1. **复现**稳定负载（benchmark / test / 压测回放）
2. **采集**合适的 profile
3. 在 `go tool pprof` **分析**（top / list / web / flame graph）
4. 同一负载下做 **假设 → 修改 → 验证**（对比指标与图谱）

## 笔记与 TODO

- 增加可运行 demos：
  - goroutine 泄漏
  - 锁竞争
  - 内存泄漏 / 分配过多导致的 GC 压力
- 补充火焰图生成与使用（Go 内置 `pprof` 的 `-http` 工作流）
- 面试 Q&A 总结
