# Interface

接口类型具体描述了一系列方法的集合，一个实现了这些方法的具体类型是这个接口类型的实例。

io.Writer类型是用得最广泛的接口之一，因为它提供了所有类型的写入bytes的抽象，包括文件类型，内存缓冲区，网络链接，HTTP客户端，压缩工具，哈希等等。

io包中定义了很多其它有用的接口类型。

Reader可以代表任意可以读取bytes的类型，Closer可以是任意可以关闭的值，例如一个文件或是网络链接。

```go
package io
type Reader interface {
    Read(p []byte) (n int, err error)
}
type Closer interface {
    Close() error
}
```



再往下看，我们发现有些新的接口类型通过组合已有的接口来定义。下面是两个例子：

```go
type ReadWriter interface {
    Reader
    Writer
}
type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

上面用到的语法和结构内嵌相似，我们可以用这种方式以一个简写命名一个接口，而不用声明它所有的方法。这种方式称为接口内嵌。

尽管略失简洁，我们可以像下面这样，不使用内嵌来声明io.ReadWriter接口。

```go
type ReadWriter interface {
    Read(p []byte) (n int, err error)
    Write(p []byte) (n int, err error)
}
```

或者甚至使用一种混合的风格：

```go
type ReadWriter interface {
    Read(p []byte) (n int, err error)
    Writer
}
```

上面3种定义方式都是一样的效果。方法顺序的变化也没有影响，唯一重要的就是这个集合里面的方法。



一个类型如果拥有一个接口需要的所有方法，那么这个类型就实现了这个接口。

例如，`*os.File`类型实现了io.Reader，Writer，Closer，和ReadWriter接口。

`*bytes.Buffer`实现了Reader，Writer，和ReadWriter这些接口，但是它没有实现Closer接口因为它不具有Close方法。

Go的程序员经常会简要的把一个具体的类型描述成一个特定的接口类型。

举个例子，`*bytes.Buffer`是io.Writer；`*os.Files`是io.ReadWriter。



接口指定的规则非常简单：表达一个类型属于某个接口只要这个类型实现这个接口。所以：

```go
var w io.Writer
w = os.Stdout           // OK: *os.File has Write method
w = new(bytes.Buffer)   // OK: *bytes.Buffer has Write method
w = time.Second         // compile error: time.Duration lacks Write method

var rwc io.ReadWriteCloser
rwc = os.Stdout         // OK: *os.File has Read, Write, Close methods
rwc = new(bytes.Buffer) // compile error: *bytes.Buffer lacks Close method
```

这个规则甚至适用于等式右边本身也是一个接口类型

```go
w = rwc                 // OK: io.ReadWriteCloser has Write method
rwc = w                 // compile error: io.Writer lacks Close method
```

因为ReadWriter和ReadWriteCloser包含所有Writer的方法，所以任何实现了ReadWriter和ReadWriteCloser的类型必定也实现了Writer接口



接口中所有方法的实现是某一种类型.

此类型便实现了此接口,但是此类型可以再分为,指针类型和值类型: *V 与 V

如果是指针实现了所有方法,值类型调用就有限制了,反之亦然



interface{}被称为空接口类型是不可或缺的。因为空接口类型对实现它的类型没有要求，所以我们可以将任意一个值赋给空接口类型。

```go
var any interface{}
any = true
any = 12.34
any = "hello"
any = map[string]int{"one": 1}
any = new(bytes.Buffer)
```



因为接口与实现只依赖于判断两个类型的方法，所以没有必要定义一个具体类型和它实现的接口之间的关系。

也就是说，尝试文档化和断言这种关系几乎没有用，所以并没有通过程序强制定义。

下面的定义在编译期断言一个`*bytes.Buffer`的值实现了io.Writer接口类型:

```go
// *bytes.Buffer must satisfy io.Writer
var w io.Writer = new(bytes.Buffer)
```

因为任意`*bytes.Buffer`的值，甚至包括nil通过`(*bytes.Buffer)(nil)`进行显示的转换都实现了这个接口，所以我们不必分配一个新的变量。

并且因为我们绝不会引用变量w，我们可以使用空标识符来进行代替。

总的看，这些变化可以让我们得到一个更朴素的版本：

```go
// *bytes.Buffer must satisfy io.Writer
var _ io.Writer = (*bytes.Buffer)(nil)
```



因为时间周期标记值非常的有用，所以这个特性被构建到了flag包中；

但是我们为我们自己的数据类型定义新的标记符号是简单容易的。

我们只需要定义一个实现flag.Value接口的类型，如下：

```go
package flag

// Value is the interface to the value stored in a flag.
type Value interface {
    String() string
    Set(string) error
}
```

String方法格式化标记的值用在命令行帮组消息中；这样每一个flag.Value也是一个fmt.Stringer。

Set方法解析它的字符串参数并且更新标记变量的值。

实际上，Set方法和String是两个相反的操作，所以最好的办法就是对他们使用相同的注解方式。



```go
var name string
var age int
n, _ := fmt.Sscanf("Kim is 22 years old", "%s is %d years old", &name, &age)
fmt.Printf("%d: %s, %d\n", n, name, age)
//2: Kim, 22
```





接口类型是对其它类型行为的抽象和概括；

因为接口类型不会和特定的实现细节绑定在一起，通过这种抽象的方式我们可以让我们的函数更加灵活和更具有适应能力。

很多面向对象的语言都有相似的接口概念，但Go语言中接口类型的独特之处在于它是满足隐式实现的。



接口类型是一种抽象的类型。

当你有看到一个接口类型的值时，你不知道它是什么，唯一知道的就是可以通过它的方法来做什么。



必备思想: 接口即类型,接口是一个方法集,实现了这个方法集的类型,即成为了此接口类型

就比如常见的error类型,它就是一个接口,所有实现它error方法的类型,都可以当作error类型



为什么实现了 Stringer 接口,通过fmt.Println 打印的值就会改变

因为 fmt.Println 接收任意个接口类型

最后遍历这些接口类型,然后通过反射调用其 Stringer 接口中规定的 String方法

p.fmt.fmtS(reflect.TypeOf(arg).String())



一个接口A内 直接包含另一个接口B的名字 , 则A包含B 

实现了B的所有方法并且也实现了A的所有方法,可以让AB同时兼容