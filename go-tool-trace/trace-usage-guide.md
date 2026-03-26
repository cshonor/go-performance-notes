# go tool trace 使用指南（全维度）

## 定位：它解决什么问题

`go tool trace` 用于**追踪 Go runtime 的全链路事件**，把调度、GC、系统调用、网络 I/O 等行为可视化，回答这类问题：

- goroutine 为什么“卡住/跑不起来”（阻塞点/唤醒点在哪里）
- 调度为什么慢（runnable 堆积、P 忙、syscall 影响等）
- GC 对业务延迟的影响（STW、各阶段耗时与频率）
- 网络 I/O 为什么慢（netpoll 等待、读写阻塞、连接抖动）

## 采集方式

### 1) 测试/基准采集（最常用）

- `go test -run TestX -trace trace.out ./...`

### 2) 程序内采集（可控、适合 demo）

- `runtime/trace`：`trace.Start(w)` / `trace.Stop()`
- 建议只在**短时间窗口**采集（避免文件过大、干扰过强）

## 分析入口

- `go tool trace trace.out`（启动 Web UI）

## 必看视图（学习顺序建议）

> 目标：能把“现象”映射到“视图证据”。

- **Goroutines / Timeline**：G 的运行/阻塞/唤醒与调度延迟
- **GC 视图**：STW 时长、GC 周期与阶段耗时
- **Syscalls**：syscall 对 M/P 绑定与调度的影响
- **Network**：netpoll 相关等待与唤醒

## 与 pprof 的配合（标准动作）

- pprof 负责回答：**哪里贵/哪里慢**
- trace 负责回答：**为什么贵/卡在 runtime 的哪个环节**

建议流程：
1. 先用 pprof（CPU/heap/alloc/mutex/block）形成初步假设
2. 再用 trace 验证调度/GC/IO 证据，缩小根因范围

## 常见坑

- trace 太大：采集窗口过长或事件太密，导致分析困难
- 把 trace 当 pprof：trace 更偏“事件时间线”，不是热点统计
- 只看一张图下结论：要结合指标/pprof/代码路径形成闭环

## TODO

- 给出一套“采集窗口推荐”（线上/压测/本地）
- 每个核心视图各补 1 个“怎么读”的示例截图与说明
