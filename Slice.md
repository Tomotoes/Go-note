# Slice

将数组转换为切片 s := arr[:]

切片容量必须大于切片长度



slice可以先后扩展,不可以向前扩展

s[i] 不可以超越len(s),向后扩展不可以超越底层数组cap(s)

```go
arr := [...]int{0,1,2,3,4,5,6,7}
s1 := arr[2:6]
s2 := s1[3:5]
```



1. 将切片 b 的元素追加到切片 a 之后：`a = append(a, b...)`

2. 复制切片 a 的元素到新的切片 b 上：

   ```
   b = make([]T, len(a))
   copy(b, a)
   ```

3. 删除位于索引 i 的元素：`a = append(a[:i], a[i+1:]...)`

4. 切除切片 a 中从索引 i 至 j 位置的元素：`a = append(a[:i], a[j:]...)`

5. 为切片 a 扩展 j 个元素长度：`a = append(a, make([]T, j)...)`

6. 在索引 i 的位置插入元素 x：`a = append(a[:i], append([]T{x}, a[i:]...)...)`

7. 在索引 i 的位置插入长度为 j 的新切片：`a = append(a[:i], append(make([]T, j), a[i:]...)...)`

8. 在索引 i 的位置插入切片 b 的所有元素：`a = append(a[:i], append(b, a[i:]...)...)`

9. 取出位于切片 a 最末尾的元素 x：`x, a = a[len(a)-1], a[:len(a)-1]`

10. 将元素 x 追加到切片 a：`a = append(a, x)`



string 可以使用切片 , string 不能根据索引的方式赋值



如果某个人写：`var slice1 []type = arr1[:]` 那么 slice1 就等于完整的 arr1 数组

（所以这种表示方式是 `arr1[0:len(arr1)]` 的一种缩写）。

另外一种表述方式是：`slice1 = &arr1`。



`arr1[2:]` 和 `arr1[2:len(arr1)]` 相同，都包含了数组从第三个到最后的所有元素。

`arr1[:3]` 和 `arr1[0:3]` 相同，包含了从第一个到第三个元素（不包括第四个）。



#### Pop/Shift

```
x, a = a[0], a[1:]
```

*一行实现 pop 出队列头。*

#### Pop Back

```
x, a = a[len(a)-1], a[:len(a)-1]
```

*一行实现 pop 出队列尾。*

#### Push

```
a = append(a, x)
```

*push x 到队列尾。*

#### Push Front/Unshift

```
a = append([]T{x}, a...)
```

*push x 到队列头。*