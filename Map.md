# Map

同时map的零值有个严重的缺陷：它可以查询，但在map中存储任何数据都有导致panic异常：

```
var m1 = map[string]string{} // empty map
var m0 map[string]string     // zero map (nil)

println(len(m1))   // outputs '0'
println(len(m0))   // outputs '0'
println(m1["foo"]) // outputs ''
println(m0["foo"]) // outputs ''
m1["foo"] = "bar"  // ok
m0["foo"] = "bar"  // panics!
```

当结构具有map字段时，就要当心了，因为在向其添加条目之前必须对其进行初始化。



不要把 map 中的键值关系简简单单理解为映射, 除了映射还可以理解为,一个数组,每个元素包括两个值

```js
const map = [{key,value},{key,value}]
map[key]=value // 保存
delete(map,key) // 删除
...
```

1. 创建map make(map[string]int)
2. 获取元素 v,ok := map[key]
3. 删除元素 delete(map,key)
4. for k,v := range map
5. len 获取元素个数
6. slice , map , function 不可以作为key
7. struct 不包含上述类型字段,也可以作为key



map 的遍历顺序不固定

