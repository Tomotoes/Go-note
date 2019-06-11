# Context

Context通常被译作上下文，它是一个比较抽象的概念。

在讨论链式调用技术时也经常会提到上下文。

上下文则几乎已经成为传递与请求同生存周期变量的标准方法。

一般理解为程序单元的一个运行状态、现场、快照，而翻译中上下又很好地诠释了其本质，上下则是存在上下层的传递，上会把内容传递给下。

在Go语言中，程序单元也就指的是Goroutine。



每个Goroutine在执行之前，都要先知道程序当前的执行状态，通常将这些执行状态封装在一个Context变量中，传递给要执行的Goroutine中。



在网络编程下，当接收到一个网络请求Request，在处理这个Request的goroutine中，可能需要在当前gorutine继续开启多个新的Goroutine来获取数据与逻辑处理（例如访问数据库、RPC服务等），即一个请求Request，会需要多个Goroutine中处理。

而这些Goroutine可能需要共享Request的一些信息；

同时当Request被取消或者超时的时候，所有从这个Request创建的所有Goroutine也应该被结束。



Context的创建和调用关系是层层递进的，也就是我们通常所说的链式调用，类似数据结构里的树，从根节点开始，每一次调用就衍生一个叶子节点。

父Context 调用 cancel 方法,所有子Context的Done管道都会传进值

首先，生成根节点，使用context.Background方法生成，而后可以进行链式调用使用context包里的各类方法，context包里的所有方法：

- func Background() Context
- func TODO() Context
- func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
- func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
- func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
- func WithValue(parent Context, key, val interface{}) Context



Context就是设计用来解决那种多个goroutine处理一个Request且这多个goroutine需要共享Request的一些信息的场景



首先调用context.Background()生成根节点，然后调用withCancel方法，传入根节点，得到新的子Context以及根节点的cancel方法（通知所有子节点结束运行）

这里要注意：该方法也返回了一个Context，这是一个新的子节点，与初始传入的根节点不是同一个实例了，但是每一个子节点里会保存从最初的根节点到本节点的链路信息 ，才能实现链式。



中间每个ctx都可以通过WithValue方式传值（实现通信），而每一个子goroutine都能通过Value方法从父goroutine取值，实现协程间的通信

每个子ctx可以调用Done方法检测是否有父节点调用cancel方法通知子节点退出运行，根节点的cancel调用会沿着链路通知到每一个子节点，因此实现了强并发控制

处理Request的goroutine启动多个子goroutine大多是处理IO密集的任务如读写数据库或rpc调用，然后在主goroutine中继续执行其他逻辑



以下是一些Context使用的规范：

- 不要把Context存在一个结构体当中，显式地传入函数。Context变量需要作为第一个参数使用，一般命名为ctx；
- 即使方法允许，也不要传入一个nil的Context，如果你不确定你要用什么Context的时候传一个context.TODO；
- 使用context的Value相关方法只应该用于在程序和接口中传递的和请求相关的元数据，不要用它来传递一些可选的参数；
- 同样的Context可以用来传递到不同的goroutine中，Context在多个goroutine中是安全的；





在Go中，每个请求的request在单独的协程中进行，处理一个request也可能涉及多个协程之间的交互。一个请求衍生出的各个协程之间需要满足一定的约束关系，以实现一些诸如有效期，中止routine树，传递请求全局变量之类的功能。于是Go为我们提供一个解决方案，标准context包。使用context可以使开发者方便的在这些协程之间传递request相关的数据、取消协程的signal或截止时间等。

`context.WithTimeout()` 函数的使用，它需要两个参数：`Context` 参数和 `time.Duration` 参数。当超时到达时，`cancel()` 函数自动调用。

`context.WithDeadline()` 函数的使用，它需要两个参数：`Context` 变量和一个表示操作将要截止的时间。当期限到了，`cancel()` 函数自动调用。

- context.Deadline方法是获取设置的截止时间的意思，第一个返回值是截止时间，到了这个时间点，Context会自动发起取消请求；第二个返回值ok==false时表示没有设置截止时间，如果需要取消的话，需要调用取消函数进行取消。
- WithTimeout和WithDeadline基本上一样，这个表示是超时自动取消，是多少时间后自动取消Context的意思。