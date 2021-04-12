## 字符串处理

由于 string 类型可以看成是一种特殊的 slice 类型，因此获取长度可以用内置的函数 `len`；同时支持 切片 操作，因此，子串获取很容易

### 字符串比较

- `Compare` 函数
- `EqualFold` 函数

`Compare` 函数, `func Compare(a, b string) int`

用于比较两个字符串的大小，如果两个字符串相等，返回为 0。如果 a 小于 b ，返回 -1 ，反之返回 1 。不推荐使用这个函数，直接使用 `==` `!=` `>` `<` `>=` `<=` 等一系列运算符更加直观

`EqualFold` 函数，计算 s 与 t 忽略字母大小写后是否相等, `func EqualFold(s, t string) bool`

```
package main

import (
	"fmt"
	"strings"
)

func main() {
	a := "test1"
	b := "test2"
	fmt.Println(strings.Compare(a, b))
}
```

### 是否存在某个字符或子串

- `func Contains(s, substr string) bool`, 子串 substr 在 s 中，返回 true
- `func ContainsAny(s, chars string) bool`, chars 中任何一个 Unicode 代码点在 s 中，返回 true
- `func ContainsRune(s string, r rune) bool`, Unicode 代码点 r 在 s 中，返回 true


### 子串出现次数--字符串匹配

在数据结构与算法中，会讲解以下字符串匹配算法：
- 朴素匹配算法
- KMP 算法
- Rabin-Karp 算法
- Boyer-Moore 算法

在 Go 中，查找子串出现次数即字符串模式匹配，实现的是 `Rabin-Karp 算法`

`func Count(s, sep string) int`, Count 是计算子串在字符串中出现的次数

### 字符串分割为`[]string`

- Fields
- FieldsFunc
- Split
- SplitAfter
- SplitN
- SplitAfterN


`func Fields(s string) []string`, Fields 用一个或多个连续的空格分隔字符串 s，返回子字符串的数组（slice）。如果字符串 s 只包含空格，则返回空列表 ([]string 的长度为 0）

由于是用空格分隔，因此结果中不会含有空格或空子字符串

```
fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
```

`func FieldsFunc(s string, f func(rune) bool) []string`, 我们可以通过实现一个回调函数来指定分隔字符串 s 的字符


```
func Split(s, sep string) []string { return genSplit(s, sep, 0, -1) }
func SplitAfter(s, sep string) []string { return genSplit(s, sep, len(sep), -1) }
func SplitN(s, sep string, n int) []string { return genSplit(s, sep, 0, n) }
func SplitAfterN(s, sep string, n int) []string { return genSplit(s, sep, len(sep), n) }
```

这四个函数都是通过 sep 进行分割，返回`[]string`。如果 sep 为空，相当于分成一个个的 UTF-8 字符，如 `Split("abc","")`，得到的是`[a b c]`

Split 会将 s 中的 sep 去掉，而 SplitAfter 会保留 sep

带 n 的方法可以通过最后一个参数 n 控制返回的结果中的 slice 中的元素个数，当 `n < 0` 时，返回所有的子字符串；当 `n == 0` 时，返回的结果是 nil；当 `n > 0` 时，表示返回的 slice 中最多只有 n 个元素，其中，最后一个元素不会分割

### 字符串是否有某个前缀或后缀

