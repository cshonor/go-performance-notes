# delve（dlv）

## 核心作用

Go 官方调试器：断点、单步、变量查看、goroutine/线程切换、core dump 调试，适合复现线上问题与深入 runtime 行为。

## 常用入口

- `dlv debug ./cmd/xxx`（编译并启动调试）
- `dlv test ./...`（调试测试）
- `dlv exec ./bin -- <args>`（调试已有二进制）
- 远程/容器常见：`dlv --listen=:2345 --headless --api-version=2 exec ./bin`

## 配合 pprof / trace 的思路

- **pprof 找热点/泄漏趋势** → **dlv 精确复现与断点验证**
- 死锁/卡住：先用 `pprof` 的 goroutine dump 看堆栈，再用 dlv 复现并在关键锁/chan/条件变量处打断点

## 面试考点

- 如何用 dlv 复现并定位死锁/竞态/卡顿（配合 goroutine 栈、线程与断点）
- 如何观察/切换 goroutine，识别阻塞点（chan、mutex、syscall）
- 线上问题定位流程（最小化复现、版本一致性、符号与构建参数）

## 待补充

- 常用 dlv 命令速查（break/trace/continue/next/step/print/goroutines/stack）
- 典型案例：chan 阻塞、mutex 争用、条件变量等待
