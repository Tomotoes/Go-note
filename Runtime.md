# Runtime

关于runtime包几个函数:

- `Gosched` 让出cpu
- `NumCPU` 返回当前系统的CPU核数量
- `GOMAXPROCS` 设置最大的可同时使用的CPU核数
- `Goexit` 退出当前goroutine(但是defer语句会照常执行)