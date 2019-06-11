# Struct

假如一个结构体中的内嵌结构体实现了一个接口，那么这个结构体也认为实现了内嵌结构体的接口

我们可以直接使用内嵌结构体的变量或者方法，那么这样很自然的就实现了其接口

调用接口规定的方法时，会依据内嵌结构体调用

如果此结构体也实现了相同的接口，那么调用该方法时，会覆盖内嵌结构体的方法

除非指定 结构体.内嵌结构体类型名.方法



内嵌结构体初始化

{结构体类型名：结构体类型名{。。。}}



当T是一个类型时，方法表达式可能会写作`T.f`或者`(*T).f`，会返回一个函数"值"，这种函数会将其第一个参数用作接收器，所以可以用通常(译注：不写选择器)的方式来对其进行调用：

```go
type User struct {
	name string
}

func (u User) Say(suffix string) {
	fmt.Println(u.name + suffix)
}

func (u *User) Hello(prefix string) {
	fmt.Println(prefix + u.name)
}

func main() {
	say := User.Say
	say(User{name: "Simon"}, "!") //"Simon!"
	say(User{}, "!")              // "!
	u := User{name: "Leann"}
	say(u, "!") // Leann!

	hello := (*User).Hello //必须显式指针
	hello(&User{name: "Leann"}, "Hi! ") //Hi! Leann
}
```



当你根据一个变量来决定调用同一个类型的哪个函数时，方法表达式就显得很有用了。你可以根据选择来调用接收器各不相同的方法。

```go
type Point struct{ X, Y float64 }
func (p Point) Add(q Point) Point { return Point{p.X + q.X, p.Y + q.Y} }
func (p Point) Sub(q Point) Point { return Point{p.X - q.X, p.Y - q.Y} }
type Path []Point
func (path Path) TranslateBy(offset Point, add bool) {
    var op func(p, q Point) Point
    if add {
        op = Point.Add
    } else {
        op = Point.Sub
    }
    for i := range path {
        // Call either path[i].Add(offset) or path[i].Sub(offset).
        path[i] = op(path[i], offset)
    }
}
```





如果成员有一个是函数类型 , 外部不可实现同名的函数:

若调用对象为引用类型，函数的接收者为值类型，那么编译器就会自动的进行对象反引用，反之亦然



```go
var cache = struct {
    sync.Mutex
    mapping map[string]string
}{
    mapping: make(map[string]string),
}
```

匿名结构体!

```go
type user struct {
 name    string
 sex     bool
 address struct {
  city string
  no   int
 }
 inter interface {
  say()
 }
}

u := &user{name: "Simon", address: struct {
city string
no   int
}{city: "QHD", no: 1}, inter: nil}
fmt.Println(u.address.city)

m := map[user]string{user{}: "1"}
```