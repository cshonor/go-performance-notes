# dlv（Delve）使用指南（全维度）

## 定位：它解决什么问题

dlv 是 Go 官方调试器，适合：

- 复现并定位死锁/卡住/疑难崩溃
- 验证“我猜是这里”的假设（断点 + 观察状态）
- 调试 runtime 路径（GMP、chan、锁相关）

## 常用启动方式

- `dlv debug ./...`：编译并启动调试
- `dlv test ./...`：调试测试
- `dlv exec ./bin -- <args>`：调试已有二进制
- `dlv attach <pid>`：附加到运行中进程（谨慎使用）

## 基础必会能力

- 下断点、单步、继续
- 查看/切换 goroutine、查看堆栈
- 查看变量、表达式求值

## 与 pprof/trace 的配合

- pprof/trace 给方向：热点/卡顿/阻塞类型
- dlv 负责“钉死根因”：在关键锁/chan/函数处断点验证状态与路径

## TODO

- 整理一份常用命令速查表（break/continue/next/step/print/goroutines/stack）
