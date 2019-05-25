9. withContext 封装函数
有时对于函数会有一些重复劳动，例如锁/解锁，初始化一个新的局部上下文，准备初始化变量等等……这里有一个例子：

```
func foo() {
    mu.Lock()
    defer mu.Unlock()

// foo 相关的工作

}

func bar() {
    mu.Lock()
    defer mu.Unlock()

// bar 相关的工作

}

func qux() {
    mu.Lock()
    defer mu.Unlock()

// qux 相关的工作

}
```

如果你想要修改某个内容，你需要对所有的都进行修改。如果它是一个常见的任务，那么最好创建一个叫做withContext的函数。这个函数的输入参数是另一个函数，并用调用者提供的上下文来调用它：

```
func withLockContext(fn func()) {
    mu.Lock
    defer mu.Unlock()

fn()

}
只需要将之前的函数用这个进行封装：

func foo() {
    withLockContext(func() {
        // foo 相关工作
    })
}

func bar() {
    withLockContext(func() {
        // bar 相关工作
    })
}

func qux() {
    withLockContext(func() {
        // qux 相关工作
    })
}
```


不要光想着加锁的情形。对此来说最好的用例是数据库链接。现在对 withContext 函数作一些小小的改动：

```
func withDBContext(fn func(db DB) error) error {
    // 从连接池获取一个数据库连接
    dbConn := NewDB()

return fn(dbConn)

}
```


如你所见，它获取一个连接，然后传递给提供的参数，并且在调用函数的时候返回错误。你需要做的只是：

```
func foo() {
    withDBContext(func(db *DB) error {
        // foo 相关工作
    })
}

func bar() {
    withDBContext(func(db *DB) error {
        // bar 相关工作
    })
}

func qux() {
    withDBContext(func(db *DB) error {
        // qux 相关工作
    })
}
```


你在考虑一个不同的场景，例如作一些预初始化？没问题，只需要将它们加到withDBContext就可以了。这对于测试也同样有效。

这个方法有个缺陷，它增加了缩进并且更难阅读。再次提示，永远寻找最简单的解决方案。

10. 为访问 map 增加 setter，getters
如果你重度使用 map 读写数据，那么就为其添加 getter 和 setter 吧。通过 getter 和 setter 你可以将逻辑封分别装到函数里。这里最常见的错误就是并发访问。如果你在某个 goroutein 里有这样的代码：

m["foo"] = bar
还有这个：

delete(m, "foo")
会发生什么？你们中的大多数应当已经非常熟悉这样的竞态了。简单来说这个竞态是由于 map 默认并非线程安全。不过你可以用互斥量来保护它们：

mu.Lock()
m["foo"] = "bar"
mu.Unlock()
以及：

mu.Lock()
delete(m, "foo")
mu.Unlock()
假设你在其他地方也使用这个 map。你必须把互斥量放得到处都是！然而通过 getter 和 setter 函数就可以很容易的避免这个问题：

func Put(key, value string) {
    mu.Lock()
    m[key] = value
    mu.Unlock()
}
func Delete(key string) {
    mu.Lock()
    delete(m, key)
    mu.Unlock()
}
使用接口可以对这一过程做进一步的改进。你可以将实现完全隐藏起来。只使用一个简单的、设计良好的接口，然后让包的用户使用它们：

type Storage interface {
    Delete(key string)
    Get(key string) string
    Put(key, value string)
}
这只是个例子，不过你应该能体会到。对于底层的实现使用什么都没关系。不光是使用接口本身很简单，而且还解决了暴露内部数据结构带来的大量的问题。

但是得承认，有时只是为了同时对若干个变量加锁就使用接口会有些过分。理解你的程序，并且在你需要的时候使用这些改进。