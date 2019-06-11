# Channel

**channel通信控制基于CSP模型，相比于传统的线程与锁并发模型，避免了大量的加锁解锁的性能消耗，而又比Actor模型更加灵活，使用Actor模型时，负责通讯的媒介与执行单元是紧耦合的–每个Actor都有一个信箱。**

**而使用CSP模型，channel是第一对象，可以被独立地创建，写入和读出数据，更容易进行扩展。**



```go
// 多通道传输数据
ch1 <- <-ch2
```



range 一个channel时, 此channel 一定要close , 不然就会死锁,因为 goruntine会一直等待数据传出



事实上，channel 内部就是一个带锁的队列。

channel 的类型不能进行隐式转换



channel存在`3种状态`：

1. nil，未初始化的状态，只进行了声明，或者手动赋值为`nil`
2. active，正常的channel，可读或者可写
3. closed，已关闭，**千万不要误认为关闭channel后，channel的值是nil**

channel可进行`3种操作`：

1. 读
2. 写
3. 关闭

把这3种操作和3种channel状态可以组合出`9种情况`：

| 操作      | nil的channel | 正常channel | 已关闭channel |
| :-------- | :----------- | :---------- | :------------ |
| <- ch     | 阻塞         | 成功或阻塞  | 读到零值      |
| ch <-     | 阻塞         | 成功或阻塞  | panic         |
| close(ch) | panic        | 成功        | panic         |

对于nil通道的情况，也并非完全遵循上表，**有1个特殊场景**：当`nil`的通道在`select`的某个`case`中时，这个case会阻塞，但不会造成死锁。



channel的4个特性的实现：

- channel的goroutine安全，是通过mutex实现的。
- channel的FIFO，是通过循环队列实现的。
- channel的通信：在goroutine间传递数据，是通过仅共享hchan+数据拷贝实现的。
- channel的阻塞是通过goroutine自己挂起，唤醒goroutine是通过对方goroutine唤醒实现的。

channel的其他实现：

- 发送goroutine是可以访问接收goroutine的内存空间的，接收goroutine也是可以直接访问发送goroutine的内存空间的
- 无缓冲的channel始终都是直接访问对方goroutine内存的方式，把手伸到别人的内存，把数据放到接收变量的内存，或者从发送goroutine的内存拷贝到自己内存。省掉了对方再加锁获取数据的过程。
- 接收goroutine读不到数据和发送goroutine无法写入数据时，是把自己挂起的，这就是channel的阻塞操作。阻塞的接收goroutine是由发送goroutine唤醒的，阻塞的发送goroutine是由接收goroutine唤醒的，看`gopark`、`goready`函数在`chan.go`中的调用。
- 接收goroutine当channel关闭时，读channel会得到0值，并不是channel保存了0值，而是它发现channel关闭了，把接收数据的变量的值设置为0值。
- channel的操作/调用，是通过reflect实现的，可以看reflect包的`makechan`, `chansend`, `chanrecv`函数。



```go
	ch4 := make(chan chan chan chan int)
	ch3 := make(chan chan chan int)
	ch2 := make(chan chan int)
	ch1 := make(chan int)
	ch1 <- 0
	ch2 <- ch1
	ch3 <- ch2
	ch4 <- ch3
	
	ch1 = <- ch2
	ch2 = <- ch3
	ch3 = <-ch4
```

可以定义一个 接收 channel 的channel...



我们可以通过`close`关闭一个管道来实现广播的效果，所有从关闭管道接收的操作均会收到一个零值和一个可选的失败标志。

关闭一个channel , 意味着不能向该通道发送值

在迭代 channel时,通道一定要关闭,不然不知道何时结束,程序会崩溃的

程序会一直等待其他的值发送进通道



**一个非空的通道关闭之后,通道中剩下的值仍然可以接收到**



- 重复关闭 channel 会导致 panic。
- 向关闭的 channel 发送数据会 panic。
- 从关闭的 channel 读数据不会 panic，读出 channel 中已有的数据之后再读就是 channel 类似的默认值，比如 chan int 类型的 channel 关闭之后读取到的值为 0。

可以使用 ok-idiom 方式，这种方式在 map 中比较常用。

```go
ch := make(chan int, 10)
...
close(ch)

// ok-idiom 
val, ok := <-ch
if ok == false {
    // channel closed
}
```



channel类型

1. ch := make(chan <- type)

   ch <- value

2. ch := make(<- chan type)

   <- ch
   
   

Channel之所以并发安全的原因是通过复制内存的方式来进行共享内存。





close一个从来没有传过值的channel,会往该channel中传进一个默认值(bool->false, int->0)

```go
done := make(chan bool,1)
go func() {
 for {
  select {
  case r := <-done:
   fmt.Println(r)
   return
  }
 }
}()

close(done)
// => false
```

range chan类型变量时 

一定要手动调用 close 方法，不然 range 不知道从哪里结束