# Goruntine

Golang 的主协程 指的是 运行在 main 函数种的协程，而子协程指的是 在程序运行过程中 由主协程创建的协程。



每个协程只会有一个主协程，而子协程可能会有很多很多。



子协程和主协程在概念和内部实现上几乎没有任何区别，唯一不同在于 **它们的初始栈大小不同**



goroutine 只是由官方实现的超级“线程池”而已。



Goroutine是Go程序并发执行的最小单元，因为Goroutine不是像Unix进程那样是自治的实体，Goroutine生活在Unix进程的线程中。

在Go语言中，每一个并发的执行单元叫作一个goroutine。当一个程序启动时，其主函数即在一个单独的goroutine中运行，我们叫它main goroutine。新的goroutine会用go语句来创建。在语法上，go语句是一个普通的函数或方法调用前加上关键字go。go语句会使其语句中的函数在一个新创建的goroutine中运行。而go语句本身会迅速地完成。

当主函数返回时，所有的goroutine都会直接打断，程序退出。除了从主函数退出或者直接退出程序之外，没有其它的编程方法能够让一个goroutine来打断另一个的执行，但是我们之后可以看到，可以通过goroutine之间的通信来让一个goroutine请求请求其它的goroutine，并让其自己结束执行。



首先，goroutine **不是操作系统线程**，而是**用户空间线程**。

因此 goroutine 是由 Go runtime 来创建并管理的，而不是 OS，所以要比操作系统线程轻量级。



当然，goroutine 最终还是要运行于某个线程中的，控制 goroutine 如何运行于线程中的是 Go runtime 中的 scheduler （调度器）。

Go 的运行时调度器是 `M:N` 调度模型，既 `N` 个 goroutine，会运行于 `M` 个 OS 线程中。换句话说，一个 OS 线程中，可能会运行多个 goroutine。

Go 的 `M:N` 调度中使用了3个结构：

- `M`: OS 线程

- `G`: goroutine

  P : 调度上下文

  - `P` 拥有一个运行队列，里面是所有可以运行的 goroutine 及其上下文

要想运行一个 goroutine - `G`，那么一个线程 `M`，就必须持有一个该 goroutine 的上下文 `P`。

Goroutine是协作式抢占调度，Goroutine本身不会主动放弃CPU

goruntine 分为 main-goruntine 和 子-goruntine

他们的区别在于,初始化栈的大小

并且 main-goruntine结束 ,程序就结束了



goroutine 只是官方实现的"超级线程池"

每个 goroutine 只占用 4-5 kb 的栈内存和大幅减少创建和销毁的开销.是高并发的根本原因



goroutinue，本质上就是协程。但也存在两点不同：

1. goroutine可以实现并行，也就是说，多个协程可以在多个处理器上跑。

   而协程同一时刻只能在一个处理器上跑（把宿主语言想象成单线程就好了）。

2. goroutine之间通信是通过channel，而协程通信时通过yield和resume()操作。



默认地， Go所有的goroutines只能在一个线程里跑，也就是只使用了一个CPU核。

在同一个原生线程里，如果当前goroutine不发生阻塞，它是不会让出CPU时间给其他同线程的goroutines的，这是Go运行时对goroutine的调度，我们也可以使用runtime包来手工调度。



当一个goroutine发生阻塞，Go会自动地把与该goroutine处于同一系统线程的其他goroutines转移到另一个系统线程上去，以使这些goroutines不阻塞