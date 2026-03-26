# sync.Pool 实战案例（pprof 验证 GC 优化）

## 目标

用可运行 demo 证明：引入 `sync.Pool` 后，分配与 GC 压力下降，并用 pprof 做前后对比。

## Demo 1：buffer 复用（典型场景）

- **问题**：每次请求都创建临时 `[]byte`/`bytes.Buffer`，导致 alloc 很高
- **方案**：用 `sync.Pool` 复用 buffer（注意 reset 与容量控制）
- **验证**：
  - alloc/heap profile：分配热点是否下降
  - 基准：`allocs/op`、`B/op` 是否下降
  - CPU：GC 相关开销是否下降（视情况）

## Demo 2：编解码临时对象复用

- **问题**：JSON/协议解析中产生大量短生命周期对象
- **方案**：复用临时结构体/缓存（小心残留引用）
- **验证**：同上 + 关注尾延迟（如果做压测）

## 常见踩坑（一定要在 demo 里复现一次）

- 忘记 reset：把上一次请求数据带到下一次（安全事故/内存泄漏）
- 把大对象放进 Pool：导致常驻内存高、抖动大
- Pool 当作连接池：语义不对（连接池要强生命周期管理）

## 标准输出模板

- 现象与指标（pprof/benchmark）
- 引入 Pool 的点与理由（为什么值得做）
- 前后对比（allocs/B、GC、P99）
- 结论与适用边界
