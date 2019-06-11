# Select



select默认是阻塞的，只有当监听的channel中有发送或接收可以进行时才会运行，当多个channel都准备好的时候，select是随机的选择一个执行的。

select 中的case 可以使用断言

```
case e, ok := <-ch1
```



select

- 可处理一个或多个channel的发送与接收
- 同时可有多个可用的channel按随机顺序处理
- **可用空的select来阻塞main函数**
- 可设置超时(time.After)
- case 后必须是 channel的动作(发送或者接收)
- case后面可以为空 `case:`

