# Defer

defer在return,panic之后执行,多个`defer`指定的函数执行顺序是"先进后出"。

defer中的普通变量状态是当时所在作用域中的变量状态

defer中的引用变量状态是函数结束之后的变量变态

如果在defer修改函数返回声明变量,那么函数的返回值会发送变化

**使用 os.Exit 退出时 , defer函数将不会执行**





```go
type Slice []int

func NewSlice() Slice {
	return make(Slice, 0)
}
func (s *Slice) Add(elem int) *Slice {
	*s = append(*s, elem)
	fmt.Print(elem)
	return s
}
func main() {
	s := NewSlice()
	defer s.Add(1).Add(2)
	s.Add(3)
}
defer s.Add(1).Add(2)
最后一个Add(2) 就是一个直接执行函数了
so 编译时, 一定是先把 s.Add(1) 给按行执行了
defer 后面, 只把最后一个函数defer了,前置的函数都是按行执行
// s.Add(1) 会先执行 
// 结果: 132
```

