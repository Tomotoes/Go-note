# String

string ,根据索引方式输出单个元素时,默认输出 ASCII码

string ,不可以根据索引的方式修改某一元素

必须转换成 整形数组



### [strings](https://www.oyohyee.com/post/Note/learn_go/#strings)

Go中的字符串处理包

| 函数                                                     | 备注                                                         |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| `Compare(a, b string) int`                               | 比较两个字符串大小(字典序)                                   |
| `Contains(s, substr string) bool`                        | 判断一个子串是否在字符串中                                   |
| `ContainsAny(s, chars string) bool`                      | 子串中是否有字符在字符串中存在                               |
| `ContainsRune(s string, r rune) bool`                    | 判断字符是否在字符串中存在(字符以ASCII传入)                  |
| `Count(s, substr string) int`                            | 计算子串在字符串中出现的次数                                 |
| `EqualFold(s, t string) bool`                            | 比较小写的utf-8编码是否相等                                  |
| `Fields(s string) []string`                              | 按照空格将字符串分割成列表                                   |
| `FieldsFunc(s string, f func(rune) bool) []string`       | 按照函数定义的要求将字符换分割成列表                         |
| `HasPrefix(s, prefix string) bool`                       | 字符串前缀是否为子串                                         |
| `HasSuffix(s, suffix string) bool`                       | 字符串后缀是否为子串                                         |
| `Index(s, substr string) int`                            | 查找子串在字符串中的位置(不存在返回-1)                       |
| `IndexAny(s, chars string) int`                          | 查找子串中任一字符在字符串中第一次出现的位置(不存在返回-1)   |
| `IndexByte(s string, c byte) int`                        | 查找字符在字符串中第一次出现的位置(第一次出现返回-1)         |
| `IndexFunc(s string, f func(rune) bool) int`             | 找到第一个能让函数返回true的位置                             |
| `IndexRune(s string, r rune) int`                        | 找到第一个符合要求的字符的位置                               |
| `Join(a []string, sep string) string`                    | 将字符串列表合成字符串                                       |
| `LastIndex(s, substr string) int`                        | 找到最后一个符合要求的子串位置                               |
| `LastIndexAny(s, chars string) int`                      | 找到最后一个在存在于子串中的字符在字符串中的位置             |
| `LastIndexByte(s string, c byte) int`                    | 找到最后一个符合要求的字符在字符串中的位置                   |
| `LastIndexFunc(s string, f func(rune) bool) int`         | 找到最后一个能让函数返回true的位置                           |
| `Map(mapping func(rune) rune, s string) string`          | 按照函数转换每个字符串的字符                                 |
| `Repeat(s string, count int) string`                     | 将字符串重复count遍                                          |
| `Replace(s, old, new string, n int) string`              | 将字符串中的指定子串替换成别的子串                           |
| `Split(s, sep string) []string`                          | 按照特定的子串将字符串分割成字符串列表                       |
| `SplitAfter(s, sep string) []string`                     | 在指定子串后将字符串分割成字符串列表(会保留用于分隔的子串)   |
| `SplitAfterN(s, sep string, n int) []string`             | 在指定子串后将字符串分割成字符串列表(会保留用于分隔的子串,且列表长度不大于n) |
| `SplitN(s, sep string, n int) []string`                  | 使用指定子串将字符串分割成字符串列表(列表长度不大于n)        |
| `Title(s string) string`                                 | 所有单词首字母转换成标题格式(大写)                           |
| `ToLower(s string) string`                               | 将字符串转换为小写                                           |
| `ToLowerSpecial(c unicode.SpecialCase, s string) string` | 将特殊编码的字符串转换为小写                                 |
| `ToTitle(s string) string`                               | 所有单词转换成标题格式(大写)                                 |
| `ToTitleSpecial(c unicode.SpecialCase, s string) string` | 将特殊编码的字符串转换为标题格式                             |
| `ToUpper(s string) string`                               | 字符串转换为大写                                             |
| `ToUpperSpecial(c unicode.SpecialCase, s string) string` | 将特殊编码的字符串转换为大写                                 |
| `Trim(s string, cutset string) string`                   | 将字符串两端在删除集合的字符删去                             |
| `TrimFunc(s string, f func(rune) bool) string`           | 将字符串两端能将函数返回true的字符删去                       |
| `TrimLeft(s string, cutset string) string`               | 将字符串开头在删除集合内字符删去                             |
| `TrimLeftFunc(s string, f func(rune) bool) string`       | 将字符串开头能将函数返回true的字符删去                       |
| `TrimPrefix(s, prefix string) string`                    | 删除字符串前面的前缀字符串(如果有这个前缀)                   |
| `TrimRight(s string, cutset string) string`              | 将字符串结尾在删除集合内的字符删去                           |
| `TrimRightFunc(s string, f func(rune) bool) string`      | 将字符串结尾能将函数返回true的字符删去                       |
| `TrimSpace(s string) string`                             | 删除字符串两端的空白字符                                     |
| `TrimSuffix(s, suffix string) string`                    | 删除字符串后面的后缀字符串(如果有这个后缀)                   |