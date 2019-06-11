# Sort

Go的内部排序使用的是快排

### [sort](https://www.oyohyee.com/post/Note/learn_go/#sort)

Go中的sort排序是坐地排序,不会返回值,结果存放在原来的变量中

| 函数                                                         | 备注                                |
| ------------------------------------------------------------ | ----------------------------------- |
| `Float64s(a []float64)`                                      | 排序float类型                       |
| `Float64sAreSorted(a []float64) bool`                        | 返回float是否排序完毕               |
| `Ints(a []int)`                                              | 排序int类型                         |
| `IntsAreSorted(a []int) bool`                                | 返回int类型是否排序完毕             |
| `Search(n int, f func(int) bool) int`                        | 使用二分查找搜索某个值(不存在返回n) |
| `SearchFloat64s(a []float64, x float64) int`                 | 使用二分查找在float中搜索某个值     |
| `SearchInts(a []int, x int) int`                             | 使用二分查找在int中搜索某个值       |
| `SearchStrings(a []string, x string) int`                    | 使用二分查找在string中搜索某个值    |
| `Slice(slice interface{}, less func(i, j int) bool)`         | 自定义类型排序                      |
| `SliceIsSorted(slice interface{}, less func(i, j int) bool) bool` | 自定义类型是否排序完毕              |
| `SliceStable(slice interface{}, less func(i, j int) bool)`   | 自定义类型稳定排序                  |
| `Strings(a []string)`                                        | 排序strings类型                     |
| `StringsAreSorted(a []string) bool`                          | 返回string是否排序完毕              |