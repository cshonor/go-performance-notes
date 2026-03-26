# go tool trace 实战案例（Demo 规划）

## 目标

用可复现 Demo 训练“问题 → 采集 → 视图证据 → 结论 → 优化 → 对比验证”的完整闭环。

## Demo 1：goroutine 阻塞排查（chan / select）

- **现象**：吞吐下降，goroutine 数上升，部分请求一直卡住
- **采集**：短窗口 trace
- **你要在 trace 里找到的证据**：
  - Goroutine timeline 显示大量 G 长时间处于阻塞态
  - 阻塞/唤醒点集中在某段 channel/select 路径
- **优化方向**：
  - 增加超时/取消（context）
  - 减少无界等待、修正 channel 使用方式

## Demo 2：GC STW 对延迟的影响

- **现象**：P99 抖动，CPU 看起来不高但延迟周期性尖刺
- **采集**：trace +（可选）pprof alloc/heap 辅助
- **证据**：
  - GC 视图出现 STW 片段与明显阶段耗时
  - 与延迟尖刺在时间轴上对齐
- **优化方向**：
  - 降低分配（逃逸分析）
  - 减少大对象、降低存活对象规模
  - 必要时调整负载/并发模型

## Demo 3：调度延迟定位（runnable 堆积）

- **现象**：goroutine runnable 很多但执行不起来，延迟高
- **证据**：
  - runnable 堆积、P 忙或被 syscall/GC 影响
  - Goroutine timeline 中大量等待调度的片段
- **优化方向**：
  - 降低阻塞点（锁/IO）
  - 调整并发粒度，减少 goroutine 风暴

## Demo 4：网络 I/O 阻塞排查（netpoll）

- **现象**：请求卡在读写，连接堆积
- **证据**：
  - Network 相关事件显示大量等待/唤醒异常
  - 与 goroutine 堆栈/阻塞点对齐（需要结合 pprof goroutine）
- **优化方向**：
  - 超时设置、连接复用、背压/限流
  - 识别慢客户端与写阻塞

## 标准输出（建议每个 demo 都写这 6 段）

- 现象与指标
- 采集方式与窗口
- 关键视图与证据截图
- 根因解释（结合 runtime 原理）
- 最小改动与优化点
- 前后对比验证（指标 + 证据）
