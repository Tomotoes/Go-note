# Panic && Recover

panic

- 停止当前程序运行
- 一直向上返回，执行每一层的 defer
- 如果没有遇见 recover，程序退出

recover

- 仅在 defer 调用中使用
- 获取 panic 的值
- 如果无法处理，可重新 panic