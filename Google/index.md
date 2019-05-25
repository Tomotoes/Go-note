C 语言int变量的默认值是随机值

Java int变量的默认值是null



打印字符串 , 是没有引号的 , 如果想打印带着引号的字符串, 请使用 "%q"



var 后面的类型是可选的



one := 0

one, two := 1, 2	// two 是新变量，允许 one 的重复声明。





1. bool string

2. (u)int (u)int8 (u)int16 (u)int32 (u)int64

   int 在32位os里,就是 int32

   ​    在64位os里,就是 int64

3. uintptr 指针

4. byte 字节 , 1字节

5. rune 字符型,相当于 char , 4字节 (int32)

   char 普遍都是一个字节的, 因为字符编码国际化,所以char的坑非常多

   在utf-8中,很多字节都是3字节 , 因此我们采用4字节的int32 来代表rune

6. float32 , float64(相当于double)  实数

7. complex64 (实部虚部32位) , complex128(实部虚部64位) 复数

go 的复数使用 i 

单个 i 会识别为变量,请使用`1i`



python的复数使用 j



string(value)

int(value)

bool(value)



**常量的数值可以当作任何类型使用**

```
const a,b = 3,4
var c int = int(math.Sqrt(a*a+b*b))
```



iota是自增值, 如果想跳过某个字段, 请使用 _

```go
const (
	cpp = itoa
  _
  java 
  golang
)
```



if switch 中可以定义变量,变量的作用域只在 语句中有效

switch 中会自动 break , 除非使用 fallthrough

switch 后面可以没有表达式

switch中的case 后面可以是一个表达式



go 语言函数可有的特性的**函数可变参数列表**



go语言的指针简单在 它不能运算



go语言只有值传递一种方式



```go
var arr1 [5]int
arr2 := [3]int{1, 2: 0}
arr3 := [...]int{1, 2, 4, 99: 99}
```

声明初始化数组的三种方式

请记住, go支持 针对某一索引进行初始化

```go
[...]int{1,2,3,50:2}
```



数组名不再是地址 , 指针数组修改值, 只需要按照平常方式即可

```go
func modify(arr *[3]int){
  // 对应的数组也会更改
  arr[0] = 3
}

modify(&[3]int{1,2,3})

```



slice可以先后扩展,不可以向前扩展

s[i] 不可以超越len(s),向后扩展不可以超越底层数组cap(s)

```go
arr := [...]int{0,1,2,3,4,5,6,7}
s1 := arr[2:6]
s2 := s1[3:5]
```



1. 创建map make(map[string]int)
2. 获取元素 v,ok := map[key]
3. 删除元素 delete(map,key)
4. for k,v := range map
5. len 获取元素个数
6. slice , map , function 不可以作为key
7. struct 不包含上述类型字段,也可以作为key



go 中的struct 不论是指针还是值,都用.来访问成员



坑! 成员调用方法, 该成员的值可能是 nil!!



每个目录一个包

main包包含可执行入口