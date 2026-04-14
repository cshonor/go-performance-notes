# go-performance-notes
Go 性能调试工具链学习与实战（pprof、火焰图、trace 等）

## 📖 关于
这是一个 **专注 Go 性能调试工具链** 的学习与沉淀仓库，内容包括：
- Go 官方调试/分析工具的系统用法
- 可复现的真实问题排查案例（问题 → 证据 → 定位 → 优化）
- 大厂高频面试考点整理
- 可运行的 demo 代码便于练习

## 📁 仓库结构

```text
.
├── 01-escape-analysis/   # 逃逸分析：栈/堆分配分析与内存优化
├── 02-sync-pool/         # sync.Pool：对象复用与 GC 优化
├── 03-pprof/             # pprof：核心性能分析工具（含火焰图）
├── 04-go-tool-trace/     # go tool trace：runtime 全链路追踪
├── 05-netpoll/           # netpoll：网络 I/O 非阻塞轮询器
└── 06-dlv/               # dlv（Delve）：Go 官方调试器
└── README.md
```

## 🧭 推荐学习顺序（从基础到进阶）

> 学习逻辑：先打牢内存基础 → 再用工具建立“证据链”能力 → 最后深入 runtime 网络底层并用调试器验证。

1. [escape-analysis](./01-escape-analysis/)（逃逸分析）
2. [sync-pool](./02-sync-pool/)（对象复用）
3. [pprof](./03-pprof/)（性能分析核心工具，含火焰图）
4. [go-tool-trace](./04-go-tool-trace/)（runtime 全链路追踪）
5. [netpoll](./05-netpoll/)（网络 I/O 轮询器/底层原理）
6. [dlv](./06-dlv/)（Delve 调试器：验证与深挖）

### 为什么这样排

- **先原理后工具**：逃逸分析 + `sync.Pool` 先把内存/GC 优化的底层逻辑打牢
- **工具落地实战**：pprof + trace 建立“问题 → 采集 → 证据 → 定位 → 优化”的排查闭环
- **最后深挖架构**：netpoll + dlv 进入 runtime 网络底层，并用调试器验证理解

## 🔤 仅用于 GitHub 目录浏览的字母排序

> 仅用于“看起来整齐”，不代表推荐学习顺序。

- `01-escape-analysis/`
- `02-sync-pool/`
- `03-pprof/`
- `04-go-tool-trace/`
- `05-netpoll/`
- `06-dlv/`

## 🎯 工具清单与学习重点

### 1. [pprof](./03-pprof/)（含火焰图）
- 6 大核心采样：CPU / Heap / Allocation / Goroutine / Mutex / Block
- 火焰图生成与解读（看懂 CPU 耗时、阻塞、GC、锁竞争等信号）
- 实战排查：goroutine 泄漏、锁竞争、内存泄漏/分配过多
- 采样与数据采集的基本原理 + 面试考点

### 2. [go-tool-trace](./04-go-tool-trace/)
- 追踪 runtime 事件（调度、GC、系统调用、网络 I/O）
- 分析 goroutine 阻塞与调度延迟
- 可视化 GMP 调度与 GC STW 影响
- 与 pprof 互补：pprof 找“热点”，trace 看“全流程”

### 3. [escape-analysis](./01-escape-analysis/)
- 使用 `go build -gcflags="-m"` 分析变量栈/堆分配
- 常见逃逸场景与优化思路
- 减少堆分配、降低 GC 压力
- 逃逸规则面试高频总结

### 4. [sync-pool](./02-sync-pool/)
- 复用临时对象，减少分配与 GC 压力
- 底层实现与与 P 绑定的机制
- 高并发服务的典型优化实践
- 用 pprof 验证优化效果（前后对比）

### 5. [netpoll](./05-netpoll/)
- 基于 epoll/kqueue 的 Go 网络 I/O 非阻塞模型
- 网络 I/O 阻塞时 GMP 的调度行为
- 高并发网络服务的性能优化抓手
- 结合 pprof + trace 分析网络瓶颈

### 6. [dlv](./06-dlv/)（Delve）
- 复杂问题的“最终手段”调试器（断点/单步/查看 goroutine）
- 调试 runtime 源码、死锁、复杂内存问题
- 线上问题复现与定位（在安全前提下）
- 补足 pprof/trace 仍无法精准定位的场景

## 📌 适合谁
- Go 后端开发者
- 性能优化方向工程师
- 大厂面试准备（底层 + 调优 + 证据链）
- 想系统掌握 Go 调试工具链的学习者

## ⭐ Star & Fork
欢迎 star / fork，一起把这个仓库沉淀成更专业的 Go 调试工具学习资料库。
