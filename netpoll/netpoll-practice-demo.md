# netpoll 实战案例（pprof + trace 分析网络瓶颈）

## 目标

用可复现 demo 训练网络服务的性能排查：定位是“业务慢”、还是“等待多”（I/O/锁/GC/调度）。

## Demo 1：慢客户端导致写阻塞（连接堆积）

- **现象**：连接数上涨、写耗时增加、goroutine 增多，吞吐下降
- **采集**：
  - pprof goroutine（看是否卡在网络写/等待）
  - pprof block/mutex（排除竞争型瓶颈）
  - trace Network/Syscall（观察等待与唤醒）
- **你要得到的证据链**：
  - goroutine 堆栈集中在写路径/等待路径
  - trace 时间线上等待时间与卡顿窗口对齐
- **优化方向**：
  - 写超时/读超时、背压/限流
  - 输出缓冲与批量写
  - 识别并隔离慢连接

## Demo 2：超时设置不当导致 goroutine 堆积

- **现象**：请求慢且不释放资源，goroutine 数持续升高
- **证据**：大量 goroutine 长时间等待 I/O 或上游响应
- **优化方向**：合理超时、取消链路（context）、重试策略与熔断

## 标准输出模板

- 现象与指标（QPS/P99/连接数/goroutines）
- 采集方案（为什么选这些 profile/视图）
- 证据（pprof + trace）
- 根因与优化点
- 前后对比验证
