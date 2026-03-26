# pprof 全维度使用指南

## 为什么第一个学

- `pprof` 是 Go 性能问题排查的**入口工具**（面试高频 + 日常刚需）
- 后续工具（trace / 逃逸分析 / sync.Pool / netpoll / dlv）都离不开 pprof 建立的“证据链思维”

## 6 大采样类型（必须掌握）

> 目标：知道“什么时候采什么”、“采到什么能说明什么”。

- **CPU**：定位热点函数/路径（耗时在哪）
- **Heap**：存活对象与占用（内存为什么涨）
- **Allocation**：分配来源（哪里在疯狂分配，GC 为什么频繁）
- **Goroutine**：协程堆栈与状态（泄漏/卡住/死锁）
- **Mutex**：锁竞争（谁在抢锁，争用有多严重）
- **Block**：阻塞事件（chan、select、sync 等阻塞）

## 采集方式（常用 2 条路）

### 1) `net/http/pprof`（在线服务最常用）

- 在服务里开启 pprof 端点（通常监听独立端口）
- 通过 HTTP 拉取 profile 文件进行分析

### 2) `runtime/pprof`（离线程序 / demo / 工具）

- 在程序里手动 start/stop 写入 profile 文件
- 适合做最小复现 demo、基准测试对比

## 分析方法（建议形成固定手法）

> 推荐顺序：**top → list → source/graph → web（可视化）→ 对比**。

- `go tool pprof` 常用视图：
  - `top`：热点总览（按 flat/cum 排序）
  - `list <func>`：定位到代码行级（看哪几行最贵）
  - `web`：图谱（调用链关系）
  - `-http=:0`：启动本地 UI（含火焰图入口）

## 最小闭环工作流（强烈建议背熟）

1. **稳定复现**：同一负载（benchmark / test / 回放流量）
2. **采集**：选对 profile（CPU/Heap/Alloc/Mutex/Block/Goroutine）
3. **分析**：用 `pprof` 找到“证据”
4. **提出假设**：为什么慢/为什么涨/为什么卡
5. **修改**：最小改动
6. **验证**：同一负载再次采集，对比前后数据

## 常见误区（先避坑）

- 只看一次 profile 下结论：没有对比就没有优化
- CPU profile 和 Heap profile 混用解释：二者含义不同
- 只盯着“top1 函数”：要看调用链、看是否是症状而非根因
- 忽略运行环境差异：负载、GOMAXPROCS、编译参数都会影响结论

## TODO（下一步可直接补充）

- 每类 profile 的最小 demo（可复现 + 可验证优化）
- 常见问题的证据链模板（现象 → 指标 → profile → 结论 → 改动）
